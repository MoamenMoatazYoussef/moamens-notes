# AWS certified developer associate 2020
17 hours
1 hour per day?

## Fundamentals
### Regions
- Region: where different availability zones are located.
- Availability zone: where the physical data centers of AWS are.
- AWS consoles are scoped by region i.e. things interacting with each other must be in the same region so that they can see each other, 
- Only IAM and S3 are Global i.e. are not scoped by region.
- It's better to choose a region that's close to you.

### IAM
- IAM: Identity and access management.
- Basically, this is a service that deals with security: Users, Groups, Roles, Permissions, etc.
- IAM is at the center of AWS i.e. every service uses IAM to gain permissions.
- Permissions in AWS are "Policies", and they are written in JSON.
- IAM components:
	- Users: is used by a physical person, they can have permissions directly, and can be grouped in Groups. 
	- Group can have permissions so that all users in a group get these permissions.
		- A user with permissions directly linked to it, and in a group, has the permissions directly linked to it AND the group's permissions. 
	- Roles, they are like users but for "machines" e.g. external apps, another AWS service, etc.
- IAM enables Multi Factor Auth MFA.
- Best practice for security: give the user MINIMAL amount of permissions they need to perform their jobs.

- IAM Federation: A function in IAM where big companies can use to integrate their own repo of users with IAM so that when an employee of the company logs in, they can easily use their company credentials to login to AWS.
- IAM Federation uses SAML standard.

WARNINGS: 
- NEVER write IAM credentials in code. EVER. (I learned that the hard way, as usual xD)
- NEVER use root account except for initial setup.

Practical:
- Create a user.
- Follow best practices.
- Log in with your new admin user.

### EC2
- Elastic Cloud Compute.
- Basically this is how we rent servers to use the cloud.
- We have the ability to configure how data is stored (EBS), how load is distributed acorss the servers we rent (ELB), and how the servers scale (ASG).
- EBS: Elastic Block Storage, ELB: Elastic Load Balancing, ASG: Auto-Scaling Group.
- Creating an instance, we choose:
	- The AMI or Amazon Machine Image: the OS that the server we rent will use.
	- Instance Type: the specs of the machine e.g. number of vCPUs, instance storage, memory, network performance, etc.
	- Configuration data:
		- How many instances do you want to launch with the configuration you selected earlier.
		- Which VPC are they in.
		- Which subnet/availability zone are they in.
		- Some other stuff like shutdown behavior, monitoring, etc.
	- Storage: the specs of the hard drives connected to it e.g. their size, how many, what kind (SSD or other), IOPs, etc.
	- Tags: tags are key-value pairs used to identify the instance (or any type of service really, we can create them anywhere), these are completely configurable with exception of the first tag, which is Name.
	- Security Groups: these are a set of firewall rules that control the traffic from and to the instnace, we can add many rules, each rule has:
		- the type of connection allowed.
		- the protocol.
		- port range.
		- sources of the traffic.
	- Creating a Key Pair: this is a public/private key pair used to access this instance, either by windows logon or by SSH.
- Then we launch the instance, and get the stuff

#### SSH Overview
			SSH		PuTTY		EC2 Instance Connect
Mac			X					X	
Linux		X					X
Win<10		X		X
Win10		X		X			X

##### Connecting to an EC2 instance on Linux
- SSH: basically it's a way to get remote control of a machine using cmd.
- To connect to an EC2, we need its:
	- Public DNS.
	- Public IP.
	- Check in security groups that port 22 tcp is open to source (for learning, make the source 0.0.0.0)
	- The key pair.
- In linux:
	- Open a terminal.
	- cd to the location of the keypair.pem
	>> chmod 0400 keypair.pem
	>> ssh -i keypair.pem ec2-user@ip-address-of-ec2
	- You're in :)
	- Now, any commands you run will run on the EC2 instance itself.

#### Connecting to an EC2 instance on Windows
We know this ya mo2men :)
Using putty.


