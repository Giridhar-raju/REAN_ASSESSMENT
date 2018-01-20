
Description:-
  Creating cloudformation template which accepts the required parameters as user inputs and configure/install the WordPress using ansible scripts which are present in git.

AWS CLI Command:-

    aws cloudformation create-stack --stack-name giriassessment --template-body file://wp_cft.json --parameters ParameterKey=imageid,ParameterValue=ami-xxxx ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=Keyname,ParameterValue=xxxxx ParameterKey=DBName,ParameterValue=wordpress ParameterKey=DBPassword,ParameterValue=xxxxxxxx ParameterKey=wordpressVersion,ParameterValue=4.9.1

Following resources are created using this template.

  1)AWS::EC2::VPC
  2)AWS::EC2::InternetGateway
  3)AWS::EC2::RouteTable
  4)AWS::EC2::Route
  5)AWS::EC2::VPCGatewayAttachment
  6)AWS::EC2::Subnet
  7)AWS::EC2::SubnetRouteTableAssociation
  8)AWS::EC2::SecurityGroup
  9)AWS::EC2::Instance

As part of EC2 instance userdata provided in the template, we will also install git, ansible which are provided . And, it will clone the git repository where ansible scripts resides and run the below ansible command to run the ansible playbook to configure and install the Apache Web Server, PHP, MySQL and wordpress by passing the using CFT parameters as extra variables.

Ansible Command:-
  ansible-playbook wordpress.yml -i 'localhost,' -e 'wordpress_db_name=",{ "Ref": "DBName" }," mysql_root_password=",{ "Ref": "DBPassword" }," wordpress_version=",{ "Ref": "wordpressVersion" },"' --connection local"


Ansible Playbook Tasks and Roles:
  In ansible playbook has 2 different roles, one role will execute common packages like Apache web server, PHP & MySQL server. Other role will install Wordpress and configure the Wordpress site.

Once the cloudformation stack creation is completed, we can check the logs at /var/log/cloud-init-output.log

After successful execution navigate to wordpress site using IpAddress/wordpress
