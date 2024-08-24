# Cisco8000-emulator

1. Acquire Cisco VXR EFT package and untar it:
```
tar -xvf 8000-emulator-eft12.1.tar
```

2. cd into the 'scripts' directory and run the Ubuntu server setup script then reboot
```
sudo ./UbuntuServerManualSetup.sh
```
   * The script takes a few minutes to run and may have a couple user prompts. Look for terminatal output: [SERVER SETUP IS COMPLETED]

3. Download and untar 8000e image tarball (if not using one of the images that come with the EFT package)

4. Construct a vxr topology yaml file [4-node](4-node-8201.yaml)

5. Note the "Custom" section in the example topology file - this is to connect router ports to linux bridge instances.  Create linux bridge instances:
```
sudo brctl addbr ce1
sudo brctl addbr ce2
sudo ip link set ce1 up
sudo ip link set ce2 up
```

```
cisco@linux-host-1:~$ brctl show
bridge name	bridge id		STP enabled	interfaces
ce1		8000.fe5a42b0d485	no		
ce2		8000.5e8491331a59	no		
docker0		8000.02425afcc113	no			
virbr0		8000.525400578356	yes		
virbr1		8000.525400d6574c	yes
```

6. test vxr.py
```
sudo vxr.py --version
```
Example output:
```
cisco@linux-host-1:~$ sudo vxr.py --version
1.5.16
```

7. Start the topology
```
sudo vxr.py start 4-node-8201.yaml
```
   * The script takes a few minutes to run