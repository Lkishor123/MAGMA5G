# MAGMA5G

## Setup:
---------------------------------------------------------

1. `git clone https://github.com/Lkishor123/MAGMA5G.git`

2. `cd MAGMA5G/Vagrant`

3. `vagrant up magma5g`

4. `vagrant ssh magma5g`

5. Change the name of the interfaces on the VM

 - `sudo su`
 - `sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"/g' /etc/default/grub`
 - `sed -i 's/enp0s3/eth0/g' /etc/netplan/50-cloud-init.yaml`
 - `grub-mkconfig -o /boot/grub/grub.cfg`

6. Reload or restart the machine: `vagrant reload magma5g`, Login and check if the names are changed correctly to: `eth0/eth1/eth2`

7. Package installation on VM: `vagrant ssh magma5g`
  
  - `sudo su`
  - `wget https://raw.githubusercontent.com/magma/magma/master/lte/gateway/deploy/agw_install_ubuntu.sh`
  - `bash agw_install_ubuntu.sh`

8. Inside VM:

- `cd MAGMA5G/deploy`
- `ansible-playbook local_deploy.yml`
- `sudo apt update`
- `sudo apt install magma` : To update to latest focal-ci build


## Verification steps on VM
-----------------------------------------------------------

1. For GNB Connectivity :

- `cat /proc/net/sctp/proc`
- `cat /proc/net/sctp/assocs`
 
2. Additional commands 

- `sudo ovs-ofctl dump-flows gtp_br0 table=20`
- `sudo ovs-ofctl dump-flows gtp_br0 table=0`


## Common Issues
---------------------------------------------------------

1. Logs 
  - `tail -F /var/log/syslog`
  - `tail -F /var/log/mme.log`

2. In case if their are existing session in redis (fail to establish PDU)
  - `redis-cli -p 6380 FLUSHALL`
  - `redis-cli -p 6380`
  - `hgetall sessiond:sessions`
