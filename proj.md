# Re-Architecting-Web-App-on-Amazon-Cloud

## Using AWS Elastic Beanstalk as a platform for re-architecting our web application on Amazon cloud. The main purpose of Elastic Beanstalk is to manage a resource to build our web application in the same suite

## Entire stack will be using PASS and SAS service and not directly using EC2 instances, we will be using Beanstalk, RDS, AmazonMQ and cloud front distribution, behind the sence Beanstalk will be also using S3 bucket, cloud watch for monitoring making the entire setup give very low operational overhead

### First creat security group and the keypairs

## Creating Relational Database Service(RDS), but before we create RDS we will create the Subnet groups and Parameter groups

### Subnet groups
![Alt text](<images/Screenshot 2023-10-11 181419.png>)

### Parameter groups
![Alt text](images/para.png)

### Create RDS Database
![Alt text](images/rds.png)

## Creating AWS ElastiCache, but before we create that, we will create subnet groups and parameter groups

### Subnet groups
![Alt text](images/elasub.png)


### Parameter groups
![Alt text](images/ElasPara.png)

### Creating AWS Elasticache
![Alt text](images/Elasticache.png)

## Creating Amazon MQ for our RabbitMQ
![Alt text](images/broker.png)

## Database Initialization

### We will run SQL queries on our instance from our source code, The instance will be lunch in the same VPC
### The only way to access our instance which is in private subnets, is through the same VPC

![Alt text](<images/Screenshot 2023-10-12 010424.png>)

### Lunch instance and run and install mysql-client making sure you one of the rule allows 3306 and copy the security group id created into the backend security group

`sudo apt update && sudo apt install mysql-client -y`

![Alt text](images/lun.png)

### Testing the connection for the DB

`mysql -h re-arch-rds-db.ckabsif0pmib.us-east-1.rds.amazonaws.com -u admin -pexmWjLfVhP3Tsvl accounts`

### We will see emppty set because we have not run our SQL query

### Clone source code
`git clone https://github.com/hkhcoder/vprofile-project.git`

![Alt text](images/gitclone.png)

### To excute all the SQL queries on the database accounts which is on RDS

` mysql -h re-arch-rds-db.ckabsif0pmib.us-east-1.rds.amazonaws.com -u admin -pexmWjLfVhP3TsvlYH4oh accounts < src/main/resources/db_backup.sql`

### Run the queries again to test the connection

`mysql -h re-arch-rds-db.ckabsif0pmib.us-east-1.rds.amazonaws.com -u admin -pexmWjLfVhP3Tsvl accounts`

![Alt text](<images/show tables.png>)

## Setting up Amazon Elastic Beanstalk

### creating roles that will be use for setting up Beanstalk

![Alt text](images/BeanRole.png)

### Elastic Beanstalk

![Alt text](images/beannnn.png)

![Alt text](images/beanCofi.png)

### We will 1. enable ACL(Acess Control Point) on s3 bucket, 2. Update the health check in the target group, 3.Update security group

## Build and Deploy Artifact

### Copying source code and making necesarry adjustment and entering details of the backend services in the source code (Mysql, Memcache and rabbitmq details) and validate
` cat src/main/resources/application.properties `

![Alt text](images/validate.png)

### Build our Artifact

`mvn install`

![Alt text](images/mvn2.png)

### From the Environment of the Elastic Beanstalk upload the build artifact and allow beanstalk to build

![Alt text](images/complete.png)

![Alt text](images/validate2.png)

### connect through https will not work because the certificate is for a different domain

![Alt text](<not secure.png>)

### we will solve the above through the Certificate Manager,we add the certificate to the DNS in Godday 

![Alt text](images/raam.png)

![Alt text](images/validate333.png)

## Amazon CloudFront ..........

### validating from different brower

![Alt text](images/diff.png)

![Alt text](<images/Re-Architecture WebApp on AWS Cloud.png>)

The End











