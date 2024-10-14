*instructions assume containerlab or other orchestrator of dockerized router images is already installed*

1. load dockerized 8201 image:
```
docker load -i 8201-32fh_clab293_latest.tar.gz 
```

2. clone/download this repo
```
git clone https://github.com/brmcdoug/Cisco8000-emulator.git
```

3. cd into the repo directory and deploy topology
```
sudo containerlab deploy -t 4-node-clab.yaml 
```

Output should look something like
```
brmcdoug@naja:~/Cisco8000-emulator$ sudo containerlab deploy -t 4-node-clab.yaml 
INFO[0000] Containerlab v0.55.1 started                 
INFO[0000] Parsing & checking topology file: 4-node-clab.yaml 
INFO[0000] Creating lab directory: /home/brmcdoug/Cisco8000-emulator/clab-clab8000e 
INFO[0000] Creating container: "spine1"                 
INFO[0000] Creating container: "leaf2"                  
INFO[0000] Creating container: "leaf1"                  
INFO[0000] Creating container: "rr1"                    
INFO[0002] Created link: rr1:FH0_0_0_1 <--> spine1:FH0_0_0_2 
INFO[0003] Created link: leaf1:FH0_0_0_0 <--> spine1:FH0_0_0_0 
INFO[0003] Created link: leaf2:FH0_0_0_1 <--> spine1:FH0_0_0_1 
INFO[0003] Adding containerlab host entries to /etc/hosts file 
INFO[0003] Adding ssh config for containerlab nodes     
INFO[0003] 🎉 New containerlab version 0.57.0 is available! Release notes: https://containerlab.dev/rn/0.57/
Run 'containerlab version upgrade' to upgrade or go check other installation options at https://containerlab.dev/install/ 
+---+-----------------------+--------------+--------------------------+-------+---------+--------------------+--------------+
| # |         Name          | Container ID |          Image           | Kind  |  State  |    IPv4 Address    | IPv6 Address |
+---+-----------------------+--------------+--------------------------+-------+---------+--------------------+--------------+
| 1 | clab-clab8000e-leaf1  | 18611d6a8d60 | 8201-32fh_clab293:latest | linux | running | 192.168.122.101/24 | N/A          |
| 2 | clab-clab8000e-leaf2  | 13d8e72ea09c | 8201-32fh_clab293:latest | linux | running | 192.168.122.103/24 | N/A          |
| 3 | clab-clab8000e-rr1    | 9ceef0760830 | 8201-32fh_clab293:latest | linux | running | 192.168.122.100/24 | N/A          |
| 4 | clab-clab8000e-spine1 | 88f9b0898c83 | 8201-32fh_clab293:latest | linux | running | 192.168.122.102/24 | N/A          |
+---+-----------------------+--------------+--------------------------+-------+---------+--------------------+--------------+
```

4. routers take a few minutes to build/boot. use docker logs command to monitor progress
```
docker logs -f clab-clab8000e-leaf1 
```

Eventually routers should come up and tail of docker logs looks like
```
18:45:05 INFO R0:applying XR config
18:45:08 INFO Sim up
Router up

^C
```

5. routers should be reachable via ssh to mgt IP - see config directory
```
ssh cisco@192.168.124.100
```

6. if for some reason routers are not reachable via ssh/ping try accessing console to troubleshoot
```
docker exec -it clab-clab8000e-leaf1  telnet 0 60000
```