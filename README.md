# How To Use minikube for Local Kubernetes Development and Testing

#Installing and Running Minikube:
1) Windows
```shell
brew install minikube 
```

2) MacOS
```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```


To begin using minikube, you can run it with the start command, which will automatically create a local Kubernetes cluster using multiple Docker containers and a recent stable version of Kubernetes.

```shell
minikube start
```

Installing minikube via Homebrew also provided kubectl, the primary tool for managing Kubernetes clusters via the command line. You can now run kubectl get as you would with any other Kubernetes cluster to list all of the pods that are running in your cluster:

```shell
kubectl get pods -A
```

## PART 2 -  Accessing the Kubernetes Dashboard
If you deployed Minikube locally, you can access the dashboard by running the minikube dashboard command:
```shell
minikube dashboard
```
This command will automatically start the dashboard, forward a port from inside of your Kubernetes cluster so that you can access it directly, and open a web browser pointed to that local port.

If you’re running minikube on a remote server where you can’t easily access a web browser, you can run minikube dashboard with the --url option appended.

```shell
minikube  dasboard --url
```
Note the port number that was returned by this command, as it will be different on your system.

However, Kubernetes’ default security configuration will prevent this URL from being accessible on a remote machine. You will need to create an SSH tunnel to access the dashboard URL. To create a tunnel from your local machine to your server, run ssh with the -L flag. Provide the port number that you noted from the forwarding process output along with the IP address of your remote server:

```shell
ssh -L 50513:127.0.0.1:50513 nayan@your_server_ip
```
You should then be able to access the dashboard in a browser at `http://127.0.0.1:50513/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/`.

## PART3: Deploying and Testing a Sample App

You can use the kubectl command to deploy a test application to your Minikube cluster. The following command will retrieve and deploy a sample Kubernetes application – in this case, Google’s hello-app.

```shell
kubectl create deployment web --image=gcr.io/google.samples/hello-app:1.0
```
This command creates a deployment, which you are calling web inside your cluster, from a remote image called hello-app on gcr.io, Google’s container registry.

Next, expose the web deployment as a Kubernetes Service, specifying a static port where it will be accessible with --type=NodePort and --port=8080:

```shell
kubectl expose deployment web --type-NodePort --port=8080
```

```shell
kubectl get service web
```

```shell
minikube service web --url
```

```shell
curl http://127.0.0.1:50513
```

## PART 4 - Managing Minikube's Resources and Filesyatem

```shell
minkube config set memory 4096
```

```shell
minkube delete
```

```shell
minkube start
```

```shell
minikube mount $HOME:/host
```
