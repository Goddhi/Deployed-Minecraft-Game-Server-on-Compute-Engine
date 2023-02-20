
# Deployed Minecraft Game Server on Compute Engine

### Overview
- The Minecraft server software will run on a Compute Engine instance.

-  An e2-medium machine type that includes a 10-GB boot disk, 2 virtual CPU (vCPU), and 4 GB of RAM. This machine type runs Debian Linux by default.

- To make sure there is plenty of room for the Minecraft server's world data, you also attach a high-performance 50-GB persistent solid-state drive (SSD) to the instance. This dedicated Minecraft server can support up to 50 players.

## Objectives

- Customize an application server

- Install and configure necessary software

- Configure network access

- Schedule regular backups

### Step 1. Create the VM
- Login into GCP web console
- In the Cloud Console, on the Navigation menu (Navigation menu), click Compute Engine > VM instances.
- Click Create Instance.
- Specify the following and leave the remaining settings as their defaults:
Name : mc-server
Region : us-central1
Zone : us-central1-a
Boot disk : Debian GNU/Linux 11 (bullseye)
Identity and API access > Access scopes	: Set access for each API
Storage : Read Write

- Click Advanced options.
- Click Disks. You will add a disk to be used for game storage.
- Click Add new disk.
- Specify the following and leave the remaining settings as their defaults:

Name : minecraft-disk
Disk type : SSD Persistent Disk
Disk Source type : Blank disk
Size(GB) : 50
Encryption : Google-managed encryption key

- Click Save. This creates the disk and automatically attaches it to the VM when the VM is created.

- Click **Networking**.

- Specify the following and leave the remaining settings as their defaults:

Network tags : minecraft-server
Network Interfaces : Click **default** to edit the interface
External IPv4 address	: Create IP Address
Name : mc-server-ip

- click **Reserve**
- click **Done**
- click **Create**

![alt text](img1.png "Title")



### Step 2. Prepare the data disk

##### Create a directory and format and mount the disk

The disk is attached to the instance, but it is not yet mounted or formatted.


- For **mc-server**, click **SSH** to open a terminal and connect.

- To create a directory that serves as the mount point for the data disk, run the following command:
```
sudo mkdir -p /home/minecraft
```
- check the attached disk using the follow command
```
lsblk
```

- To format the disk, run the following command:
```
sudo mkfs.ext4 -F -E lazy_itable_init=0,\
lazy_journal_init=0,discard \
/dev/disk/by-id/google-minecraft-disk
```

- To mount the disk, run the following command:
```
sudo mount -o discard,defaults /dev/disk/by-id/google-minecraft-disk /home/minecraft
```
