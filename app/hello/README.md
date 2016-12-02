    cd k8s/app/hello
    go get -u
    go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'

    docker build -t cloudgenius/hello-example:1.0.0     .
    docker push cloudgenius/hello-example:1.0.0

    CID3=$(docker run -d cloudgenius/hello-example:1.0.0)

    CIP=$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' ${CID3})
    curl $CIP
