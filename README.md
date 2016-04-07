# Ansible-ZABBIX playbook for CentOS 7
# How to run:
$ ansible-playbook choose-your-yaml-file.yml -i environments/production -e env=environments/production -u root

#Please note:
If you get any error messages saying something about undefined variables then have a look at this: ['ansible_enp0s8']['ipv4']['address'] As you can see I've used 'enp0s8' instead of eth0 because I've tested this script in my own local virtual environment within VirtualBox. So please change it back to whatever NIC your IP addresses of your hosts are listening to.

#To-do list:
- Import Schema (create.sql.gz) is not working
- Expanding number of firewall rules. Port 80 is closed by default.
