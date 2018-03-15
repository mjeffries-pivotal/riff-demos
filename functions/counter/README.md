Couter Riff Demo
===

A small example of a Riff function that returns an increasing number with each call, using Redis on the back end.

Install the Redis Chart
---
```
helm install --name riff --set usePassword=false --set persistence.enabled=false stable/redis
```

Build and Create the Function
---
```
mvn install
mvn package
```

```
riff create java -a target/counter-1.0.0-jar-with-dependencies.jar --handler functions.Counter
```

Update the name of the container image in the function YAML file.
---
For example, the output from the line above should include something like this:
```
Successfully tagged mjeffries/counter:0.0.1
```

So the container image name in the counter-function.yaml should be:
```
container:
    image: mjeffries/counter:0.0.1
```

Now create the function again, this time with the right container image name.
---
```
riff create java -a target/counter-1.0.0-jar-with-dependencies.jar --handler functions.Counter
```

Call the Function
---
```
export GATEWAY=`minikube service --url demo-riff-http-gateway`
export HEADER="Content-Type: text/plain"

curl $GATEWAY/requests/counter -H "$HEADER"
```
