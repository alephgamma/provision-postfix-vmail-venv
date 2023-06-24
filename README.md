# provision-postfix-vmail-venv

This is an ansible environment to provision an email server with virtual mailboxes.

## Pre-requisites

* ansible control-node
* target-node
* DNS domain
  - Domain record: {{ fqdn }}
  - Mail server record: mail.{{ fqdn }}
  - MX record
* PTR record
* certificate is ready

## The Big Picture
![alt text](https://github.com/alephgamma/provision-postfix-vmail-venv/blob/main/postfix-vmail.png?raw=true)
