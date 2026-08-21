K8s manifest files

mongo-config.yaml
mongo-secret.yaml
mongo.yaml
webapp.yaml

K8s commands
start Minikube and check status
minikube start --vm-driver=hyperkit 
minikube status
get minikube node's ip address
minikube ip
get basic info about k8s components
kubectl get node
kubectl get pod
kubectl get svc
kubectl get all
get extended info about components
kubectl get pod -o wide
kubectl get node -o wide
get detailed info about a specific component
kubectl describe svc {svc-name}
kubectl describe pod {pod-name}
get application logs
kubectl logs {pod-name}
stop your Minikube cluster
minikube stop


⚠️ Known issue - Minikube IP not accessible

If you can't access the NodePort service webapp with MinikubeIP:NodePort, execute the following command:
minikube service webapp-service

***
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get svc                                
NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          24h
mongo-service    ClusterIP   10.99.5.194      <none>        8080/TCP         41m
webapp-service   NodePort    10.109.246.123   <none>        3000:30100/TCP   40m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> minikube ip
192.168.49.2
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl apply -f mongo.yaml                      
deployment.apps/mongo-deployment unchanged
service/mongo-service configured
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl apply -f mongo.yaml
deployment.apps/mongo-deployment unchanged
service/mongo-service unchanged
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get pod            
NAME                                 READY   STATUS                       RESTARTS   AGE
mongo-deployment-54f4bddd58-2xkvz    1/1     Running                      0          56m
webapp-deployment-7c769cbdfd-rlgfk   0/1     CreateContainerConfigError   0          56m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get pod
NAME                                 READY   STATUS                       RESTARTS   AGE
mongo-deployment-54f4bddd58-2xkvz    1/1     Running                      0          57m
webapp-deployment-7c769cbdfd-rlgfk   0/1     CreateContainerConfigError   0          57m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl describe pod webapp-deployment-7c769cbdfd-rlgfk
Name:             webapp-deployment-7c769cbdfd-rlgfk
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Fri, 21 Aug 2026 06:16:27 +0530
Labels:           app=webapp
                  pod-template-hash=7c769cbdfd
Annotations:      <none>
Status:           Pending
IP:               10.244.0.7
IPs:
  IP:           10.244.0.7
