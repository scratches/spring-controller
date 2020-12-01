# Running examples

This depends on a [library](https://github.com/kubernetes-client/java/wiki/1.-Installation) that is not yet in Maven Central. 
So you'll need to build it yourself. The following worked in November 2020. 

```
git clone --recursive https://github.com/kubernetes-client/java
cd java
mvn install
```

Then build the controller itself.

```
mvn spring-boot:build-image
id=$(docker create spring-controller:0.0.1-SNAPSHOT)
docker cp $id:/workspace/io.kubernetes.client.examples.SpringControllerExample target/app
docker rm -v $id > /dev/null
./target/app
```
 
The above incantation won't work if you're not using Linux, so run the controller
within the Docker image. It'll need your Kubernetes configuration file, which 
you make available to the container.

```
mvn spring-boot:build-image
id=$(docker images -aq spring-controller )
docker run -v $HOME/.kube/:/home/cnb/.kube $id 
```