## Setting up simple LVS HTTP load-balancer


### Overview
- For this setup I used RedHat's Piranha but any LVS should work.
- There are 2 load-balancers in active/standby and two HTTP pools (with two servers each).
- The lvs.cf file should be identical on both lb's.
- All servers (lb and web) are on the same subnet (10.200.10.0/24)
- The load-balancers are in LVS-NAT mode.


### Environment
```
10.200.10.11 lb1 (mgmt IP)
10.200.10.12 lb2 (mgmt IP)
10.200.10.100 VIP1
10.200.10.101 web101
10.200.10.102 web102
10.200.10.200 VIP2
10.200.10.201 web201
10.200.10.202 web202
```

- All incoming HTTP connections are hitting VIP1 or VIP2 and then getting load-balanced between the two servers in each pool.
- In this case we're using wlc (wighted least connection) method but the weights are the same.


### NAT
LVS-NAT requires that the load-balancers act as default gateways for our web servers - 
you'll need to configure a static route on all web servers, so their DG is the LB.

You may want to have other special routes via the real default gateway e.g. for SSH connections.
On the LB's you'll need to add the following to sysctl.conf:

      net.ipv4.ip_forward = 1
      net.ipv4.conf.all.send_redirects = 0
      net.ipv4.conf.default.send_redirects = 0
      net.ipv4.conf.eth0.send_redirects = 0


### Failover
In case of lb1 going down, lb2 will automatically take over after 18 seconds.
It keeps an active connection table, so failover is mostly seamless.

