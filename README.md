# Kubespray inventory for private dev cluster creation

## Usage:

**Kubespray installation:**
```
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
sudo pip3 install -r requirements.txt
git clone https://github.com/smart-cn/kubespray_inventory.git inventory/mycluster
```

**Preparation on master node (for each worker node):**
```
ssh-copy-id -i ~/.ssh/id_rsa vagrant@host
```

**Creating cluster:**
```
ansible-playbook -i inventory/mycluster/hosts.yaml -u vagrant -b -v --private-key=~/.ssh/id_rsa cluster.yml --ask-become-pass
sudo cp /root/.kube/config /home/vagrant/.kube/
sudo chown vagrant:vagrant /home/vagrant/.kube/config
```

**Removing cluster:**
```
ansible-playbook -i inventory/mycluster/hosts.yaml -u vagrant -b -v --private-key=~/.ssh/id_rsa reset.yml --ask-become-pass
```

## 2DO:
* Looks like with the latest docker on each hardnode an iptables rule must be added (need to recheck):
```
sudo iptables -P FORWARD ACCEPT 
```
