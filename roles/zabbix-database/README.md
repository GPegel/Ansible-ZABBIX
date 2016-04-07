
# role: zabbix-database

This role provides a MySQL database to use with Zabbix. It will configure the schema's and create the Zabbix database users.

## Configurable parameters:

The directory to hold that database data:
```
zabbix_database_path: "/var/lib/mysql"
```

Database credentials for the Zabbix Server and Zabbix Web Server:
```
zabbix_db_name: "zabbix"
zabbix_db_user: "zabbix"
zabbix_db_password: "{{ vaulted_zabbix_db_password }}"
zabbix_db_web_user: "zabbix-web"
zabbix_db_web_password: "{{ vaulted_zabbix_db_password }}"
```

The frontend address to reach the Zabbix Database on. Useful for running in a multinode/clustered setup:
```
zabbix_db_frontend: "10.10.10.101"
```

## Related server groups:

* [zabbix_server_backends]
* [zabbix_database_backends]
* [zabbix_web_servers]

## Behaviour

MariaDB will be installed on all specified hosts in [zabbix_database_backends]. The databases will be configured in standalone mode at this time. The specified Zabbix users and passwords will be configured and the Zabbix specific schema's are loaded.

Remote access to the database will be granted for every server in [zabbix_web_servers]. Remote access for a Zabbix Server will be granted to variable 'zabbix_server_frontend'. If this parameter is not specified it will only allow access the first server in [zabbix_server_backends] to prevent multiple zabbix-servers from writing to the database simultaneously causing corruption.

In short, if you have multiple zabbix-servers, be sure to use a loadbalanced address and set the parameter 'zabbix_server_frontend' with the appropiate value.
