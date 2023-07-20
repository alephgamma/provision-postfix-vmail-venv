# provision-postfix-vmail-venv

This is an ansible environment to provision an email server with virtual mailboxes.

## Pre-requisites

* ansible control-node
  - mkvirtualenv/postactivate contains environment variables:
    - DOMAIN
    - IP
* target-node
* DNS domain
  - Domain record: `{{ your-domain.com }}`
  - Mail server record: `mail.{{ your-domain.com }}`
  - MX record:  `10 mail.{{ your-domain.com }}`
* PTR record
* HTTPS Certificate is ready

## The Big Picture
For those of us that learn better by seeing, the image below is for visualization. 
![alt text](https://github.com/alephgamma/provision-postfix-vmail-venv/blob/master/postfix-vmail.png?raw=true)

## The playbooks

## The templates

## Use-Cases

### Local submission: Receipt verification from local user to local user

### Local submission: Transmission (sent) verification from local user to local user

### Network submission: Receipt verification at the target-node

### Network submission: Transmission (sent) verification from the target-node
