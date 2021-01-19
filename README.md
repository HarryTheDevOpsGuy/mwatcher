# mWatcher Version
 **Version**        : v0.1.2 <br>
 **Release Date**   : 19-Jan-21 <br>

# Welcome to mWatcher!

 **mWatcher** is a script to monitor **Linux server health** . it allow you to generate html report and send email notification when your server resource( Memory, Disk, CPU ) Utilization goes high. You can generate full report of your linux server.


# Getting Start
 ![mWatcher email alert templates](https://raw.githubusercontent.com/HarryTheDevOpsGuy/mwatcher/master/assets/img/mwatcher.png)

It will help you to monitor bulk domain's ssl. it required internet access to fetch certificate details .


#### Prerequisite
if you want get notification on your email you must need to install mSlack. if you want to get slack notification you must need to install mSlack accordingly. or you can install and configure both if you wish to get notification on both.

 - mSend - To get email notification - [Installation steps](https://github.com/HarryTheDevOpsGuy/mSend)
 - mSlack - To get slack notification - It will install automatically.


##### Step 1: Install mwatcher.
```bash
sudo curl -sL "https://github.com/HarryTheDevOpsGuy/mwatcher/raw/master/$(uname -p)/mwatcher" -o /usr/bin/mwatcher
sudo chmod +x /usr/bin/mwatcher

# Verify installation
mwatcher -v
```


## Help

```bash
mwatcher -h

mwatcher [OPTION]
  -e email            Send Email Notification
  -s "#devops"        Send Slack Notification
  -u units:action     monitor service
  -m 80:alarmtype     Notify if used momory >= 80% ( alerm type warning, critical, etc)
  -c 80:alarmtype     Notify if used cpu  >= 80% ( alerm type warning, critical, etc)
  -d 80:alarmtype     Notify if used disk >= 80% ( alerm type warning, critical, etc)
  -a file_path        Aattach file with email Notification.
  -x interval         Repeat alert Interval in minutes.
  -h                  Display this help
  -v                  Display version
```


## DEFAULT VARIABLES | CONFIGURATIONS
You can edit default threshold rules file `/etc/mwatcher/rules.sh`

```bash
# vim /etc/mwatcher/rules.sh
### User defined variables
## For more details visit https://github.com/HarryTheDevOpsGuy/mwatcher

REPEAT_NOTIFICATION_INTERVAL="30 minutes"
LOG_DIR="/tmp/mwatcher.log"
# Time Interval to check CPU MEMORY DISK Utilization to avaid spikes
TIME_INTERVAL="2" # 2 Seconds

# Threshold configure ( default no Threshold)
MEMORY_ALARM=( 70:info 80:Warning 90:Critical )
CPU_ALARM=(70:info 80:Warning 90:Critical )
DISK_ALARM=( 70:info 80:warning 90:Critical )
#SERVICE_STATUS=( nginx:restart mysql ssh )

# Email Notification
EMAIL_NOTIFICATION="false"
FULL_HTML_REPORT_INTERVAL="1 days"
#EMAIL_NOTIFY_TO=( youremail@gmail.com )

# Slack Notification
SLACK_NOTIFICATION="false"
# export SLACK_CLI_TOKEN=xoxb-343434345454-sdlfkdf-kdfde-example-token
# export SLACK_CHANNEL='#devops'
```


### Example 1 -  To trigger alert based on threshold on slack and email

If you want to get alert on your slack and email. it will send you full html report on your email id once in day. also it will notify you if any resource (cpu, memory, disk) utilization goes high. you can use command arguments multiple times.

```bash
mwatcher -c 80:warning -d 80:critical -m 80:warning -s '#SLACK_CHANNEL' -e yourname@domain.com
```
  * `-c 80:warning` - `WARNING` trigger alert if `CPU` usage greater than `80%`.
  * `-d 80:critical` -  `CRITICAL` alert if `DISK` usage greater than `80%`.
  * `-m 90:critical` - `CRITICAL` alert if `MEMORY` usage greater than 90%
  * `-s '#SLACK_CHANNEL'` - Notify on Slack channel
  * `-e yourname@domain.com` - Notify on email.
  * `Note :` - it will send you full html report daily.



### Example 2 -  To Generate html report only.
You can generate html report only. this command will not trigger any alert.

```bash
mwatcher /tmp/daily_report.html
```

## Configure Cronjobs To Get email alert daily or weekly.
You can schedule cronjob according to your choice. Here is few example for you. you can use any of one.

	# Schedule cron every 5 min for memory monitoring
	*/5 * * * * /usr/bin/mwater -m 80:warning -m 90:critical -s '#SLACK_CHANNEL' -e yourname@domain.com

  #### Email Notification Template | Report

   ![mWatcher email alert templates](https://raw.githubusercontent.com/HarryTheDevOpsGuy/mwatcher/master/assets/img/email_snapshot.png)


  #### Thanks you
