# Standalone to Standalone Migration

### Prerequisities:

Two AWS EC2 instances in different regions (Server A and Server B):
https://drive.google.com/file/d/1XpUH3mht_4PRbGE7E1j0e80fnj26SPku/view

Install Splunk in server A:
https://github.com/SoftManiaTech/splunk-system-admin-training/blob/main/Module%201%20-%20Splunk%20Server%20Deployment/README.md

Download and install Splunk demo app:
https://drive.google.com/file/d/1GjflDX-5eZo7qNKAVHvT5F7XWzDGn5ui/view

Setting Up an SCP File Transfer Connection between two Instances:
https://drive.google.com/file/d/1Efi7IJGjA0_RjVuOK7kvGa_QyJzqUPIe/view 


### Steps for migration

In Server A,

```bash
sudo su splunk
```
Stop Splunk in server_A
```bash
/opt/splunk/bin/splunk status
/opt/splunk/bin/splunk stop
```

```bash
cd /home/splunk
```
Package the SPLUNK_HOME directory
```bash
tar -cvzf splunk_migration_pkg.tar.gz /opt/splunk/
```
Check for the newly created Package
```bash
ls -ltr
```
Copy the packaged tar file from server_A to server_B
```bash
scp /home/splunk/splunk_migration_pkg.tar.gz splunk@<Server_B_PUBLIC_IP>:/home/splunk
```
In Server_B,
```bash
sudo su - splunk
```
```bash
cd /home/splunk/
ls -ltr
```
Extract the tar package in Server_B
```bash
tar -C / -zxvf /home/splunk/splunk_migration_pkg.tar.gz
```
```bash
cd /opt/splunk
ls -ltr
```
```bash
/opt/splunk/bin/splunk status
```
Start the Splunk
```bash
/opt/splunk/bin/splunk start
```
```bash
/opt/splunk/bin/splunk status
```

Login to server_B using the existing splunk credentials.

