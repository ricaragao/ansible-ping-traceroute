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


## Results

### Ping results

The ping results OK show "msg": "PING SUCCESS". If fail, show skipping.

```
TASK [test_connect : Debug] ********************************************************************************************************************
ok: [192.168.103.3] => (item=Pingando 192.168.103.3) => {
    "msg": "PING SUCCESS"
}
skipping: [192.168.103.3] => (item=Pingando 192.168.103.55) 
ok: [192.168.103.6] => (item=Pingando 192.168.103.3) => {
    "msg": "PING SUCCESS"
}
ok: [192.168.103.3] => (item=Pingando 192.168.103.6) => {
    "msg": "PING SUCCESS"
}
skipping: [192.168.103.6] => (item=Pingando 192.168.103.55) 
ok: [192.168.103.3] => (item=Pingando 192.168.103.1) => {
    "msg": "PING SUCCESS"
}
ok: [192.168.103.6] => (item=Pingando 192.168.103.6) => {
    "msg": "PING SUCCESS"
}
ok: [192.168.103.6] => (item=Pingando 192.168.103.1) => {
    "msg": "PING SUCCESS"
}
```

### Traceroute results

The traceroute results show the stdout of command.

```
ok: [192.168.103.3] => (item=tracepath 192.168.103.3) => {
    "msg": [
        " 1:  webserver.local                                       0.093ms reached", 
        "     Resume: pmtu 65535 hops 1 back 1 "
    ]
}
ok: [192.168.103.6] => (item=tracepath 192.168.103.3) => {
    "msg": [
        " 1?: [LOCALHOST]                                         pmtu 1500", 
        " 1:  192.168.103.3                                         0.670ms !H", 
        " 1:  192.168.103.3                                         0.454ms !H", 
        "     Resume: pmtu 1500 "
    ]
}
ok: [192.168.103.3] => (item=tracepath 192.168.103.55) => {
    "msg": [
        " 1?: [LOCALHOST]                                         pmtu 1500", 
        " 1:  webserver.local                                     448.397ms !H", 
        " 1:  no reply", 
        " 1:  webserver.local                                     3006.881ms !H", 
        "     Resume: pmtu 1500 "
    ]
}
ok: [192.168.103.3] => (item=tracepath 192.168.103.6) => {
    "msg": [
        " 1?: [LOCALHOST]                                         pmtu 1500", 
        " 1:  192.168.103.6                                         0.416ms !H", 
        " 1:  192.168.103.6                                         0.384ms !H", 
        "     Resume: pmtu 1500 "
    ]
}
ok: [192.168.103.6] => (item=tracepath 192.168.103.55) => {
    "msg": [
        " 1?: [LOCALHOST]                                         pmtu 1500", 
        " 1:  localhost.localdomain                               447.715ms !H", 
        " 1:  localhost.localdomain                               3004.794ms !H", 
        "     Resume: pmtu 1500 "
    ]
}
ok: [192.168.103.6] => (item=tracepath 192.168.103.6) => {
    "msg": [
        " 1:  localhost.localdomain                                 0.083ms reached", 
        "     Resume: pmtu 65535 hops 1 back 1 "
    ]
}
ok: [192.168.103.3] => (item=tracepath 192.168.103.1) => {
    "msg": [
        " 1?: [LOCALHOST]                                         pmtu 1500", 
        " 1:  192.168.103.1                                         0.286ms reached", 
        " 1:  192.168.103.1                                         0.206ms reached", 
        "     Resume: pmtu 1500 hops 1 back 1 "
    ]
}
ok: [192.168.103.6] => (item=tracepath 192.168.103.1) => {
    "msg": [
        " 1?: [LOCALHOST]                                         pmtu 1500", 
        " 1:  192.168.103.1                                         0.235ms reached", 
        " 1:  192.168.103.1                                         0.181ms reached", 
        "     Resume: pmtu 1500 hops 1 back 1 "
    ]
}
```