[![CircleCI](https://circleci.com/gh/CoffeeITWorks/ansible_burp_reports.svg?style=svg)](https://circleci.com/gh/CoffeeITWorks/ansible_burp_reports)

Getting Started
================

Check the documentation added in: 

https://github.com/CoffeeITWorks/ansible-generic-help#getting-started


Role Name
=========

ansible_burp_reports deploy and maintenance role.

This roles builds burp_reports version specified on defaults/main.yml. 
Also configures it to get it working and maintained in a centralized way.


Requirements
--------------

Burp-ui working and var defined in your group_vars/group
Recommended: ansible_burpui_server and ansible_burp2_server from CoffeeITWorks: https://github.com/CoffeeITWorks

Variables
---------

Try to not edit the role only for vars, all configurable vars are in defaults/main.yml so all of them can
be set on group_vars/group or host_vars/host files.

First required:

    burpui_apiurl: http://admin:admin@localhost:5000/api/


Not really required but you can customize all the following: 

```yaml
    burp_report_days_outdated: 25
    burp_report_conf: /etc/burp/burp-reports.conf

    burp_conf_user: 'root'
    burp_conf_group: 'root'

    # https://github.com/pablodav/burp_server_reports#usage
    burp_report_inventory_input: None
    burp_report_inventory_output: None # could be for example /var/www/html/inventory_central.csv

    # Tests tasks
    burp_report_test: false

    # http://docs.ansible.com/ansible/cron_module.html
    report_outdated_special_time: 'weekly'
    report_inventory_special_time: 'weekly'

    burp_report_emails_notes: 'some useful notes to change'
    burp_report_emails_to: root@localhost
    burp_report_emails_from: burpserver@localhost
    burp_excluded_clients: 'agent,monitor'

    smtp_relay: 'localhost'
    smtp_port: 25
    smtp_mode: 'normal'
    smtp_login: ''
    smtp_password: ''

    ## burp-reports installation packages: 
    # doc: https://github.com/pablodav/burp_server_reports

    # This example is the variable that setups the burp_reports version:
    burp_reports_pip_packages:
    - name: burp_reports
        version: 1.2.4
```

Optional vars
-------------

Optional prepare templates and schedule jobs.

```yaml
burp_reports_nagios_servers:
  - SERVERNAME

# Optional cron jobs, example with template generated to send status through nsca
# http://docs.ansible.com/ansible/latest/modules/cron_module.html
burp_reports_cron_jobs:
  - name: 'burp-reports inventory nsca'
    job: '/usr/local/bin/burp-reports-nsca'
    day: "*"
    hour: "*"
    minute: "*/10"
    month: "*"
    weekday: "*"

# Optional generation of nsca scripts
burp_reports_nsca_scripts:
  - script: '/usr/local/bin/burp-reports-nsca'
    command: '/usr/local/bin/burp-reports -c {{ burp_report_conf }} --report inventory -i {{ burp_report_inventory_input }} -o {{ burp_report_inventory_output }}'
    nagios_service: 'burp_reports_status'
```

To use it you will need a nagios config service for burp-reports server name, ex:

```cfg
; https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/objectdefinitions.html#service
define service {
        service_description            burp_reports_status
        host                           YOURBURP-REPORTS-ORIGINATED
        use                            generic-service
        passive_checks_enabled         1
        max_check_attempts             1
        active_checks_enabled          0
        notification_interval          60
        check_freshness                1
        freshness_threshold            7200       ; 2 hour threshold, without cron execution will execute check_command
        check_command                  no-burp-reports
        # set the contact group
        # contact_groups                 {{ contact_group }}
        flap_detection_enabled         0
}

; https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/4/en/freshness.html

define command {
    command_name    no-burp-reports
    command_line    /usr/local/nagios/libexec/check_dummy 2 "burp-reports was not executed in time"
}
```

Tags
----

config_burp_reports_cron: if you want to change some cron task
test_burp_reports: tests commands, all configuration should be already done. 

Playbook example for site.yml
-----------------------------

    - hosts: servers
      environment: "{{ proxy_env }}"
      roles:
      - { role: common, tags: ["role_all", "role"] }
      
environment is not required!, only is an example in case you are behind a firewall. If that's you case, add the proxy var to you group_vars/all.
Example of that vars is at: https://github.com/CoffeeITWorks/ansible-generic-help/blob/master/example1/group_vars/all/vars

License
-------

MIT

Author Information
------------------

This role was main developed by Pablo Estigarribia (pablodav at gmail)

Burp backup and restore
=======================

Main page: http://burp.grke.org/

Burpui
======

Main page: https://git.ziirish.me/ziirish/burp-ui

