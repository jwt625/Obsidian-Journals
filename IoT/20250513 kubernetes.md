
2025-05-13T23:07:52-07:00
Following
- https://kubernetes.io/docs/tutorials/kubernetes-basics/


# Minikube
2025-05-13T23:08:13-07:00
https://kubernetes.io/docs/tutorials/hello-minikube/

## Installation
https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fbinary+download

```
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-darwin-arm64
sudo install minikube-darwin-arm64 /usr/local/bin/minikube

```


2025-05-13T23:10:14-07:00
Looks good:
![[Pasted image 20250513231017.png]]

## Create cluster
Something is happening:
![[Pasted image 20250513231059.png]]

2025-05-13T23:11:43-07:00
Done:
![[Pasted image 20250513231141.png]]


Dashboard:
![[Pasted image 20250513231226.png]]

2025-05-13T23:12:54-07:00
this is cool, although empty:
![[Pasted image 20250513231259.png]]



## Create a deployment

```
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
```

- `deployment.apps/hello-node created`
![[Pasted image 20250513231502.png]]

2025-05-13T23:27:11-07:00
cursor totally got it:
![[Pasted image 20250513232716.png]]

## Create a service
2025-05-13T23:30:42-07:00
![[Pasted image 20250513233042.png]]

hmm the dashboard also looks different now:
![[Pasted image 20250513233337.png]]
![[Pasted image 20250513233351.png]]

Is this making my internet slower?? Or maybe it is just the stanford bechtel.
Lol I could also see it in the docker desktop:
![[Pasted image 20250513233519.png]]

2025-05-13T23:36:30-07:00
![[Pasted image 20250513233631.png]]

## addons
2025-05-13T23:37:27-07:00

![[Pasted image 20250513233723.png]]

`metric-server` addon:
![[Pasted image 20250513233913.png]]
- hmm why is the API not available?
- ok okay just have to wait:
![[Pasted image 20250513233949.png]]


2025-05-13T23:40:55-07:00
Stopped minikube. It also updated in docker desktop:
![[Pasted image 20250513234107.png]]






