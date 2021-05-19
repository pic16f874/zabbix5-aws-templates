# Zabbix ECS and RDS Templates

This is a template and script that will allow you to monitor ECS clusters and their services in Zabbix.

This started from datorama's and Pahanda's monitoring scripts and templates:
* https://github.com/Pahanda/zabbix-templates-ecs
* https://github.com/datorama/zabbix_rds_template

Changes:
changes related zabbix5 and python3

Requirements:
* Zabbix Server (tested with 5.0.11).
* Installed on the server:
  * Python3 
  * python3-boto3 library for Python3 (apt install python3-boto3 on Ubuntu 20.04)
* IAM user credentials with read access to CloudWatch metrics.  You have two choices on how to provide them to the script:
  * You can store them server-side using the instructions in the boto3 documentation.  Note that the script will be running as the zabbix user.
  * You can also store them as Zabbix macros.  If you do this, it is _strongly_ recommended that you create a separate IAM user with read-only permissions (you can even limit it as far as allowing only "cloudwatch:GetMetricStatistics", if you're particularly paranoid).  This is because the access and secret keys will be stored in plaintext on the Zabbix Server and may be visible in Latest Data views and alert emails, depending on your configuration.

Installation and Setup:
* Copy the [ecs_stats3.py](/ecs_stats3.py) and  [rds_stats3.py](/rds_stats3.py)  scripts to your Zabbix externalscripts directory (usually `/usr/lib/zabbix/externalscripts`, but check your Zabbix Server configuration if you're not sure).  Make sure it has executible permissions.
* Create symlinks to *.py files (templates use ecs_stats.py and rds_stats.py names)
* Import the [zbx_Template_AWS_ECS_cluster.xml](/zbx_Template_AWS_ECS_cluster.xml) and [zbx_Template_AWS_RDS.xml](/zbx_Template_AWS_RDS.xml) templates into your Zabbix Server.
* Create a host matching the ClusterName of your ECS cluster and attach the "Template Host AWS ECS" template to it.
* Set up the AWS credentials and default region either server-side (for the zabbix user) or as macros attached to your host.  If you're doing it with macros, make sure to set:
  * `{$AWS_ACCESS_KEY}`
  * `{$AWS_SECRET_KEY}`
  * `{$REGION}`
  
A few considerations:
* The Low-Level Discovery currently runs every 1 hour.
* Added triggers and graphs
