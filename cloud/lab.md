# Lab Overview
In this lab, students will work with the Linux VM to learn about cloud computing with localstack to emulate interacting with an AWS environment. This setup keeps everything local, leverages your VM environment, and gives students a taste of cloud workflows. The lab is divided into several tasks, each focusing on different aspects:
1. **Initial Setup**: Students will prepare their virtual machines by installing necessary packages and tools.
2. **Deploying localstack with docker**: Students will create a docker compose file and deploy localstack
3. **AWS profile configuration**: Students will create a aws profile configuration to use with the aws command when interacting with localstack
4. **AWS S3 Buckets**: Students will interact with localstack to learn about S3 buckets.
5. **AWS EC2**: Students will interact with localstack to learn about EC2 instances.
6. **AWS lambda**: Students will interact with localstack to learn about lambda functions.

You will need two separate terminals, one to running the docker container and the other to run commands.

---


Learning Outcomes
- Understand how to emulate AWS services locally with LocalStack.
- Configure and interact with an S3 bucket using AWS CLI.
- Configure and interact with an EC2 instances using AWS CLI.



## NIST NICE Framework Mapping

The below items are mapped to the NIST NICE Framework for this lab. The NIST NICE Framework maps Knowledge, Skills, and Abilities (KSA) for a cybersecurity job function.

| Category | Code  | Description |
|----------|-------|-------------|
| Knowledge| K0770 | Knowledge of system administration principles and practices. |
|          | K0805 | Knowledge of command-line tools and techniques. |
| Skills   | S0002 | Skill in using command-line tools. |
|          | S0487 | Skill in operating IT Systems. |
| Abilities| A0001 | Ability to administer network operating systems. |
|          | A0058 | Ability to execute OS command line (e.g., ipconfig, netstat, dir, nbtstat). |

<div style="page-break-after: always;"></div>
---

## Purpose
The purpose of this lab is to provide students with hands-on experience in implementing and managing cloud computing. By completing this lab, students will gain practical skills in configuring, deploying, and troubleshooting using AWS services. These skills are can be important for managing cloud computing, ensuring students are well-prepared for real-world scenarios.

<br/>
<div style="page-break-after: always;"></div>

## Overview
In this lab, students will work with the Linux VM to learn about cloud computing with localstack to emulate interacting with an AWS environment. The lab is divided into several tasks, each focusing on different aspects:
1. **Initial Setup**: Students will prepare their virtual machines by installing necessary packages and tools.
2. **Deploying localstack with docker**: Students will create a docker compose file and deploy localstack
3. **AWS profile configuration**: Students will create a aws profile configuration to use with the aws command when interacting with localstack
4. **AWS S3 Buckets**: Students will interact with localstack to learn about S3 buckets.
5. **AWS EC2**: Students will interact with localstack to learn about EC2 instances.
6. **AWS lambda**: Students will interact with localstack to learn about lambda functions.

You will need two separate terminals, one to running the docker container and the other to run commands.


## Installing packages
<pre><code class="language-bash">
sudo apt update
sudo apt install -y vim wget curl libxml2-utils
sudo apt install -y python3 python3-pip docker.io awscli docker-compose
</code></pre>

### Directory creation and file creation
Create directory and CD into it
<pre><code class="language-bash">
mkdir localstack
cd localstack
</code></pre>

Create File
<pre><code class="language-bash">
shuf -n10 /usr/share/dict/american-english > example.txt
</code></pre>



## Deploying the localstack instance

Create docker-compose.yml file with the following contents
<pre>
services:
  localstack:
    image: localstack/localstack:latest
    container_name: localstack1
    ports:
      - "4566:4566"          # Main LocalStack endpoint
      - "4510-4559:4510-4559" # Additional service ports
    environment:
      - SERVICES=s3,ec2,lambda      # Enable S3 and EC2 only
      - DEBUG=1              # Enable debug logs (optional)
      - DOCKER_HOST=unix:///var/run/docker.sock  # For Docker-in-Docker if needed
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"  # Optional, for container management
      - "./localstack-data:/var/lib/localstack"      # Persist data locally
    restart: unless-stopped
