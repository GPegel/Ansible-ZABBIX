# Ansible-ZABBIX playbook for CentOS 7
This playbook creates an environment for running Zabbix 3.0.1

# How to run:
$ ansible-playbook choose-your-yaml-file.yml -i environments/production -e env=environments/production -u root

# Post-installation:
Open a web browser and enter the IP adres of one of the Zabbix Web Servers like : http://127.0.0.1/Zabbix
Enter the standard Zabbix user credentials as in:
- username: Admin
- password: zabbix

Do not forget to enable the Zabbix Host in the GUI:
- Configuration -> Hosts -> Click 'Disabled' at Status so it enables

#Please note:
If you get any error messages saying something about undefined variables then have a look at this: ['ansible_enp0s8']['ipv4']['address'] As you can see I've used 'enp0s8' instead of eth0 because I've tested this script in my own local virtual environment within VirtualBox. So please change it back to whatever NIC your IP addresses of your hosts are listening to.

#To-do list:
- found another possible bug: It looks like I have to run this "setsebool -P httpd_can_network_connect_db=1" on a Database server before Zabbix is able to communicate with the Database server
- Slave Zabbix server is not allowed to connect to the Master Database server
- Import Schema (create.sql.gz) is not working (FIXED)
- Expanding number of firewall rules. Port 80 is closed by default resulting in HTTPd not responding. The daemon is runnig but it's blocked by Firewall. (FIXED)
