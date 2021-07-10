# Kubespray inventory for private dev cluster creation

## Usage:

**Kubespray installation:**
```
sudo apt-get update;  sudo apt-get -y install python3-pip
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
git checkout $(git describe --tags `git rev-list --tags --max-count=1`) -q
pip3 install -r requirements.txt
export PATH=$PATH:$HOME/.local/bin/
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


**Adding admin account for the Dashboard (if required):**
```
kubectl apply -f inventory/mycluster/dashboard_admin_account.yaml
```
Getting access token:
```
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep fulladmin | awk '{print $1}')  | grep 'token:' | sed -e's/token:\| //g'
```


**Adding Dashboard to the ingress (if required)**
```
kubectl apply -f inventory/mycluster/dashboard_ingress.yaml
```

**Deploying helloworld example as the root web page and adding it to the ingress (if required):**
```
kubectl apply -f inventory/mycluster/hello_world.yaml
```

**K9s installation:**
```
sudo wget -qO- https://github.com/derailed/k9s/releases/latest/download/k9s_Linux_x86_64.tar.gz | tar zxvf -  -C /tmp/; sudo mv /tmp/k9s /usr/local/bin
```

**Removing cluster:**
```
ansible-playbook -i inventory/mycluster/hosts.yaml -u vagrant -b -v --private-key=~/.ssh/id_rsa reset.yml --ask-become-pass
```