</pre>

Start the LocalStack within Docker with docker-compose
<pre><code class="language-bash">
sudo docker-compose up
</code></pre>

Expected output
<pre>
...
localstack1   | 2000-01-01T23:43:39.315 DEBUG --- [  MainThread] localstack.utils.ssl       : Attempting to download local SSL certificate file
localstack1   | 2000-01-01T23:43:39.659 DEBUG --- [  MainThread] localstack.utils.ssl       : SSL certificate downloaded successfully
localstack1   | 2000-01-01T23:43:39.660 DEBUG --- [  MainThread] plux.runtime.manager       : instantiating plugin PluginSpec(localstack.runtime.server.twisted = <class 'localstack.runtime.server.plugins.TwistedRuntimeServerPlugin'>)
localstack1   | 2000-01-01T23:43:39.660 DEBUG --- [  MainThread] plux.runtime.manager       : loading plugin localstack.runtime.server:twisted
localstack1   | 2000-01-01T23:43:39.792 DEBUG --- [ady_monitor)] plux.runtime.manager       : instantiating plugin PluginSpec(localstack.hooks.on_infra_ready._run_init_scripts_on_ready = <function _run_init_scripts_on_ready at 0x7b913d1558a0>)
localstack1   | 2000-01-01T23:43:39.793 DEBUG --- [ady_monitor)] plux.runtime.manager       : loading plugin localstack.hooks.on_infra_ready:_run_init_scripts_on_ready
localstack1   | Ready.
</pre>

Once complete, open a new terminal in the localstack directory created before and don't close the one running the localstack.

## Setup AWS profile
<pre><code class="language-bash">
aws configure --profile localstack
</code></pre>

Enter the following when configureing the aws localstack profile:
- AWS Access Key ID: test
- AWS Secret Access Key: test
- Default region name: us-east-1
- Default output format: json



## S3 Buckets

### Veiwing help for the S3 command
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3 help
</code></pre>

You can browse the help command to understand some of the tasks you can perform as well as a few examples. To exit out of the help page, you should be able to push the **q** key on your keyboard to quit and get back to the terminal.

You can append **help** at the end of the command to get help with the latest part of the command. For example, the previous command we ran with the `S3 help` at the end gave us information about the **s3** command. If we had `s3 ls help` then it would instead give us context around the **ls** command and not the s3 command.

### List buckets
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 s3 ls --profile localstack
</code></pre>
You should note that there is no output because there is no S3 buckets. Next we will create a S3 bucket and try this command again and we should see the newly created bucket.

### Creating a new bucket
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3 mb s3://my-local-bucket
</code></pre>

### List buckets
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 s3 ls --profile localstack
</code></pre>

### Copy file to bucket
Within our terminal in the localstack directory we can issue the `ls` command and see a file called example.txt. 
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3 cp example.txt s3://my-local-bucket/example.txt
</code></pre>

Open a web broswer and you should be able to see the item in the bucket. Go to the following URL: http://localhost:4566/my-local-bucket

### Download object from bucket
Within our terminal in the localstack directory we will download the example.txt file into a file called downloaded-example.txt 
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3 cp s3://my-local-bucket/example.txt downloaded-example.txt
</code></pre>

Once complete you should be able to issue the `ls` command and see the newly downloaded file. Both the example.txt and downloaded-example.txt should have the same content.

### Delete object from bucket
Let's now see about deleting an object from a bucket. Delete the example.txt file we created earlier. Modify the below command to complete that.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3api delete-object --bucket --key <file-name.format>
</code></pre>

You can now verify the object is gone in the web browswer by navigating to the my-local-bucket again. 

We can also use the `curl` command in a terminal to curl the content of the bucket and pipe the output into xmllint to format the XML content. Otherwise it will come out as a blob of text with no formatting. 
<pre><code class="language-bash">
curl http://localhost:4566/my-local-bucket | xmllint --format -
</code></pre>