Controlled By:  ReplicaSet/webapp-deployment-7c769cbdfd
Containers:
  webapp:
    Container ID:   
    Image:          nanajanashia/k8s-demo-app:v1.0
    Image ID:       
    Port:           3000/TCP
    Host Port:      0/TCP
    State:          Waiting
      Reason:       CreateContainerConfigError
    Ready:          False
    Restart Count:  0
    Environment:
      USER_NAME:  <set to the key 'mongo-user' in secret 'mongo-secret'>      Optional: false
      USER_PWD:   <set to the key 'mongo-password' in secret 'mongo-secret'>  Optional: false
      DB_URL:     <set to the key 'mongo-url' of config map 'mongo-cpnfig'>   Optional: false
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-gp4n2 (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       False 
  ContainersReady             False 
  PodScheduled                True 
Volumes:
  kube-api-access-gp4n2:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                    From               Message
  ----     ------     ----                   ----               -------
  Normal   Scheduled  58m                    default-scheduler  Successfully assigned default/webapp-deployment-7c769cbdfd-rlgfk to minikube
  Normal   Pulling    58m                    kubelet            spec.containers{webapp}: Pulling image "nanajanashia/k8s-demo-app:v1.0"
  Normal   Pulled     52m                    kubelet            spec.containers{webapp}: Successfully pulled image "nanajanashia/k8s-demo-app:v1.0" in 58.133s (5m27.776s including waiting). Image size: 124898678 bytes.
  Warning  Failed     2m30s (x230 over 52m)  kubelet            spec.containers{webapp}: Error: configmap "mongo-cpnfig" not found
  Normal   Pulled     2m30s (x229 over 52m)  kubelet            spec.containers{webapp}: Container image "nanajanashia/k8s-demo-app:v1.0" already present on machine and can be accessed by the pod
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl apply -f webapp.yaml                           
deployment.apps/webapp-deployment configured
service/webapp-service unchanged
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get pod                                        
NAME                                 READY   STATUS    RESTARTS   AGE
mongo-deployment-54f4bddd58-2xkvz    1/1     Running   0          60m
webapp-deployment-5766fd95c7-btfxx   1/1     Running   0          41s
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get log                                        
error: the server doesn't have a resource type "log"
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get logs
error: the server doesn't have a resource type "logs"
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get all 
NAME                                     READY   STATUS    RESTARTS   AGE
pod/mongo-deployment-54f4bddd58-2xkvz    1/1     Running   0          61m
pod/webapp-deployment-5766fd95c7-btfxx   1/1     Running   0          94s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          24h
service/mongo-service    ClusterIP   10.99.5.194      <none>        27017/TCP        61m
service/webapp-service   NodePort    10.109.246.123   <none>        3000:30100/TCP   61m

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongo-deployment    1/1     1            1           61m
deployment.apps/webapp-deployment   1/1     1            1           61m

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/mongo-deployment-54f4bddd58    1         1         1       61m
replicaset.apps/webapp-deployment-5766fd95c7   1         1         1       94s
replicaset.apps/webapp-deployment-7c769cbdfd   0         0         0       61m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get pod 
NAME                                 READY   STATUS    RESTARTS   AGE
mongo-deployment-54f4bddd58-2xkvz    1/1     Running   0          61m
webapp-deployment-5766fd95c7-btfxx   1/1     Running   0          112s
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get svc
NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          24h
mongo-service    ClusterIP   10.99.5.194      <none>        27017/TCP        63m
webapp-service   NodePort    10.109.246.123   <none>        3000:30100/TCP   63m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> minikube ip
192.168.49.2
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get pod
NAME                                 READY   STATUS    RESTARTS   AGE
mongo-deployment-54f4bddd58-2xkvz    1/1     Running   0          64m
webapp-deployment-5766fd95c7-btfxx   1/1     Running   0          4m55s
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get all
NAME                                     READY   STATUS    RESTARTS   AGE
pod/mongo-deployment-54f4bddd58-2xkvz    1/1     Running   0          67m
pod/webapp-deployment-5766fd95c7-btfxx   1/1     Running   0          7m31s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          24h
service/mongo-service    ClusterIP   10.99.5.194      <none>        27017/TCP        67m
service/webapp-service   NodePort    10.109.246.123   <none>        3000:30100/TCP   67m

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongo-deployment    1/1     1            1           67m
deployment.apps/webapp-deployment   1/1     1            1           67m

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/mongo-deployment-54f4bddd58    1         1         1       67m
replicaset.apps/webapp-deployment-5766fd95c7   1         1         1       7m31s
replicaset.apps/webapp-deployment-7c769cbdfd   0         0         0       67m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> ^C
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> minikube service webapp-service --url
http://127.0.0.1:61038
❗  Because you are using a Docker driver on windows, the terminal needs to be open to run it.
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git init                             
Initialized empty Git repository in C:/Users/VINOD KUMAR M/Desktop/K8s-Demo-Project/.git/
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        mongo-config.yaml
        mongo-secret.yaml
        mongo.yaml
        webapp.yaml

nothing added to commit but untracked files present (use "git add" to track)
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git add .
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git commit -m "kubernetis web mongo" 
[master (root-commit) a997b2d] kubernetis web mongo
 4 files changed, 109 insertions(+)
 create mode 100644 mongo-config.yaml
 create mode 100644 mongo-secret.yaml
 create mode 100644 mongo.yaml
 create mode 100644 webapp.yaml
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'origin'
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git clone https://github.com/Varun-kumarp/Kubernetes-_Webapp_With_MongoDB.git
Cloning into 'Kubernetes-_Webapp_With_MongoDB'...
warning: You appear to have cloned an empty repository.
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git init
Reinitialized existing Git repository in C:/Users/VINOD KUMAR M/Desktop/K8s-Demo-Project/.git/
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git add .                                                                    
error: 'Kubernetes-_Webapp_With_MongoDB/' does not have a commit checked out
error: unable to index file 'Kubernetes-_Webapp_With_MongoDB/'
fatal: adding files failed
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git push -u origin main                                                      
error: src refspec main does not match any
error: failed to push some refs to 'origin'
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git init
Reinitialized existing Git repository in C:/Users/VINOD KUMAR M/Desktop/K8s-Demo-Project/.git/
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git status
On branch master
nothing to commit, working tree clean
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git add .
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git remote add origin https://github.com/Varun-kumarp/Kubernetes-_Webapp_With_MongoDB.git
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git git branch -M main                                                                   
git: 'git' is not a git command. See 'git --help'.

The most similar command is
        init
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project>  git branch -M main   
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git push -u origin main                                                                  
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 1.05 KiB | 215.00 KiB/s, done.
Total 6 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/Varun-kumarp/Kubernetes-_Webapp_With_MongoDB.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git init
Reinitialized existing Git repository in C:/Users/VINOD KUMAR M/Desktop/K8s-Demo-Project/.git/
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> git pull
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 1.30 KiB | 49.00 KiB/s, done.
From https://github.com/Varun-kumarp/Kubernetes-_Webapp_With_MongoDB
   a997b2d..bb71699  main       -> origin/main
Updating a997b2d..bb71699
Fast-forward
 README.md | 34 ++++++++++++++++++++++++++++++++++
 1 file changed, 34 insertions(+)
 create mode 100644 README.md
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubetcl get svc
kubetcl : The term 'kubetcl' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a 
path was included, verify that the path is correct and try again.
At line:1 char:1
+ kubetcl get svc
+ ~~~~~~~
    + CategoryInfo          : ObjectNotFound: (kubetcl:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
 
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get svc
NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          25h
mongo-service    ClusterIP   10.99.5.194      <none>        27017/TCP        97m
webapp-service   NodePort    10.109.246.123   <none>        3000:30100/TCP   97m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get svc webapp-service
NAME             TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
webapp-service   NodePort   10.109.246.123   <none>        3000:30100/TCP   97m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get endpoint webapp-service
error: the server doesn't have a resource type "endpoint"
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> kubectl get endpoints webapp-service
Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
NAME             ENDPOINTS         AGE
webapp-service   10.244.0.8:3000   98m
PS C:\Users\VINOD KUMAR M\Desktop\K8s-Demo-Project> 
***