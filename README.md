# ossim-config
config files that i use to constomize my SIEM

# crontab for autoupdates

SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

###m h  dom mon dow   command
`0 */6 * * * /usr/bin/alienvault-update -q --feed > /dev/null 2>&1`

# mail aliases
`echo "monit: root" >> /etc/aliases`
`echo "root: root@cryp7.net" >> /etc/aliases`

# OSSEC authd
Reference: http://ossec-docs.readthedocs.io/en/latest/programs/ossec-authd.html

## OSSEC server config

### Certificate Generation

`openssl genrsa -out /var/ossec/etc/sslmanager.key 2048`
`openssl req -new -x509 -key /var/ossec/etc/sslmanager.key -out /var/ossec/etc/sslmanager.cert -days 365`

Note: There is no authentication or authorization with ossec-authd, so it's recommended to not run it all the time. However, given the situation and the need to automate this registration process, it may be worthwhile to create an upstart or systemd job to start the ossec-authd daemon on USM.

### Firewall Configuration
Add the following line to /etc/ossim/firewall_include
`-A INPUT -p tcp --dport 1515 -j ACCEPT`

Then run `ossim-reconfig`

## Linux Client Registration
`/var/ossec/bin/agent-auth -m x.x.x.x -p 1515`
where x.x.x.x is the OSSEC Server IP (USM server or remote sensor IP)


## Windows Client Registration
Run the agent-auth.exe Windows package from the OSSEC Agent Installation directory.
Agent-auth.exe –m x.x.x.x –p 1515

NOTE: The OSSEC Agent service on the client needs to be manually or programmatically started after this is run
