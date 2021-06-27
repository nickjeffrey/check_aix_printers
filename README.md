# check_aix_printers
nagios check for AIX print queues
By default, this script will check all print queues.  However, if a single queue name is provided as a command line parameter, only that queue will be checked.

# Requirements
perl, ssh, sudo on nagios server

# Configuration

This script is executed remotely on a monitored system by the NRPE or check_by_ssh methods available in nagios.  

If you are using the check_by_ssh method, you will need to add a section similar to the following to the services.cfg file on the nagios server.  
This assumes you already have ssh key pairs configured.
```
    define service {
       use                             generic-service
       hostgroup_name                  all_aix
       service_description             AIX print queues
       normal_check_interval           15                       ; Check every 15 minutes under normal conditions
       check_command                   check_by_ssh!/usr/local/nagios/libexec/check_aix_printers
       }
```

Alternatively, if you are using the check_nrpe method, you will need to add a section similar to the following to the services.cfg file on the nagios server.  
This assumes you already have ssh key pairs configured.
```
   define service{
      use                             generic-service
      hostgroup_name                  all_aix
      service_description             AIX print queues
      normal_check_interval           15                       ; Check every 15 minutes under normal conditions
      check_command                   check_nrpe!check_aix_printers -t 30
      notification_options            c,r                     ; Send notifications about critical and recovery events
      }
```

If you are using the NRPE method, you will also need a command definition similar to the following on each monitored host in the /usr/local/nagios/nrpe/nrpe.cfg file:
```
    command[check_aix_printers]=/usr/local/nagios/libexec/check_aix_printers
```
