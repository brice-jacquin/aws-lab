# Module 7 : Storage

## 3. Lab S3 Bucket

### 3.1. Create a S3 Bucket

Create a S3 bucket, name it with your name as prefix, for example : *myname-my-bucket-example*
Activate the versioning and encrypt it with the S3 Key. 

<details>
<summary>SOLUTION</summary>

* Go AWS S3 Service
* **Create Bucket**:
  * Name it as wanted
  * Disable the ACL
  * Block the Public Access
  * Enable versioning
  * Enable server-side encrypt with the S3 managed keys.
  * Create Bucket

</details>

### 3.2. Upload to S3 and check versions

Create a file on your desktop and upload it multiple times. Modify your file between each upload. Check the different versions and restore one of them as current. 

<details>
<summary>SOLUTION</summary>

* Create a text file on your laptop. 
* Click on your bucket
* Click on Upload and Add files. Select your text file. Upload
* Repeat the operation as many times you want and modify your file (by incrementing an integer for example)
* Go to your Bucket, under Objects. 
* Click on your text file. You have the detail of your object here : size, URI, etc. 
* Go under Versions:
  * Download the version of your choice. You retrieve the version you uploaded earlier. 
  * Select the *current version* of Delete it.
  * Go back to the version tab and check the current version of your object.


</details>

### 3.3. Discover Object class

We will now simulate the fact that your file is an archive. For legal reason you need to store it for 3 years but you don't need to access it oftenly. 

Upload a new file in your bucket with the storage class **Glacier Flexible Retrieval**. 
Then, try to download the file from S3.

<details>
<summary>SOLUTION</summary>

* Click on your bucket
* Click on Upload and Add files. Select your file
* Under properties, select the Glacier Flexible Retrieval class, and upload 
* Now your file is uploaded but is not available for read 
* Select the object, and **initiate restore** under Actions:
  * Set the number of days at 1
  * Select **Expedited retrieval** 
  * Initiate Restore
* Wait a couple of minute (max 5min)
* Select the object again, you should be able to Download it

</details>

### 3.4. Create lifecycle rule 

Glacier is a very cost effective storage class (3.6$/TB/month) but is doesn't allow to get your data instantly. Also, the retreival is more expensive than the standard class: it is designed for archive and cold storage. 
A good solution is to implement a lifecycle rule, to automatically migrate old objects or versions from a class to an other. 

Disable versioning on the bucket and create a lifecyle rule that has the following characteristics: 
* Migrate objects from standard to Glacier Instant Retrieval after one week. 
* Migrate objects from Glacier Instant Retrieval to Glacier Deep Archive after 120 days
* Delete objects olders than 365 days
* Delete incomplete multipart upload after one week

It will be applied to every objects of the bucket

<details>
<summary>SOLUTION</summary>

* Click on your bucket
* Go under Management, **create lifecycle rule**:
  * Name: my-lifecyle-rule
  * Rule scope: Apply to all
  * Rule actions, select the following options: 
    * *Move current versions of object between storage classes*
    * *Expire current versions of objects*
    * *Delete expired objects markers or incomplete multipart upload*
  * Transition current versions option: 
    * Select Glacier Instant Retrieval and set the value to 7 days
    * Add a transition
    * Select Glacier Deep Archive and set the value to 120 days
  * Expire current version option:
    * Set the value to 365
  * Delete incomplete multipart uploads option: 
    * Set the value to 7
  * Create rule

</details>
