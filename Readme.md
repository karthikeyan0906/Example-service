# Example service

# Directory Structure :

deploy/ansible -> ansible roles and playbooks for installing tools and configuring services . 
deploy/infra   -> cloudformation files splitted into resource speficific files which needs to be stitched together 
                  using any python or CF SDK tools since modifying in a single template would be very tedious and difficult
deploy/packer.json -> can be used if we follow Image baking process . service AMI can be created from the baked base ami.
src            -> Service files
Jenkinsfile   -> can be used to create a pipeline jobs via groovy scripts instead of creating build jobs manually.


## Building
-----

From the root of the project, run

>    mvn clean package

## Integration Tests
----

From the root of the project, run:

>    mvn integration-test

## Running Locally
----

From the root of the project, run:

>    mvn spring-boot:run

### AWS Resources

Make sure you have valid AWS credentials.


### JVM Options

Make sure you have a local environment variable set as follows via command lines args or in your default shell environment:

>    export ENVIRONMENT=dev

or:

>    mvn spring-boot:run -Denvironment=dev


If the default HTTP/8080 or HTTPS/8443 ports are in-use locally, then you can override them by:

>    mvn spring-boot:run -Drun.jvmArguments='-Dserver.port=8888 -Dssl.server.port=8889'

### Health Check

You can use your browser, Poster, curl, ... to verify the application has started by hitting:

>    curl http://localhost:8080/health

Verify the application is fully functional by hitting the following endpoint (NOTE: This should fail as no CloudFormation has been executed to create any required AWS resources):

>    curl http://localhost:8080/health/check


## Technology
-------------

  *   Monitoring  - Newrelic (https://newrelic.com/)  or any open source tools like zabbix , newrelic 
  *   Logging - Sumologic (https://www.sumologic.com/) or any open source tools like ELK etc Splunk
  *   IAAC - CLoudFormation 
  *   ImageBuilder - Packer
  *   ConfigurationManagement - Ansible 
  *   Oncall Management Tool - Pagerduty
  

  
## Build and Deployment Strategy
---------------------------------

SIMPLE deployment 
Rolling deployment 

  
## Security Aspects 
---------------------------------

As per the compliance and other seurity standards , Image Baking process of base ami can be regularly updated with the required packages
and softwares . 

Penetration testing assesments can be carried to find to find any vulnerabilities or loopholes . 

WAF tools like Incapsula (Web Applicaiton Firewall) can be used  as inbound restriction for whitelisting and blacklisting based on countries,IP's.

Bastion Jump server can be used while SSHing prod servers,2 factor implementations in AWS consoles  .

outbound restriction can be scrutinized with squid tool

Cloudwatch and Newrelic Alerts can be intergrated with Pagerduty .

chaos tools can be used to test the resilience and withstanding capacity of the services by manually triggering alerts with high spikes

## Managing Costs
----------------------

Target Tracking policies can be enabled in ASG scaling options to scale down servers when not in use . 

DynamoDb was used since its reliable and cost efficient (Pap per request was chosed so that we can pay for only what we use
 in terms of read and write requests)
 
Serverless Application services can be tried to reduce the infra costs . 
