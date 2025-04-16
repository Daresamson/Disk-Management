
# Disk Management and Mounting Project

## Project Overview
This project covers the process of attaching a new disk to an AWS EC2 instance, creating a partition, formatting it, mounting it, and ensuring it auto-mounts on boot. The project is carried out step-by-step using commands in Ubuntu on an EC2 instance. 

---

## Steps Taken
![Screenshot (251)](https://github.com/user-attachments/assets/3ee76f8f-6b30-40a4-a187-fe3d1f485e88)
![Screenshot (253)](https://github.com/user-attachments/assets/94b44663-ef51-4884-9963-ae79417b0af1)

### 1. **List Available Disks**
The first step was to list all available disks and partitions to confirm that the new disk (`/dev/nvme1n1`) was available for partitioning.

```bash
lsblk
```
![Screenshot (254)](https://github.com/user-attachments/assets/6b26b462-17f4-403c-9039-a42bd7f47e4b)

### 2. **Create a New Partition on the Disk**
Next, I used `fdisk` to create a partition on the available disk. The partition process involved the following steps:
1. **Started fdisk on `/dev/nvme1n1`:**
![Screenshot (255)](https://github.com/user-attachments/assets/0f9a651f-e116-4715-94e0-8272ffb91faa)

```bash
sudo fdisk /dev/nvme1n1
```

2. **Created a New Partition**:
    - Selected **primary** partition type (`p`).
    - Set the partition number as `1`.
    - Accepted the default for the first and last sector to use the full disk size.
    - Wrote the partition table.

The partition was successfully created as `/dev/nvme1n1p1`.
![Screenshot (255)](https://github.com/user-attachments/assets/033de0d3-2f6a-4cf5-b5a7-8297a09c38f6)
![Screenshot (256)](https://github.com/user-attachments/assets/850d36fe-b06d-48d2-8739-cd8ff0c42b7b)

### 3. **Format the Partition**
After creating the partition, I formatted it with the `ext4` filesystem to prepare it for use.

```bash
sudo mkfs.ext4 /dev/nvme1n1p1
```

The formatting process completed successfully.
![Screenshot (256)](https://github.com/user-attachments/assets/71c10454-0be7-4b2c-8545-affbb8ab0473)

### 4. **Create a Mount Point and Mount the Partition**
I created a mount point and mounted the partition to it:

1. **Created the Mount Point**:
   ```bash
   sudo mkdir /mnt/mydisk
   ```

2. **Mounted the Partition**:
   ```bash
   sudo mount /dev/nvme1n1p1 /mnt/mydisk
   ```

3. **Verified the Mount**:
   I checked the mounted filesystems to ensure the partition was mounted correctly.

   ```bash
   df -h
   ```
![Screenshot (256)](https://github.com/user-attachments/assets/856ab6e9-db9d-4b98-b180-742a29d6202f)
![Screenshot (257)](https://github.com/user-attachments/assets/f6d36dcc-6117-48b8-813f-f58191cf136e)

### 5. **Automate Mounting at Boot**
To ensure that the partition is automatically mounted at boot, I added the partition to `/etc/fstab`.

1. **Edited the `/etc/fstab` File**:
   Opened the file and added the following line to configure automatic mounting:

   ```bash
   sudo nano /etc/fstab
   ```

   Added the line:
   ```bash
   /dev/nvme1n1p1 /mnt/mydisk ext4 defaults 0 0
   ```

2. **Saved and Exited the Editor**:
   I saved the file (with `CTRL+X`, then `Y`, and `Enter`).
![Screenshot (258)](https://github.com/user-attachments/assets/b8c8b482-f82e-46cb-a13b-1547b07064c6)

### 6. **Test the `/etc/fstab` Configuration**
To test the new configuration without rebooting, I used the following commands:

1. **Reloaded systemd Daemon**:
   ```bash
   sudo systemctl daemon-reload
   ```
![Screenshot (259)](https://github.com/user-attachments/assets/f549e67c-fb2f-4d62-8baf-f412db095b1c)

2. **Unmounted the Partition**:
   ```bash
   sudo umount /mnt/mydisk
   ```

3. **Mounted All Filesystems**:
   ```bash
   sudo mount -a
   ```

4. **Verified the Mount**:
   ```bash
   df -h
   ```

This confirmed that the partition was correctly mounted.

---
![Screenshot (259)](https://github.com/user-attachments/assets/994d9f54-374f-4848-8a20-0b2663f0b93b)

## Conclusion
I successfully completed the disk management and mounting task on my EC2 instance, which involved creating a partition, formatting it, and ensuring that it mounts both manually and automatically at boot. I also tested the `/etc/fstab` configuration to ensure everything works as expected. 

This project helped me gain hands-on experience with disk management on Linux-based systems, as well as automating the mounting process at boot.

---
