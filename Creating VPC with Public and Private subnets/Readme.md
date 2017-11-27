# Creating AWS VPC with Public & Private subnets

## Create VPC

  * Using the VPC Wizard
  
  1. In the AWS console type VPC in the Search box and click on VPC/Isolated Cloud Resources. It will take you to the VPC Dashboard.
  2. On the VPC Dashboard CLick on _Start VPC Wizard_ button
  3. On the _Step 1. Select VPC Configuration_ page choose _VPC with Public and Private Subnets_, click _Select_ button. 
  4. On the _Step 2. VPC with Public and Private Subnets_ page configure the following settings,  click _Create VPC_ button. 
      * Make sure that the _IPv4 CIDR block:_ **10.0.0.0/16**
      * Write the name of your VPC in VPC Name field, for ex. **My VPC**
      * Make sure that the  Public subnet's IPv4 CIDR: is **10.0.0.0/24**
      * Choose the _Avaialblility Zone_, for ex. **eu-west-1a**
      * Give the public subnet a meaningful name, for ex. **My Public Subnet**
      * Make sure that the  Private subnet's IPv4 CIDR: is **10.0.1.0/24**
      * Choose the _Avaialblility Zone_ the same as for public subnet, for ex. **eu-west-1a**
      * Give the private subnet a meaningful name, for ex. **My Private Subnet**
      * Specify the details of your NAT gateway, click on _Use the NAT instance instead_. 
      * It will popup the Instance Type field, leave as deafult
      * In the Key Pair Name either create a new key (instructions in a different tutorial) or choose existing pem (for ex. mykey.pem) file from the pull down.
      
      everything else leave as defaults
    
  *Create VPC and Subnets separately
    
    TBD
    

## Create EC2s

  1. Select Services from the top menu
  2. Select EC2, click _Launch Instance_
  3. On _Step 1: Choose an Amazon Machine Image (AMI)_ page select **Amazon Linux AMI 2017.09.1 (HVM), SSD Volume Type** (free), click _Select_
  4. On _Step 2: Choose an Instance Type_ page choose the suggeted instance type and click _Next:Configure Instance Details_
  5. On _Step 3: Configure Instance Details_ page configure the following settings,  click _Next:Add Storage_ 
    * Network: choose the VPC you've created
    * Subnet: choose the **Public Subnet** you created
    * Autoassign Public IP: **Enable**
    * IAM role: use Default Role
  6. On _Step 4: Add Storage _ page click _Next:Add Tags_ 
  7. On _Step 5: Add Tags _ page add a new tag **Key=Name, Value=Public EC2** click _Next:Configure Security Group_
  8. On _Step 6: Configure Security Group_  page click _Review and Launch_
  9. On _Step 7: Review Instance Launch_ page click _Launch Instance_
  
  Repeat the steps for the **private** EC2 but choose the **Private Subnet** you created as a subnet and give the instance a different name
  
## Connect to Public EC2

  1. From the Windows machine open up Putty software 
  2. Populate Hostname with ec2-user@public.instanceDNS.....amazonaws.com
  3. Select SSH/Auth and browse for the ppk file (for ex. mykey.ppk) how to create the ppk file is in a different tutorial.
  4. Click _Open_
  5. In the terminal window optionally chcek the instace instalation details by typing 
  
    cat /var/log/cloud-init-output.log
  
  6. In the terminal window create a new dir for the private key
  
    mkdir key
    cd key
    
  7. On the Windows command prompt type change to the directory where the private keys are located and copy the key file to the public EC2 instance
  
  cd PrivateKeyDir
  pscp -i Mykey.ppk MyKey.pem  ec2-user@public.instanceDNS.....amazonaws.com:key/Mykey.pem
  


## Connect to Private EC2

 
  1. On the public EC2 Instance terminal type (instead of 10.0.1.228 type the private EC2 instance IP address. Hint: Click _Connect_ button on the INstances Dashboard)
  
    chmod 400 mykey.pem
    ssh -i "mykey.pem" ec2-user@10.0.1.228 
  
  2. In the private instnce terminal window optionally chcek the instace instalation details by typing 
  
    
    cat /var/log/cloud-init-output.log
  
  
THIS IS IT :)

