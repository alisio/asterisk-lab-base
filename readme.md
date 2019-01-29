## Asterisk-lab-test

This Vagrantfile implements a virtualbox, asterisk server, based on Centos/7 for testing purposes.

## Server information
* Centos/7 based install
* Asterisk 11 installed by default (change asteriskVersion variable inside vagrant file if another version is desired)
* Default SIP, IAX2, AMI and RTP ports mapped from host
* VoIP related packages: sngrep
* Pre-configured phone extensions: 1000 to 1010 (no need for password)
* Pre-configured local extensions context: from-office

## usage
1. start server running the following command:
```bash
vagrant up
```
2. provision server
```bash
vagrant provision
```
3. Acess server terminal:
```bash
vagrant ssh
```