### Add and Delete Tag to a S3 bucket
Adding a tag to the S3 bucket
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3api put-bucket-tagging --bucket my-local-bucket --tagging 'TagSet=[{Key=Environment,Value=Test}]'
</code></pre>

Get Tags on the S3 bucket
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3api get-bucket-tagging --bucket my-local-bucket
</code></pre>

To remove the tag from the bucket
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3api delete-bucket-tagging --bucket my-local-bucket
</code></pre>

Once the tag is deleted and you try to get the tags, it should come back with an error saying TagSet doesn't exist.

### Remove a bucket
Let's reupload our example.txt file to the bucket.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack s3 cp example.txt s3://my-local-bucket/example.txt
</code></pre>

To remove a bucket you can use the following.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 s3 rb s3://my-local-bucket --profile localstack
</code></pre>
It should fail if the bucket isn't empty.


To remove a bucket and force the deletion regardless if has items, you can use the --force option.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 s3 rb s3://my-local-bucket --force --profile localstack
</code></pre>
If successful, you should see output in the terminal about the bucket being removed.



## EC2 Instances
LocalStack supports a subset of EC2 functionality (e.g., launching instances, describing them), and while it doesn’t run full VMs like real AWS EC2, it simulates the API and metadata, making it a great teaching tool for cloud compute concepts.

AWS Instance Types going more in detail on system specs and purpose:
https://aws.amazon.com/ec2/instance-types/


Here’s a selection of instance types across different families that you can use with LocalStack’s --instance-type parameter:

General Purpose
- t3.micro: Next-gen burstable instance, slightly more modern than t2.micro.
- t3.small: Slightly larger than t3.micro, good for small workloads.
- m5.large: Balanced compute/memory, common for general-purpose apps.
- m6i.xlarge: Intel-based, newer generation, for medium workloads.

Compute Optimized
- c5.large: High CPU performance, ideal for compute-intensive tasks.
- c6g.medium: Graviton (ARM)-based, energy-efficient compute.
- c7a.xlarge: 7th-gen AMD EPYC, high-performance compute.

Memory Optimized
- r5.large: High memory-to-CPU ratio, great for databases.
- r6i.xlarge: Intel-based, newer memory-optimized instance.
- x2gd.medium: Graviton-based, extreme memory performance.

Storage Optimized
- i3.large: High I/O, optimized for storage-heavy workloads.
- d3.xlarge: Dense storage, for big data or file systems.

GPU/Accelerated Computing
- g4dn.xlarge: NVIDIA GPU, for graphics or ML workloads.
- p3.2xlarge: High-end GPU, for deep learning simulations.

Notes
Naming Convention: AWS instance types follow a pattern: [family][generation].[size] (e.g., t3.micro: T-family, 3rd gen, micro size).
LocalStack Behavior: In LocalStack, these are just strings—it accepts them without enforcing resource limits or actual hardware differences. Use them to teach naming and use-case concepts.

### EC2 Create Security Group
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack ec2 create-security-group --group-name my-sec-group --description "My security group"
</code></pre>

Authorize inbound ssh traffic
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack ec2 authorize-security-group-ingress --group-name my-sec-group --protocol tcp --port 22 --cidr 0.0.0.0/0
</code></pre>

List security groups
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack ec2 describe-security-groups
</code></pre>


### EC2 get instances and format as table

Create an EC2 instance:
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 ec2 run-instances --image-id ami-12345678 --instance-type t2.micro --count 1 --profile localstack
</code></pre>

List EC2 instaces:
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 ec2 describe-instances --profile localstack
</code></pre>

When listing the EC2 instances you should get a json output in the terminal. Since we just created a instance with the t2.micro specs, we should be able to see that in the InstanceType as well as the State should be running. 

