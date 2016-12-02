cd k8s/app/monolith
go get -u
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'

docker build -t cloudgenius/monolith-example:1.0.0     .
docker push cloudgenius/monolith-example:1.0.0



docker run -d cloudgenius/monolith-example:1.0.0
docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container name or cid>
curl <the container IP>


CID=$(docker run -d cloudgenius/monolith-example:1.0.0)
CIP=$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' ${CID})
curl $CIP
