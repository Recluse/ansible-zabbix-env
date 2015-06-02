# zabbix-environment

This Ansible playbook configures a entire Zabbix environement (server, proxy and agents).         
As frontend, you can choose Nginx or Apache. As backend, MySQL, PGSQL and SQLite are supported.         

### Operational System

This module works on CentOS 7.

### General Configuration

  * To setup a full environment, you must select just ONE server, ONE proxy and ONE or MANY agents. The agents may point to the server or proxy.         
  * In the `hosts` file, configure the address of each one.              
  * The server frontend is totally switchable, you can select Nginx, run the playbook, change to Apache, run again and it will load normally. The service is not removed, just stopped. The server will be access from the address: `http://serveraddres/zabbix`.         
  * Databases are also switchable, but no data are exported/imported on the switch. The services are not stopped, just the server/proxy configuration are changed.              
  * Once the database is installed, it won't try to load the SQL files again. This way, no data is overridden.         
  * I recommend running this setup in a fresh environment (without database/frontend configuration). It can be buggy if tried to run on an existent front/backend.              

### Variables Reference

All the basic parameters are stored in variables files, localized in `group_vars/*`.     


**group_vars/all**
  * `zabbix_version`: version of Zabbix Repository and installation. Versions 2.2 and 2.4 are working.
  * `serverip`: IP address to the zabbix-server choosed. Used to configure agents/proxy.
  * `proxyip`: IP address to the zabbix-proxy choosed. Used to configure agents.
  * `point_agent_to`: Where should the zabbix-agent be pointed to? Some agents may be pointed to the proxy and others direct to the server, so proxy and agent are accepted values. You can override this to a specifc agent in `hosts_vars/<specific-agent>`. 


**group_vars/proxy or group_vars/server**
  * `_installfrontend`: True if need frontend installation on the server. If False, just installs the server and its configuration files.
  * `_frontend`: apache or nginx, select the frontend to install. This setting is switchable between runs. 
  * `_db`: mysql, pgsql or sqlite3 (last one just for the proxy), select the backend. This setting is switchable, but no data is migrated between runs.
  * `_installdb`: True if database installation is needed. If False, zabbix-server will use the specified information about the database but won't install it.
  * `_loaddb`: if True, will configure the database (users, load sql files).
  * `_db*`: configure the database user and password name. For the sqlite3, _dbname is the path do the .db file, hardcoded in the playbook.
  * `_timezone`: timezone defined in php.ini and php-fpm.
  
  * `pgsql_version`: version used in the Postgres repository.
  * `pgsql_changeroot`: will change the root (postgres) password role. Needed if installing/loading the database.
  * `pgsql_postgrespass`: password to set in the postgres role.

  * `mysql_changeroot:` will change the root password. Needed if installing/loading the database.
  * `mysql_rootpass`: password to set in the mysql root.

### More Info

  * A lot of specific zabbix-{server,proxy,agent} are hardcoded in the templates. If you may edit it to variables, I would appreciate a pull request to incorporate the modifications.         
  * I tested this environment in VMs and Vagrant and worked fine. I tried to make it most idempotent possible, but some changs won't work, like changing the postgress/root password.         
  * After the run, is needed to log in the zabbix-server interface and add the proxy/agents. Login: Admin / Pass: zabbix. The defaults.         
  * Any bug, error or improvement, you can reach me at Github or jonatas.baldin at gmail dot com.         
