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


    burp_report_days_outdated: 25
    burp_report_conf: /etc/burp/burp-reports.conf

    burp_conf_user: 'root'
    burp_conf_group: 'root'
    
    # https://github.com/pablodav/burp_server_reports#usage
    burp_report_inventory_input: None
    burp_report_inventory_output: None
    
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

