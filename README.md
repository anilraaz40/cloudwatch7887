# cloudwatch7887
## 1. Create IAM Role for EC2
<pre>
Go to AWS Management Console → IAM → Roles → Create role.

Choose AWS service → EC2.

Attach the CloudWatchAgentServerPolicy managed policy:

Search for CloudWatchAgentServerPolicy and select it.

Give the role a name, e.g., CloudWatchAgentRole.

Create the role.
</pre>

## 2. Attach IAM Role to EC2
<pre>
Go to EC2 → Instances → Select your instance.

Click Actions → Security → Modify IAM role.

Select the role you created (CloudWatchAgentRole) and Save.
</pre>

## 3. Install CloudWatch Agent on Ubuntu
### Run these commands inside your EC2:
<pre>
sudo apt update
sudo apt install wget -y
wget https://amazoncloudwatch-agent.s3.amazonaws.com/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
sudo dpkg -i amazon-cloudwatch-agent.deb

</pre>

## 4. Create CloudWatch Agent Configuration
<pre>
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
</pre>
<P>
It will ask you questions about:

Metrics you want to collect (CPU, memory, disk, etc.).

Log file paths if you want to send logs.

Interval for metric collection.
At the end, it will save a config file in:
/opt/aws/amazon-cloudwatch-agent/bin/config.json

</P>

## 5. Start CloudWatch Agent
<pre>
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
    -a fetch-config \
    -m ec2 \
    -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
    -s

</pre>
## 6. Verify Agent is Running
<pre>
  sudo systemctl status amazon-cloudwatch-agent
</pre>
<p>You should see it active (running).</p>

## 7. Check in AWS Console
<pre>
Go to CloudWatch → Metrics and verify your EC2 metrics.

If you configured logs, check CloudWatch → Logs.
</pre>