### Use the AWS CLI to stop the instance:
modify the below code to stop the previously created instance. You will need to find the InstanceID of the instance you want to stop.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 --profile localstack ec2 stop-instances --instance-ids <instance-id>
</code></pre>

Expected Output for instanceId i-47f4c092c32fb3ab0:
<pre>
aws --endpoint-url=http://localhost:4566 ec2 stop-instances --instance-ids i-47f4c092c32fb3ab0 --profile localstack
{
    "StoppingInstances": [
        {
            "CurrentState": {
                "Code": 64,
                "Name": "stopping"
            },
            "InstanceId": "i-47f4c092c32fb3ab0",
            "PreviousState": {
                "Code": 16,
                "Name": "running"
            }
        }
    ]
}
</pre>

Check the status of the instance to ensure it has stopped:
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 ec2 describe-instances --instance-ids <instance-id> --profile localstack
</code></pre>


### Get all EC2 instances for profile
Just like before we can list all instance for the localstack profile and depending on the number of instances running the output can get long.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 ec2 describe-instances --profile localstack
</code></pre>

Get all EC2 instances for profile and formatting output to table so it may be a bit easier to digest
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 ec2 describe-instances --profile localstack  --output table
</code></pre>

You can also do some querying for specific fields. The previous table output had all the content from the json file. Maybe we just want to filter out certain items.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 ec2 describe-instances --profile localstack \
  --query 'Reservations[*].Instances[*].[InstanceId, InstanceType, State.Name, LaunchTime]' \
  --output table
</code></pre>

Querying for specific fields adding column headers. The previous output could be nice but you can enhance it some more by modifying the command to add column headers for each field pulled.
<pre><code class="language-bash">
aws --endpoint-url=http://localhost:4566 ec2 describe-instances --profile localstack \
  --query 'Reservations[*].Instances[*].{ID: InstanceId, Type: InstanceType, Status: State.Name, Pub_IP: PublicIpAddress, Launch: LaunchTime}' \
  --output table
</code></pre>

### Terminate an EC2 instance
You can shut down the specified instance as well. 
<pre><code class="language-bash">
INSTANCE_ID=<instance-id>
aws --endpoint-url=http://localhost:4566 ec2 terminate-instances --instance-ids $INSTANCE_ID --profile localstack
</code></pre>




## Lambda Functions
Create a file called lambda_handler.py with vim. You can use other text editors if vim isn't perferred. Like nano text editor.
<pre><code class="language-python">
vim lambda_handler.py
</code></pre>

Put the following code in the lambda_handler.py file and save the file.
<pre><code class="language-python">
def lambda_handler(event, context):
    return {"statusCode": 200, "body": "Hello from LocalStack!"}
</code></pre>

Now zip the python file into a zip file to be used for lambda.
<pre><code class="language-python">
zip -r lambda.zip lambda_handler.py
</code></pre>

Create the lambda function called my-function and use the lambda.zip file.
<pre><code class="language-python">
aws --endpoint-url=http://localhost:4566 --profile localstack lambda create-function --function-name my-function --runtime python3.8 --role arn:aws:iam::000000000000:role/lambda-role --handler lambda_handler.lambda_handler --zip-file fileb://lambda.zip
</code></pre>

You can list the lambda functions.
<pre><code class="language-python">
aws --endpoint-url=http://localhost:4566 --profile localstack lambda list-functions
</code></pre>

Invoking the lambda my-function function and havign it put the output into a file called output.txt
<pre><code class="language-python">
aws --endpoint-url=http://localhost:4566 --profile localstack lambda invoke --function-name my-function output.txt
</code></pre>

There should now be a new file in the localstack directory called output.txt and we can cat the file to view the contents.
<pre><code class="language-python">
cat output.txt
</code></pre>

We can even delete functions
<pre><code class="language-python">
aws --endpoint-url=http://localhost:4566 --profile localstack lambda delete-function --function-name my-function
</code></pre>

You can verify that there are no more functions by rerunning the list functions command and the results should show no functions anymore.

