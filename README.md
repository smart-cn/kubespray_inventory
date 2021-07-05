# Kubespray inventory for private dev cluster creation

## Usage:

**Kubespray installation:**
```
sudo apt-get update;  sudo apt-get -y install python3-pip
pip3 install ansible
export PATH=$PATH:$HOME/.local/bin/
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
sudo pip3 install -r requirements.txt
git clone https://github.com/smart-cn/kubespray_inventory.git inventory/mycluster
```

**Preparation on master node (for each worker node):**
```
ssh-keygen -b 4096 -q -t rsa -N '' -f ~/.ssh/id_rsa 2>&1 > /dev/null
ssh-copy-id -i ~/.ssh/id_rsa vagrant@172.16.35.10
ssh-copy-id -i ~/.ssh/id_rsa vagrant@172.16.35.11
ssh-copy-id -i ~/.ssh/id_rsa vagrant@172.16.35.12
```

**Creating cluster:**
```
ansible-playbook -i inventory/mycluster/hosts.yaml -u vagrant -b -v --private-key=~/.ssh/id_rsa cluster.yml --ask-become-pass
mkdir -p ~/.kube/
sudo cp /root/.kube/config $HOME/.kube/
sudo chown vagrant:vagrant $HOME/.kube/config
```

**K9s installation**
```
sudo wget -qO- https://github.com/derailed/k9s/releases/latest/download/k9s_Linux_x86_64.tar.gz | tar zxvf -  -C /tmp/; sudo mv /tmp/k9s /usr/local/bin
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
