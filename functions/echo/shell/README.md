echo-shell
===

The "Hello World" of Riff, the echo-shell function takes in a string and echos it back at the user

Build and Deploy the Function
---
```
riff create --name echo-shell
```

Update the name of the container image in the function YAML file.
---
For example, the output from the line above should include something like this:
```
Successfully tagged mjeffries/echo-shell:0.0.1
```

So the container image name in the echo-shell-function.yaml should be:
```
container:
    image: mjeffries/echo-shell:0.0.1
```

Now create the function again, this time with the right container image name.
---
```
riff create --name echo-shell
```

Make sure the pod is running.  
---
The STATUS should be **Running**, if it is **ImagePullBackOff**, then the container image name is likely wrong.
```
kubectl get pods
NAME                                             READY     STATUS    RESTARTS   AGE
echo-shell-7b88cf98d5-6w6b6                      2/2       Running   0          3s
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
curl $GATEWAY/requests/echo-shell -H "$HEADER" -d "Hello World"
```

Which will return:
```
Echoing: Hello World
```

Alternativly, you may use the `riff publish` command, which will also POST the data over the HTTP gateway

```
riff publish --input echo-shell -d "Hello World" -r
```
