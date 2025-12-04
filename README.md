# Useful terminal commands:

## Python

### Install compatible pip 
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py
```

## Centos
Proxy httpd permission denied.

```  /usr/sbin/setsebool -P httpd_can_network_connect 1 ```



## Ubuntu

### Login to the server without any password
ssh-copy-id [-i [identity_file]] [user@]machine

``` ssh-copy-id -i ~/.ssh/id_rsa.pub user@machine ```

### Generate ssh key

``` ssh-keygen -t rsa ```

### SSH examples

1. you will be able to have your ssh command listen on a control socket and wait for commands from subsequent ssh calls.
```
ssh -D localhost:1010 -S /tmp/.ssh1010 -M -fN localhost1010
# (...)
# You can terminate ssh connection
ssh -S /tmp/.ssh1010 -O exit localhost1010

```

### Change SSH Port

1- edit config file:
``` /etc/ssh/sshd_config ```

2- Change Port 22 to other port (Exp: 2022)

3- Disable old port usage:

```
sudo systemctl stop ssh.socket
sudo systemctl disable ssh.socket
```

4- Check any error on config ``` sshd -t ```

5- Restart service

```
sudo systemctl restart ssh
```

6- Check Port

After running the command below, the new port will be shown.

```
sudo ss -tulpn | grep sshd
```

### Proxychains
1- Check ip
 ````
 proxychains curl ifconfig.me/ip
 ````

### Tmux

1- start new session with name
```
tmux new -s test
```

2- Attach to tmux existing session
```
# Sessions list
tmux ls
# To connect to a session
tmux a -t test
```

### Disk space
- Top 10 large directories in `/var/` path.
```
sudo du -a /var/ | sort -n -r | head -n 10
```

- To see journal usage
```
journalctl --disk-usage
```
- To delete old x days journal logs
```
sudo journalctl --vacuum-time=2days
```
- Extend hard disk
```
fdisk /dev/sda

Command (m for help): n

Partition number (1-4): 3

Command (m for help): w
```
```
pvdisplay

pvcreate /dev/sda3
```
```
vgdisplay | grep Name

vgextend vg_vmware /dev/sda3
```
```
lvdisplay | grep Path

lvextend -l +100%FREE /dev/vg_vmware/lv_root

resize2fs /dev/vg_vmware/lv_root 
```


### Process
- to kill process by name
```
ps -ef |grep FullNode.jar |grep -v grep |awk '{print $2}'
```
### Docker
- To delete a container logs
```
sudo sh -c 'echo "" > $(docker inspect --format="{{.LogPath}}" CONTAINER_NAME)'
```

## Network

### iptables forward port to other ip
```
echo 1 > /proc/sys/net/ipv4/ip_forward

iptables -F
iptables -t nat -F
iptables -X

iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.12.77:80
iptables -t nat -A POSTROUTING -p tcp -d 192.168.12.77 --dport 80 -j SNAT --to-source 192.168.12.87
```


### Check server network gateway
``` ip r | grep ^def ```

## Apache2

### FREE SSL

``` sudo certbot --apache --cert-name anticoag360.ir --key-type rsa ```
