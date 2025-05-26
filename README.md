# Monitor-EC2-logs-and-trigger-alarm
Monitor EC2 logs and trigger alarm

 ![image](https://github.com/user-attachments/assets/585ef6d7-78d8-48c6-a97e-db2fc5c08afe)
 
!\ Create IAM policy and assign it to new role
1. IAM Policy
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogStreams"
    ],
      "Resource": [
        "arn:aws:logs:*:*:*"
    ]
  }
 ]
}
 
 
2\ Launch a new EC2 instance and assign IAM role to it
 
 
3\ Go to cloudwatch  logs  create log groups
 

4\ Login into Ec2 machine and install unified cloud-watch agent and configure app logs.
 
vi /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json
{
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/application.log",
            "log_group_name": "application_logs",
            "log_stream_name": "{instance_id}",
            "timestamp_format": "%b %d %H:%M:%S",
            "initial_position": "start_of_file",
            "buffer_duration": 5000
          }
        ]
      }
    },
    "log_stream_name": "{instance_id}",
    "force_flush_interval": 5
  }
}
==============================================================
4\ Run,
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config \
  -m ec2 \
  -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json \
  -s
 
5\ Touch application.log file to collect logs in created loggroup
 
6\ Go to created loggroup and you can see appended log entries
 
7\ Create log metric filter to trigger alert for keywork “ERROR”

8\ Setup an alarm to receive alert based on log filter
Go to  Alarm  All Alarm  Create Alarm  Sources  Loggroup  browse created loggroup  Select new SNS  Mail  Give mail id
 
9\ Manually append ERROR test in application.log to trigger alarm
 
10\ See loggroup to collect those ERROR pattern log
 

11\ Alarm will be triggered
 
12\ Mail has been triggered finally
