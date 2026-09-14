# Kubernetes Multi-Container Pod Demo

A hands-on Kubernetes project demonstrating how multiple containers can run inside the same Pod and share data using an `emptyDir` volume.

## Architecture

```text
                Kubernetes Pod
                     |
          +----------+----------+
          |                     |
     Nginx Container       Ubuntu Container
          |                     |
          +----------+----------+
                     |
              Shared emptyDir
                     |
                index.html
```

## What This Project Demonstrates

* Multi-Container Pods
* Shared Volumes
* `emptyDir`
* `volumeMounts`
* Container cooperation
* Kubernetes YAML manifests
* `kubectl exec`
* `kubectl port-forward`

## How It Works

The Ubuntu container creates an `index.html` file inside:

```text
/pod-area/index.html
```

Both containers share the same `emptyDir` volume.

The Nginx container mounts the same volume at:

```text
/usr/share/nginx/html
```

Therefore, Nginx can serve the file created by the Ubuntu container.

## Deploy

```bash
kubectl apply -f multicontainer-pod.yaml
```

## Check the Pod

```bash
kubectl get pods
```

Expected:

```text
two-container-pod   2/2   Running
```

## Test the Shared File

```bash
kubectl exec two-container-pod -c nginx-container -- cat /usr/share/nginx/html/index.html
```

Expected:

```text
Hello from Zeyad's Multi-Container Pod!
```

## Access Through Nginx

```bash
kubectl port-forward pod/two-container-pod 8080:80
```

Then:

```bash
curl http://localhost:8080
```

Expected:

```text
Hello from Zeyad's Multi-Container Pod!
```

## Cleanup

```bash
kubectl delete -f multicontainer-pod.yaml
```

## Key Kubernetes Concept

M
