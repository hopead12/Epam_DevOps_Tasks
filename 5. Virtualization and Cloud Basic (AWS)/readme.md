# Task 5 AWS

#### Step 1

I created account on AWS and comleted labs on tutorials.


#### Step 2

###### AWS EC2

Launch my instance.

```
AMI - Amazon Linux 2
Instance Type - t3.micro
Network - default
Storage size - 8 GB (SSD)
Security Group - only SSH
```
And connected to it with MobaXterm.
![Photo](./screen/1.png)

#### Step 3

Created a snapshot of my instance and AMI for backup.

![Photo](./screen/2.jpg)

#### Step 4 

Created and attached a Disk_D to my instace.

![Photo](./screen/3.jpg)

#### Step 5

Launch backup.

![Photo](./screen/4.jpg)

Attached a Disk_D to backup.

![Photo](./screen/5.jpg)

#### Step 6

Created instance with WordPress on LightSail and ran my domain on *pp.ua.

![Photo](./screen/10.jpg)

#### Step 7

###### S3 Bucket

Created repository on S3 Bucket, uploaded some files.

![Photo](./screen/6.jpg)

#### Step 8

Created new user AWS - Administator.
Used CLI AWS and login in AWS.
Created new bucket "my-second2-buckup-bucket"
Uploaded file with CLI.

>aws s3 cp "C:\Users\Danil\Bosenko\Vagrantfile" s3://my-second2-backup-bucket

![Photo](./screen/7.jpg)

#### Step 9
###### AWS ECS

Installed Docker on an Amazon EC2 instance

>sudo yum install docker

Started to the Docker service. Add to group docker and reboot.
```
sudo service docker start
sudo usermod -a -G docker ec2-user
sudo reboot
```
Created docker file. Built and ran.
```
touch Dockerfile
vim Dockerfile
docker build -t hello-world .
docker run -t -i -p 80:80 hello-world
```

![Photo](./screen/8.jpg)

#### Step 10

Created cluster.

![Photo](./screen/11.jpg)

#### Step 11
###### Lambda


![Photo](./screen/12.jpg)

#### Step 12
###### Static website

http://website-danylo-bosenko.s3-website.eu-central-1.amazonaws.com/