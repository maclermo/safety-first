## safety-first

### Rationale

It is important to be protected from unwanted attack attemps from high-risk countries. I think this simple solution is very easy to automate and implement when deploying new virtual machines.

### Installation

Run the following commands to install the pre-requisites :

```shell
sudo apt-get update
sudo apt-get install -y ipset

sudo ipset -N dangerous hash:net
sudo iptables -A INPUT -m set --match-set dangerous src -j DROP
```

Copy the file ``block-ips`` to ``/usr/bin/`` and ``chmod +x /usr/bin/block-ips`` the file.

Install the following crontabs with ``crontab -e``:

```cron
0 3 * * * /bin/bash /usr/bin/block-ips &> /dev/null
@reboot /bin/bash /usr/bin/block-ips &> /dev/null
```

At every 3AM and at every reboot, you will get the latest ip definitions applied to your iptables/ipset.
