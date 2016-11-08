[![Build Status](https://travis-ci.org/CoffeeITWorks/ansible_burp_reports.svg?branch=master)](https://travis-ci.org/CoffeeITWorks/ansible_burp_reports)

Role Name
=========
Burp_reports installation and configuration role

Requirements
--------------

Burp-ui working and var defined in your group_vars/group
Recommended: ansible_burpui_server and ansible_burp2_server

Variables
---------

Try to not edit the role only for vars, all configurable vars are in defaults/main.yml so all of them can
be set on group_vars/group or host_vars/host files, so that one defined there will be the used when deployed.

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
      
environment is not required!, only is an example in case you are behind a firewall. If that's you case, add the proxy var to you group_vars/all: 

    proxy_env:
      http_proxy: 'http://user:password@hostproxy:8080'
      https_proxy: 'http://user:password@hostproxy:8080'
      ftp_proxy: 'http://user:password@hostproxy:8080'

License
-------

MIT