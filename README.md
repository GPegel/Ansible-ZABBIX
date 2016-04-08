# Ansible-ZABBIX playbook for CentOS 7
This playbook creates an environment for running Zabbix 3.0.1

# How to run:
First of all, copy your SSH id to the targets otherwise Ansible will not run.

$ ssh-copy-id root@ip-address
Enter your password and continue.

To start the play:

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
- Zabbix Agents are not available. I guess this is Firewall related. The error message is "Received empty respone from Zabbix Agent at [127.0.0.1]. Assuming that agent dropped connection because of access permisions" (FIXED)
- For some reason are the IPTables not filled in the "Zabbix Database" role when I run the playbook for the first time on a new target. When I run the playbook for the second time on that same target than they are filled. (FIXED)
- Remove "Common" role. (FIXED)
- After enabling Host, still Zabbix is not running message appears. In CLI run 'setenforce 0', then refresh page and tadaaa... the server is running. (FIXED)
- found another possible bug: It looks like I have to run this "setsebool -P httpd_can_network_connect_db=1" on a Database server before Zabbix is able to communicate with the Database server (FIXED)
- Slave Zabbix server is not allowed to connect to the Master Database server
- Import Schema (create.sql.gz) is not working (FIXED)
- Expanding number of firewall rules. Port 80 is closed by default resulting in HTTPd not responding. The daemon is runnig but it's blocked by Firewall. (FIXED)
