
```shell
kubeadm-init
apt-get install kubeadm
sudo apt-get install kubeadm
kubeadm
kubeadm --init
sudo kubeadm init
sudo apt-get update
sudo apt-get upgrade
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
sudo tee /etc/modules-load.d/containerd.conf <<EOF
overlay
br_netfilter
EOF
sudo modprobe overlay
sudo modprobe br_netfilter
sudo tee /etc/sysctl.d/kubernetes.conf <<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
sudo sysctl --system
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update
sudo apt install -y containerd.io
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
sudo systemctl enable containerd
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml
kubectl get nodes
sudo ufw allow 6443
git clone https://github.com/skynet86/hello-world-k8s.git
ls
cd hello-world-k8s/
cat hello-world.yaml
clear
kubectl create -f hello-world.yaml
kubectl get all
kubectl describe pod hello-world-deployment-994cf46b-668z5
nano taint-fix.yaml
kubectl apply -f taint-fix.yaml
kubectl describe pod pod/hello-world-deployment-994cf46b-lg76x
kubectl describe pod/hello-world-deployment-994cf46b-lg76x
kubectl describe node <nodename> | grep Taints
kubectl describe node nahid-hp-elitebook-830-g6 | grep Taints
kubectl describe node workder01 | grep Taints
kubectl describe node worker01 | grep Taints
kubectl taint node nahid-hp-elitebook-830-g6 node-role.kubernetes.io/control-plane:NoSchedule
cat taint-fix.yaml
kubectl taint nodes worker01  node.kubernetes.io/disk-pressure:NoSchedule
history
sudo nano hello-world.yaml
history | grep decribe
history | grep describe
kubectl apply -f hello-world.yaml
pwd
kubectl get pods all
kubectl delete -f hello-world.yaml
kubectl delete pod -A --field-selector 'status.phase==Failed'
kubectl describe hello-world-deployment-994cf46b-b2mxf
kubectl describe pod hello-world-deployment-994cf46b-b2mxf
kubectl describe node workder01
kubectl describe node worker01
kubectl describe node nahid-hp-elitebook-830-g6
kubectl describe node worker01 | grep taint
kubectl taint nodes work01 node.kubernetes.io/disk-pressure:NoSchedule-
kubectl taint nodes worker01 node.kubernetes.io/disk-pressure:NoSchedule-
kubectl describe node worker01 | grep -i taints
# Remove all stopped containers
docker container prune -f
# Remove all unused images
docker image prune -a -f
# Remove all unused volumes
docker volume prune -f
# Remove all unused networks
docker network prune -f
ssh -L 192.168.0.109:8001:127.0.0.1:8001 nahid@192.168.0.109
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
kubectl get pods -A
kubectl describe pod kubernetes-dashboard
kubectl describe pod dashboard-metrics-scraper-687dbbb9c4-bwwlx
kubectl get pod
kubectl get nodeds
sudo lsof | grep REG | grep -v "stat: No such file or directory" | grep -v DEL | awk '{if ($NF=="(deleted)") {x=3;y=1} else {x=2;y=0}; {print $(NF-x) "  " $(NF-y) } }'  | sort -n -u  | numfmt  --field=1 --to=iec
cd workspace/
cd ..
df -h
df -i
sudo apt-get clean
sudo apt-get autoremove --purge
sudo du -sh /var/*
sudo du -sh /home/*
sudo du -sh /*
sudo du -sh /home/* | sort -rh | head -n 10
sudo du -sh /root/* | sort -rh | head -n 10
sudo du -sh /usr/* | sort -rh | head -n 10
docker system prune -a --volumes
kubectl get pods --all-namespaces --field-selector=status.phase=Failed
kubectl get pods --all-namespaces --field-selector=status.phase=Failed -o jsonpath='{range .items[*]}{.metadata.namespace}{" "}{.metadata.name}{"\n"}{end}' | while read namespace pod; do kubectl delete pod $pod -n $namespace; done
kubectl describe pod hello-world-deployment-994cf46b-h2x49
sudo du -sh /var/log/* | sort -rh | head -n 20
sudo du -sh /tmp/* | sort -rh | head -n 20
sudo kubectl taint nodes worker01 node.kubernetes.io/disk-pressure:NoSchedule-
history | grep apply
kubectl top nodes
kubectl get events
history | grep delete
exit
```
