Ansible Role - Rasp Config
===========================

[![CI](https://github.com/m0by314/ansible_rasp_config/workflows/CI/badge.svg?event=push)](https://github.com/m0by314/ansible_rasp_config/actions?query=workflow%3ACI)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-rasp--config-green)](https://galaxy.ansible.com/m0by314/ansible_rasp_config)

An Ansible role to configure and secure a Raspberry Pi.

This role applies the security rules defined in the [official documentation](https://www.raspberrypi.org/documentation/configuration/security.md)


Requirements
------------

None

Role Variables
--------------

Variables are listed below along with default values:

Variable                          | Required | Default value | Description/Comment
----------------------------------| -------- | --------------| -----------
**rasp_user**                     |  Yes     |               | User who will replace pi user
**rasp_user_encrypted_password**  |  Yes     |               | The password must be encrypted for the use of the module. See  https://docs.ansible.com/ansible/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module for details on various ways to generate these password values.
**rasp_user_password**            |  Yes     |               | Password encrypt with ansible-vault or not encrypt 
**rasp_ssh_port**                 |          | 22            | Port ssh 
**ssh_key_file**                  |  Yes     |               | Full path to the id_rsa.pub key
**SERIAL**                        |          | true          | Enable/disable shell and kernel messages on the serial connection. (enabled by default)
**CAMERA**                        |          | false         | Enable/disable the CSI camera interface
**VNC**                           |          | false         | Enable/disable the RealVNC virtual network computing server
**SPI**                           |          | false         | Enable/disable SPI interfaces and automatic loading of the SPI kernel module, needed for products such as PiFace.
**I2C**                           |          | false         | Enable/disable I2C interfaces and automatic loading of the I2C kernel module.
**ONEWIRE**                       |          | false         | Enable/disable the Dallas 1-wire interface. This is usually used for DS18B20 temperature sensors.
**RGPIO**                         |          | false         | Enable or disable remote access to the GPIO pins.
**rasp_extra_packages**           |          | []            | List of packages to install.
**rasp_firewall**                 |          | false         | Enable/disable firewall
**rasp_open_firewall_port**       |          | []            | List the ports to open

**If the firewall is activated, automatically the ssh port contained in the variable rasp_ssh_port is opened** 

**Automatic packages installed by this role**: openssh-server, logrotate, fail2ban, ufw

Dependencies
------------

None.

Example Playbook
----------------

```yaml
---
- hosts: pi
  become: true
  remote_user: pi

  vars:
    rasp_user: raspc
    rasp_user_encrypted_password: $6$aefaY.bmHSu4WUub$gs6LaubATAq/GaizAbP83BGZe/eCJjneMbSweBiTBuyYIJt1zyh9n0hE.65IKaCUv4dcrlts8vbY1d3JckRco/
    rasp_user_password: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      31346637636133656135306137366532343039393233346261313430663663366335333865373663
      6264343363633030353739643635323930363030396230650a343437343633383965653033353535
      39353337316464316238376365313632373138393565383165353265653838343933366230666634
      6465626536646434330a636330663030646534663363306233316663663130666133633133643635
      3161
    rasp_ssh_port: 2225
    ssh_key_file: '/home/raspc/.ssh/id_rsa.pub'
    rasp_extra_packages:
      - python3
      - golang
    CAMERA: true
    rasp_firewall: true
    rasp_open_firewall_port:
      - 80
      - 8080

  roles:
    - m0by314.ansible_rasp_config
```

License
-------

MIT

Author Information
------------------

[m0by314](https://github.com/m0by314)
