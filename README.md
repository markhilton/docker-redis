# Docker Redis image

Instructions to create Redis image on Debian VM from scratch, assuming that docker and docker-compose are already installed.

## Run container

when VM is already prepared (instructions below) start the container by executing:

```
docker-compose up -d
```

## Prepare VM

Default VM needs some tweaking to run Redis without any warrnings.

### Raise somaxconn above 511

WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
To fix this warning you have to set a new config to /etc/rc.local so that the setting will persist upon reboot.

Edit file: /etc/rc.local and add:

```
sysctl -w net.core.somaxconn=65535
```

When you reboot the next time, the new setting will be to allow 65535 connections instead of 128 as before.

### Fix VM memory overcommit

WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
To fix this we simply just echo the needed line into the correct file:

```
echo 'vm.overcommit_memory = 1' >> /etc/sysctl.conf
```

### Disable THP

WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local $
This is also simple to fix by just running the recommended command as stated in the warning.

Edit file:  /etc/rc.local add this:

```
echo never > /sys/kernel/mm/transparent_hugepage/enabled
```

Now this will be persistent upon reboot as well.
