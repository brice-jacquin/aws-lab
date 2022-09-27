# Module 7 : Storage

## Prerequisites

You have done the 01_ebs folder

## 2. Lab EFS

You have seen you can use EBS to store data. Now you need a share space to deposit files, with EFS.

Lab objective: 

![](../../ressources/assets/module07/module_07-EFS.png)

### 2.1. Create a SG for NFS

Create a Security Group that allows **NFS inbound** flow in your VPC.

<details>
<summary>SOLUTION</summary>

* Go to the AWS EC2 service
* Select the Security Groups menu and create a new Security group: 
  * Name it efs-inbound, same for the description
  * Select your VPC
  * Add a inbound rule for the NFS protocol and the 10.0.0.0/8 CIDR range 
  * Create the Security Group

</details>


### 2.2. Create a new EFS

Create an standard EFS.

<details>
<summary>SOLUTION</summary>

* Go to the AWS EFS service
* Click on **Create file system**
* Chose a name such as my-efs
* Select your VPC
* Create
* When the file system is created click on it and **View details**
* Go under **Network** tab and click on Manage
* For each mount target, select the Security group you created and save


</details>

### 2.3. Mount the EFS

You have created a new EFS, now you need to mount it on both instances. The local folder might be **/efs**.

<details>
<summary>SOLUTION</summary>

* Go to the AWS EFS service
* Click on your EFS and **View details**
* Click on **Attach**
* Copy the second command : 
  * `sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport <fsname>:/ efs`
* In the AWS EC2 Service, go under Instances menu
* Click on your first instance and Connect
* Chose the Session Manager Tab and Connect
* Execute the following commands:
```sh 
# Become root
sudo -i

# Create local folder
mkdir -p /efs

# Mount the EFS. Notice we have changed the mount point given by aws (efs -> /efs)
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport <fsname>:/ /efs
```
<details>
<summary>Go Further</summary>
When you try to resolve the DNS name of your FS, it wont be the same IP returned on one instance or the other. 
It will return the IP of the EFS mount target of the instance Availability zone. You can check it in the detail of the EFS.
</details>
* Repeat for the 2nd instance
* If you write data in /efs for one the 2 instances, the other should also see the modifications

</details>

### 2.5. (Optional) Question

Can you guess the EFS size ? 

### 2.4. Conclusion

EFS is useful when you need to share data in realtime with other servers such as cache, frontend server files, documents etc. 
It uses NFS and consequently doesn't match high performance use cases (i.e do not host an Oracle database on EFS)
Windows doesn't support natively NFS so it is mainly used by Unix systems. 

You never specified the size of the EFS, meaning you can add as much data as you want and you will pay for it at the GB, in opposite of the EBS where you reserved an amount of GB and pay for it even if your EBS isn't full. It also means the EFS **can cost A LOST** (do not log applications on it if you don't have a proper log cleaner policy)
