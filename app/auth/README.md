cd k8s/app/auth
go get -u
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'

docker build -t cloudgenius/auth-example:1.0.0     .
docker push cloudgenius/auth-example:1.0.0

CID2=$(docker run -d cloudgenius/auth-example:1.0.0)

CIP=$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' ${CID2})
curl $CIP
