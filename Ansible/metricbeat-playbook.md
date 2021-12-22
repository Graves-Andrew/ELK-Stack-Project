---
- name: Install Metricbeat
  hosts: webservers
  become: yes
  tasks:

  - name: Download Metricbeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

  - name: Install Metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

  - name: Drop in metricbeat.yml
    copy:
      src: /etc/ansible/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

  - name: Enable and configure docker module
    command: metricbeat modules enable docker

  - name: Metricbeat setup
    command: metricbeat setup

  - name: Start Metricbeat
    command: service metricbeat start

  - name: Enable service Metricbeat on boot
    systemd:
      name: metricbeat
      enabled: yes