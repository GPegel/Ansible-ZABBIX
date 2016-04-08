# Ansible-ZABBIX playbook for CentOS 7

This playbook creates an environment for running Zabbix 3.0.1 on Apache.

The following roles will be installed:
1. Apache
2. Firewall
3. HTOP
4. Pagerduty-Agent
5. Zabbix

Each role is configured to copy automatically (configuration) files when needed. If you encounter some strange behavior, then edit the configuration files in Ansible and run the playbook again. Usually the files are located in "roles/role-name/templates".

# Before running the playbook:

1. Start the servers who are going to receive the roles.
2. add the IP addresses of the servers in the 'inventory' file.
3. copy your SSH id to the targets otherwise Ansible will not run.

```
$ ssh-copy-id root@ip-address
```

# Start running the playbook:

```
$ ansible-playbook site.yml -i environments/production -e env=environments/production -u root
```

# Post-installation:
Open a web browser and enter the IP address of one of the Zabbix Web Servers like : http://127.0.0.1/Zabbix
Enter the standard Zabbix user credentials as in:
- user name: Admin
- password: zabbix

Do not forget to enable the Zabbix Host in the GUI:
- Configuration -> Hosts -> Click 'Disabled' at Status so it enables

# Please note:
If you get any error messages saying something about undefined variables then have a look at this: ['ansible_enp0s8']['ipv4']['address'] As you can see I've used 'enp0s8' instead of eth0 because I've tested this script in my own local virtual environment within VirtualBox. So please change it back to whatever NIC your IP addresses of your hosts are listening to.

# To-do list:
- Zabbix Agents are not available. I guess this is Firewall related. The error message is "Received empty respone from Zabbix Agent at [127.0.0.1]. Assuming that agent dropped connection because of access permissions" (FIXED)
- For some reason are the IPTables not filled in the "Zabbix Database" role when I run the playbook for the first time on a new target. When I run the playbook for the second time on that same target than they are filled. (FIXED)
- Remove "Common" role. (FIXED)
- After enabling Host, still Zabbix is not running message appears. In CLI run 'setenforce 0', then refresh page and tadaaa... the server is running. (FIXED)
- found another possible bug: It looks like I have to run this "setsebool -P httpd_can_network_connect_db=1" on a Database server before Zabbix is able to communicate with the Database server (FIXED)
- Slave Zabbix server is not allowed to connect to the Master Database server (FIXED)
- Import Schema (create.sql.gz) is not working (FIXED)
- Expanding number of firewall rules. Port 80 is closed by default resulting in HTTPd not responding. The daemon is running but it's blocked by Firewall. (FIXED)
