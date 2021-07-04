# kubespray_inventory

**Usage:**
(for every worker node, run on master node): ssh-copy-id -i ~/.ssh/id_rsa vagrant@host
cd kubespray
ansible-playbook -i inventory/mycluster/hosts.yaml -u vagrant -b -v --private-key=~/.ssh/id_rsa cluster.yml --ask-become-pass
ansible-playbook -i inventory/mycluster/hosts.yaml -u vagrant -b -v --private-key=~/.ssh/id_rsa reset.yml --ask-become-pass

**2DO:**
Looks like with the latest docker on each hardnode an iptables rule must be added (need to recheck):
sudo iptables -P FORWARD ACCEPT 
