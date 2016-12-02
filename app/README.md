# App

A sample 12 Factor Application.

## Usage

Generate TLS certificates:

```
go run certgen/main.go
```
```
wrote ca.pem
wrote ca-key.pem
wrote server.pem
wrote server-key.pem
```

#### Install Go

    cd
    wget https://storage.googleapis.com/golang/go1.7.4.linux-amd64.tar.gz
    rm -rf /usr/local/bin/go
    sudo tar -C /usr/local -xzf go1.7.4.linux-amd64.tar.gz
    export PATH=$PATH:/usr/local/go/bin
    export GOPATH=~/go

#### Get the app code

    mkdir -p $GOPATH/src/github.com/beacloudgenius
    cd $GOPATH/src/github.com/beacloudgenius
    git clone https://github.com/beacloudgenius/k8s.git

#### Build a static binary of the monolith app

    cd k8s/app/monolith
    go get -u
    go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'

#### Run monolith locally

    cd k8s/app/monolith
    ./monolith -http :5080 -health :5081
    2016/12/01 18:49:58 Starting server...
    2016/12/01 18:49:58 Health service listening on 0.0.0.0:5081
    2016/12/01 18:49:58 HTTP service listening on 0.0.0.0:5080
    127.0.0.1:54777 - - [Thu, 01 Dec 2016 18:52:20 PST] "GET /secret HTTP/1.1" curl/7.43.0
    127.0.0.1:54779 - - [Thu, 01 Dec 2016 18:52:31 PST] "GET /login HTTP/1.1" curl/7.43.0
    127.0.0.1:54787 - - [Thu, 01 Dec 2016 18:53:26 PST] "GET /login HTTP/1.1" curl/7.43.0
    127.0.0.1:54789 - - [Thu, 01 Dec 2016 18:53:31 PST] "GET /secret HTTP/1.1" curl/7.43.0
    127.0.0.1:54796 - - [Thu, 01 Dec 2016 18:53:58 PST] "GET / HTTP/1.1" curl/7.43.0

The password is `password`

#### Test with cURL

```
curl --cacert ./ca.pem http://localhost:5080                                        
{"message":"Hello"}

curl --cacert ./ca.pem http://localhost:5080/secure
authorization failed

>>> type `password` at the prompt

curl --cacert ./ca.pem http://localhost:5080/login -u user
Enter host password for user 'user':
{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVzZXJAZXhhbXBsZS5jb20iLCJleHAiOjE0ODA5MDg1MDAsImlhdCI6MTQ4MDY0OTMwMCwiaXNzIjoiYXV0aC5zZXJ2aWNlIiwic3ViIjoidXNlciJ9.S5Kap0pzhS9G4kOcEhwhTh1HuzKyBic1QbNsbLt188E"}

TOKEN=$(curl --cacert ./ca.pem http://localhost:5080/login -u user | jq -r '.token')
Enter host password for user 'user':
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   222  100   222    0     0   2393      0 --:--:-- --:--:-- --:--:--  2439

echo $TOKEN
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InVzZXJAZXhhbXBsZS5jb20iLCJleHAiOjE0ODA5MDg1MjUsImlhdCI6MTQ4MDY0OTMyNSwiaXNzIjoiYXV0aC5zZXJ2aWNlIiwic3ViIjoidXNlciJ9.JQIsbDRxxai1nxlYjGLfsW6V_Pe19kchJpE0PGP4Z-A

curl --cacert ./ca.pem -H "Authorization: Bearer $TOKEN" http://localhost:5080/secure
{"message":"Hello"}

curl --cacert ./ca.pem http://localhost:5080/secure
authorization failed

```


### Distributed microservices hello and auth


SHELL 1
```
cd k8s/app/

go build -o ./h ./hello
./h -http :5080 -health :5081
```
SHELL 2
```
cd k8s/app/

go build -o ./a ./auth
./a -http :5090 -health :5091
```

SHELL 3
```
TOKEN=$(curl --cacert ./ca.pem 127.0.0.1:5090/login -u user | jq -r '.token')
Enter host password for user 'user':
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   222  100   222    0     0   3099      0 --:--:-- --:--:-- --:--:--  3126

curl --cacert ./ca.pem -H "Authorization:  Bearer $TOKEN" http://127.0.0.1:5080/secure
{"message":"Hello"}

curl --cacert ./ca.pem http://127.0.0.1:5080/secure
authorization failed
```
