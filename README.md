# CloudOps

![image](https://user-images.githubusercontent.com/14958328/157708221-013c7bd7-4d74-4ef5-8332-26df2dcf2df4.png)


Kindly check below inline answers. 

1. We require an outbound Squid Proxy that permits connections too
whitelisted domains that we can define.

Designed a solution that implements Squid proxy that proxies outbound connections(whitelists domains that we can define.). 

Squid proxy instances are created through an autoscaling group in public subnets and proxies outbound connections of instances where proxy is configured.

An internal facing NLB is created to load balance requests to the proxy instances. Clients should be configured with NLB DNS has the proxy host.

export http_proxy=http://<Proxy-DOMAIN>:<Proxy-Port>
export https_proxy=$http_proxy


2. Ensure that you consider a solution that demonstrates best practice
resilience within a single region.

For the purpose of this Diagram considered 2 AZs to show resiliency in a single region. Autoscaling group and NLB are created over this 2 AZs.

3. We would like to be able to run the same stack in different regions.
Note that Virginia has a different number of availability zones which we
would like to take advantage of for better resilience. Discuss how you
could use Terraform to use all the availability zones in a particular
region.

While using terraform we can get the list of all available availability zones with the data source 'aws_availability_zones'.

data "aws_availability_zones" "available" {
  state = "available"
}

Next, refer the above datasource while creating the NLB and ASG to scale based on the number of AZs.

4. We are looking to improve the security of our network and have decided we need a bastion server to avoid logging on directly to our servers. Propose adding a bastion server, the bastion should be the only route to SSH onto servers in the VPC.

In the Design diagram placed Bastition host in public subnets. This instance has port 22 opened in the ingress rules to accept ssh requests. For security purpose need to configure ingress rule to only accept connections from client IP.

5. We would like to be able to review the Squid access log.
6. We would like to be able to monitor the health of our squid instances.

CloudWatch can be used for Logging and Monitoring the instances. Install cloudwatch logs agent on the instance and stream the Squid access logs on the instances to CloudWatch Logs group.
  
