# CloudFront  

##基本概念：  
* **Edge location**: This is the location where content will be cached. This is separate to an AWS Region/AZ. *(Edge locations are not just READ only, you can write to them too)* 
* **Origin**: This is the origin of all the files that the CDN will distribute. This can be either an **S3 Bucket**, an **EC2 Instance**, an **Elastic Loac Balancer**, or **Route53**.  
* **Distribution**: This is the name given the CDN which consists of a collenction of Edge Locations.  
* **Web Distribution**: Typically used for Websites.
* **RTMP**: Used for Media Streaming.  To create an RTMP distribution, you must store the media files in an Amazon S3 bucket.
To use CloudFront live streaming, create a web distribution.
* Objects are cached for the life of the TTL(Time To Live)  
* You can clear cached objects, but you will be charged.