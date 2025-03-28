# SSH TCP Forwarding to access services from the remote servers network

[Back](../README.md)

Recently I had the problem that I set up my Homelab again and didn't do it from home. In other words, I was in an external network and only had the option of opening an ssh connection to my Proxmox host.

But now I had to access the web admin interface of my internal router to configure the reverse proxy.

The solution was to enable my SSH remote endpoint for RemoteTcpForwarding.

To do this, the `AllowTcpForwarding` option must be activated in the `/etc/ssh/sshd_config` file:


```text
AllowTcpForwarding yes
```

Afterwards you have to restart the ssh service to reload the changed config:

```bash
sudo systemctl restart ssh.service
```

Then on the local machine I was able to forward the remote networks servers port `192.168.0.1:80` to the local port `3434`:

```bash
ssh -L 3434:192.168.0.1:80 network@jfuerlinger.ddns.net
```

🚀🚀🚀 **Mission accomplished** 🚀🚀🚀

[Back](../README.md)