# AWS S3 Tips 
## S3 

### S3 has the following feature:

* Tiered Storage Available  
* Lifecycle Management  
* Versioning  
* Encryption  
* MFA Delete



## S3 Storage Classes
### S3 Standard  
* 99.99% availability  
* 99.999999999%(11*9)% durability, stored redundantly across multiple devices in multiple facilities, and is designed to sustain the loss of 2 facilities concurrently


### S3 - IA (Infrequently Accessed)
* For data that is accessed less frequently, but requires rapid access when needed.
* Lower fee than S3, but you are charged a retrieval fee.


### S3 One Zone - IA  
* For where you want a lower-cost option for infrequently accessed data, but do not require the multiple availability zone data resilience. 
 

### S3 - Intelligent Tiering  

* Designed to optimize costs by automatically moving data to the most cost-effective access tier,without performance impact or operational overhead.

### S3 Glacier
* S3 Glacier is a secure, durable, and low-cost storage class for data archiving. You can reliably store any amount of data at costs that are competitive with or cheaper than on-premises solutions. Retrieval times configurable from minutes to hours.  

### S3 Glacier Deep Archive  
* S3 Glacier Deep Archive is Amazon S3's lowest-cost storage class where a retrieval time of 12 hours is acceptable.  

## S3 - The Basics  
* Encryption in transit is achieved by **SSL/TLS**  
* Encryption at Rest(Server Side) is achieved by   
> S3 Managed Key - SSE-S3  
> AWS Key Management Service, Managed Keys-SSE-KMS  
> Server Side Encryption With Customer Provided Keys-SSE-C
* Client Side Encryption

## S3 - Versioning  
* Store all versions of an object(including all writes and even if you delete an object)  
* Great backup tool  
* Once enable, **Versioning cannot be disabled, only suspended**
* Integrates with lifecycle rules  
* Versioning's MFA Delete capability, which uses multi-factor authentication, can be used to provide an additional layer of security.  

## S3 - Lifecycle  
* Automates moving your objects between the different storage tiers  
* Can be used in conjunction with versioning  
* Can be applied to current versions and pervious versions.  

## S3 - Cross Region Replication  
* Versioning must be enabled on both the source and destination buckets.
* Regions must be unique.
* Files in an existing bucket are not replicated automatically.
* Delete markers are not replicated
* Deleting individual versions or delete markers will not be replicated  

























## S3 - Charges
You are charged for S3 in the following ways:  

* Storage
* Requests  
* Storage Management Pricing  
* Data Transfer Pricing  
* Transfer Acceleration  
* Cross Region Replication Pricing  








































## Exam Tips  
* Remember that S3 is Object-based:i.e. allows you to upload files.
* Files can be from 0 byte to 5 TB.
* There is unlimited storage.
* Filed are stored in **Buckets**
* **S3 is a universal namespace**. That is , names must be unique globally.
* Not suitable to install an operating system on, if you wanna install operating, you will need a **block storage**. But S3 using the **Object-based Storage**.  
* Successful uploads will generate a **HTTP 200** status code.  
* You can turn on **MFA Delete**   

---

##### The Key Fundamentals of S3 Are:
* **Key**(This is simply the name of the object)  
* **Value**(This is simply the data and is made up of a sequence of bytes).
* **Version ID** (Important for versioning)  
* **Metadata**(Data about data your are stroing)  
* **Subresources:**  
> Access Control Lists
> Torrent  
* Read after Write consistency for PUTS of new Objects  
* Eventual Consistency for overwrite PUTS and DELEtES(can take some time to propagate)   



















