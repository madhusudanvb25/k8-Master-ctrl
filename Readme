CREATE 3 ec2 t3.medium machine with all traffic enabled

In Master machine: 
sudo su
wget https://raw.githubusercontent.com/akshu20791/Deployment-script/main/k8s-master.sh
ls -l
ls
chmod 777 k8s-master.sh
ls -l
./k8s-master.sh


################# IN the nodes ###############################
sudo su (if not done)
 wget https://raw.githubusercontent.com/akshu20791/Deployment-script/main/k8s-nodes.sh
ls
ls -l
chmod 777 k8s-nodes.sh
ls -l
./k8s-nodes.sh


###########################################################################

RUN THE COMMAND IN NODES:

modprobe br_netfilter
echo 1 > /proc/sys/net/bridge/bridge-nf-call-iptables
echo 1 > /proc/sys/net/ipv4/ip_forward

###

After k8s installation is done . we need to connect nodes with the master via tokens 

### generate token in master:  (run below command in k8s master) 

kubeadm token create --print-join-command 

## whatever token comes up ...copy the token and paste it in notepad and after that in the command you will see 6443 written ...after that paste --cri-socket unix:///var/run/cri-dockerd.sock


EXAMPLE: YOUR TOKEN SHOULD LOOK LIKE THIS (REMEMBER NOT TO COPY BELOW TOKEN ITS NOT YOURS)

kubeadm join 172.31.17.126:6443 --cri-socket unix:///var/run/cri-dockerd.sock --token 5fh9ia.7dqyttb7tvzzarg6 --discovery-token-ca-cert-hash sha256:47a2d8b3157d6cee55109aa3a37887d031a24128de05732b0c168fe9da62733e 


################

Now paste the tokens in the nodes one by one 

#################


######  Single con pod   ####### 

vi mypod1.yml

apiVersion: v1
kind: Pod
metadata:
  name: my-first-pod
spec:
  containers:
    - name: apachecon
      image: httpd
      ports:
        - containerPort: 80


kubectl apply -f pod1.yml

kubectl get pods

kubectl get pods -o wide



#######  Replica set   #########

vi myreplica.yml

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myreplica
spec:
  replicas: 5
  selector:
    matchLabels:
      company: staragile
  template:
    metadata:
      labels:
        company: staragile
    spec:
      containers:
        - name: c01
          image: httpd
          ports:
            - containerPort: 80

kubectl apply -f myreplica.yml

kubectl get pods #give me the list of the pods

kubectl get pods -o wide #give me the list of the pods which also tells in which node the pods is created

kubectl get pods --show-labels

kubectl delete pod <pod>

kubectl get pods #you will see the deleted pod is replaced by otrher pod

kubectl scale --replicas=10 -f myreplica.yml

(it will increase the replicas to 10)


kubectl scale --replicas=1 -f myreplica.yml


kubectl get rs #this will give you the list of the replicaset


kubectl delete rs myreplica #this will delete your replicaset


kubectl get pods #the pods are not deleted




#### Let’s now add versioning and rollback to your deployment step by step.  ####
✅ Step 1: Your v1 Deployment (mydeploy-v1.yml)

vi mydeploy-v1.yml


apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydep
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: busybox-container
          image: ubuntu
          command: ["/bin/sh", "-c", "while true; do echo Hello from v1; sleep 5; done"]

kubectl apply -f mydeploy-v1.yml
kubectl get deploy
kubectl get pods
kubectl logs -f <pod-name>


✅ Step 2: Create v2 Deployment (mydeploy-v2.yml)
vi mydeploy-v2.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydep
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: busybox-container
          image: ubuntu
          command: ["/bin/sh", "-c", "while true; do echo Hello from v1; sleep 5; done"]


kubectl apply -f mydeploy-v2.yml
kubectl get pods
kubectl logs -f <new-pod-name>


✅ Step 3: Rollback to v1

kubectl rollout undo deployment/mydep
kubectl get pods
kubectl logs -f <rolled-back-pod-name>


You should now see:
Hello from v1

🧠 Bonus: Check rollout history

kubectl rollout history deployment/mydep


