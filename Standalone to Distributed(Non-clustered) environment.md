# Standalone to Distributed(Non-clustered) environment
## 1) Take backup

Login as a root user and install the zip package
```bash
sudo su
yum install zip -y
```
Switch to Splunk user
```bash
sudo su - splunk
```
Stop the Splunk
```bash
cd /opt/splunk/bin/
./splunk stop
```
Zip the folder etc folder in /opt/splunk/
```bash
cd /opt/splunk/
zip -r etc.zip etc
```
Move the zip file to /tmp folder
```bash
cp etc.zip /tmp
```

Zip the folder "splunk" in /opt/splunk/var/lib
```bash
cd /opt/splunk/var/lib
zip -r splunk.zip splunk
```
Move the zip file to /tmp folder
```bash
cp splunk.zip /tmp
```
Check if the file exists
```bash
cd /tmp
ll
```
## 2) Copy Apps to your local
Check for the app to migrate in your standalone Splunk
```bash
cd /opt/splunk/etc/apps/
ll
```
Tar the app which you want to migrate
```bash
tar -zcf <APP_FOLDER_NAME>.tar.gz <APP_FOLDER_NAME>/
ll
```
Copy the tar file to /tmp folder
```bash
cp <APP_FOLDER_NAME>.tar.gz /tmp/
```
Check if that file exists in /tmp folder
```bash
cd /tmp/
ll
```
### Copy tar file from tmp to local,

In Downloads folder of local, execute the below command.
```bash
scp -i "YOUR_KEY.PEM" ec2-user@PUBLIC_Standalone_Splunk_IP:/tmp/<APP_FOLDER_NAME>.tar.gz .
```
## 3) How to Convert Splunk App configs from Standalone to Distributed

Follow the steps mentioned in the below link to split the app

https://github.com/SoftManiaTech/splunk-app-packaging/blob/main/Splunk_App_Packaging_Toolkit_Installation.md

## 4) Deploy app in the respective components

Note: Execute the below steps for all the three splunk components (Indexer, Search Head & Forwarder)

Copy the splited app from local to respective splunk components (Indexer, Search Head, Forwarder)
```bash
scp -i "YOUR_KEY.PEM" <APP_FOLDER_NAME>.tar.gz ec2-user@PUBLIC_IP:/tmp
```
Extract the file in /tmp folder
```bash
cd /tmp/
ll
tar -xvf <APP_PACKAGE_NAME>.tar.gz
ll
cd <Extracted_Folder>
tar -xvf <APP_PACKAGE_NAME>.tar.gz
ll
```
Move the app folder to apps folder
```bash
cp -rf <APP_FOLDER_NAME>/ /opt/splunk/etc/apps/
cd /opt/splunk/etc/apps/
ll
```
Restart the splunk
```bash
/opt/splunk/bin/splunk restart
```
## 5) verify if the app is installed successfully.

Login to UI of Indexer, Search Head & Forwarder, check for the app which we migrated in "Manage Apps" page. 

## 6) Data Migration

Copy the zipped file "splunk.zip" from Standalone to local,
```bash
scp -i "YOUR_KEY.PEM" ec2-user@PUBLIC_Standalone_Splunk_IP:/tmp/splunk.zip .
```
Copy the zipped file "splunk.zip" from local to Indexer,
```bash
scp -i "YOUR_KEY.PEM" splunk.zip ec2-user@PUBLIC_INDEXER_IP:/tmp
```
In indexer,
Login as a root user and install the zip package

```bash
sudo su
yum install zip -y
```

Switch to splunk user
```bash
sudo su - splunk
```

Check if the zip file "splunk.zip" exists
```bash
cd /tmp/
ll
```

Unzip the splunk.zip
```bash
unzip splunk.zip -d /opt/splunk/var/lib/
```
Restart the indexer
```bash
/opt/splunk/bin/splunk restart
```
