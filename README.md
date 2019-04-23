# Introduction

This is a Ansible playbook for connectivity test between hosts using ping and traceroute. Very useful when you are preparing a big infraestructure with many hosts that need connectivity between them.

# How to use

Usually, all linux OS has ping and traceroute applications, then you don't need install nothing in your remote hosts.

The ping return OK or NOT OK, and Traceroute the result of command.

## Configuration

This playbook separates the **sources** hosts from the **targets**. You can change this list in the file `hosts`. For example:
```
[sources]
192.168.103.3
192.168.103.6

[targets]
192.168.103.3
192.168.103.55
192.168.103.6
192.168.103.1
```

The file `groups_vars/all` has these values that you can change:
```
user: root
ping_command: ping -c 1 -q -W 1 
traceroute_command: traceroute -m 10
```

- **user** : remote user of *sources hosts* (don't need root)
- **ping_command** : ping command and your options (you can change)
- **traceroute_command** : traceroute command and your options (you can change by tracepath, is traceroute is not present)


## Execute

Inside of playbook path, execute: 

```
# ansible-playbook -i hosts site.yml -k
```

This above command ask for password. If you don't need password, remove -k parameter.


