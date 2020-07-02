## snap
sudo snap install microk8s --classic

## group permission
sudo usermod -a -G microk8s <username>

## .bashrc
alias mkctl="microk8s kubectl"

## create kubectl config file
mkctl config view --raw > $HOME/.kube/config

## install CoreDNS
sudo microk8s.enable dns

## some other plugins
sudo microk8s.enable dashboard
sudo microk8s.enable ingress

## troubleshooting
sudo microk8s.inspect
```
Inspecting services
  Service snap.microk8s.daemon-cluster-agent is running
  Service snap.microk8s.daemon-flanneld is running
  Service snap.microk8s.daemon-containerd is running
  Service snap.microk8s.daemon-apiserver is running
  Service snap.microk8s.daemon-apiserver-kicker is running
  Service snap.microk8s.daemon-proxy is running
  Service snap.microk8s.daemon-kubelet is running
  Service snap.microk8s.daemon-scheduler is running
  Service snap.microk8s.daemon-controller-manager is running
  Service snap.microk8s.daemon-etcd is running
  Copy service arguments to the final report tarball
Inspecting AppArmor configuration
Gathering system information
  Copy processes list to the final report tarball
  Copy snap list to the final report tarball
  Copy VM name (or none) to the final report tarball
  Copy disk usage information to the final report tarball
  Copy memory usage information to the final report tarball
  Copy server uptime to the final report tarball
  Copy current linux distribution to the final report tarball
  Copy openSSL information to the final report tarball
  Copy network configuration to the final report tarball
Inspecting kubernetes cluster
  Inspect kubernetes cluster

 WARNING:  IPtables FORWARD policy is DROP. Consider enabling traffic forwarding with: sudo iptables -P FORWARD ACCEPT 
The change can be made persistent with: sudo apt-get install iptables-persistent
WARNING:  Docker is installed. 
File "/etc/docker/daemon.json" does not exist. 
You should create it and add the following lines: 
{
    "insecure-registries" : ["localhost:32000"] 
}
and then restart docker with: sudo systemctl restart docker
Building the report tarball
  Report tarball is at /var/snap/microk8s/1489/inspection-report-20200630_092408.tar.gz
```