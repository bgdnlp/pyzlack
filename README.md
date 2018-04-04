PyZlack
=======

Python 3 script to send alerts from Zabbix to Slack.

Installation
------------
1. Place the script and config file into Zabbix's [alertscripts directory](
https://www.zabbix.com/documentation/3.4/manual/config/notifications/media/script).

2. Add it as a Script Media Type in Zabbix. Set parameters to:
    * `-b {ALERT.MESSAGE}`
    * `{ALERT.SUBJECT}`

    if you use a config file. Add `-w` otherwise. See `pyzlack -h` for possible parameters.

3. Create a user a user that has the above script set as media.

4. Add an Action under Configuration to call the script.

Subject: `{TRIGGER.STATUS}: {TRIGGER.NAME}`

Default message:
```
Item:: {HOST.NAME1} | {ITEM.NAME1} | {ITEM.VALUE1}
Item:: {HOST.NAME2} | {ITEM.NAME2} | {ITEM.VALUE2}
Item:: {HOST.NAME3} | {ITEM.NAME3} | {ITEM.VALUE3}
Item:: {HOST.NAME4} | {ITEM.NAME4} | {ITEM.VALUE4}
Item:: {HOST.NAME5} | {ITEM.NAME5} | {ITEM.VALUE5}
Item:: {HOST.NAME6} | {ITEM.NAME6} | {ITEM.VALUE6}
Item:: {HOST.NAME7} | {ITEM.NAME7} | {ITEM.VALUE7}
Item:: {HOST.NAME8} | {ITEM.NAME8} | {ITEM.VALUE8}
Item:: {HOST.NAME9} | {ITEM.NAME9} | {ITEM.VALUE9}

Trigger_name:: {TRIGGER.NAME}
Trigger_description:: {TRIGGER.DESCRIPTION}
Trigger_status:: {TRIGGER.STATUS}
Trigger_severity:: {TRIGGER.SEVERITY}
Trigger_nseverity:: {TRIGGER.NSEVERITY}
Trigger_URL:: {TRIGGER.URL}
Event_ID:: {EVENT.ID}
Event_age:: {EVENT.AGE}
Event_ack:: {EVENT.ACK.STATUS}
```

Recovery message should be the same.

Set the Action to be sent to the user created in step 3.

Configuration file
------------------
The script can read an ini-style config file. Configuration options:
```
[DEFAULT]
webhook_url = https://hooks.slack.com/services/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
send_to = #monitoring
username = Zabbix
emoji = :loudspeaker:
```

**webhook_url**: URL for Slack [incoming webhook](https://api.slack.com/incoming-webhooks).

**send_to**: #channel or @user to send the alerts to. If the string doesn't start with # or @, it's assumed to be a channel and # will be prepended.

**username**: Name of the user the message will appear to be coming from.

**emoji**: Slack emoji to use as an icon for the message. Must start and end in colons. Custom emojis can easily be added to Slack.

**icon_url**: URL to an icon.

The config file can have multiple sections and the script can be pointed to a different section using `-s`. This way any combination of parameters can be used to send to different channels or users or different icons, etc. Multiple media types can be set up in Zabbix, one for each section.

Script parameters
-----------------
See `pyzlack -h`. The only mandatory parameter is the text that will be sent to Slack. Webhook URL can be specified either as a parameter or in the config file. If `-b` isn't present the message will have no attachement, only the text.
