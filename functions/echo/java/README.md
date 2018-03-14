echo-java
===

The "Hello World" of Riff, the echo-java function takes in a string and echos it back at the user

Build and Create the Function
---
```
mvn install
mvn package
```

```
riff create --name echo-java java -a target/echo-1.0.0.jar --handler functions.Echo
```

Update the name of the container image in the function YAML file.
---
For example, the output from the line above should include something like this:
```
Successfully tagged mjeffries/echo-java:0.0.1
```

So the container image name in the echo-shell-function.yaml should be:
```
container:
    image: mjeffries/echo-java:0.0.1
```

Now create the function again, this time with the right container image name.
---
```
riff create --name echo-java java -a target/echo-1.0.0.jar --handler functions.Echo
```

Call the Function
---
If you havn't already, ensure your shell environment is setup with the Riff HTTP Gateway
```
export GATEWAY=`minikube service --url demo-riff-http-gateway`
export HEADER="Content-Type: text/plain"
```

And then call the function
```
curl $GATEWAY/requests/echo-java -H "$HEADER" -d "Hello World"
```

Which will return:
```
Echoing: Hello World
```

Alternativly, you may use the `riff publish` command, which will also POST the data over the HTTP gateway

```
riff publish --input echo-java -d "Hello World" -r
```
