# fail2ban WordPress

This custom Fail2Ban filter and jail will deal with all scans for common Wordpress Web Exploits being scanned for by automated bots and those seeking to find exploitable web sites.

[![DUB](https://img.shields.io/dub/l/vibe-d.svg)](https://github.com/mitchellkrogza/Fail2Ban.WebExploits/blob/master/LICENSE.md)<img src="https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker/blob/master/.assets/spacer.jpg"/>[![Build Status](https://travis-ci.org/mitchellkrogza/Fail2Ban.WebExploits.svg?branch=master)](https://travis-ci.org/mitchellkrogza/Fail2Ban.WebExploits)<img src="https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker/blob/master/.assets/spacer.jpg"/><a href='https://twitter.com/ubuntu101za'><img src='https://img.shields.io/twitter/follow/ubuntu101za.svg?style=social&label=Follow' alt='Follow @ubuntu101za'></a>

_______________
#### Version: V0.1
#### Total Exploits: XXX
____________________


## How To Use This Filter

### 1 - Copy the webexploits.conf file from the repository to your server

```xxx```

************************************************
### 2 - Create the Jail Config in your jail.local file

```sudo nano /etc/fail2ban/jail.local```

Paste the contents below into your jail.local file

For NGINX

```
[webexploits]
enabled  = true
port     = http,https
filter   = webexploits
logpath = %(nginx_access_log)s
maxretry = 3
```

For APACHE

```
[webexploits]
enabled  = true
port     = http,https
filter   = webexploits
logpath = %(apache_access_log)s
maxretry = 3
```

************************************************
### 3 - Test the filter against some of your log files

```fail2ban-regex /var/log/nginx/myweb-access.log /etc/fail2ban/filter.d/webexploits.conf```

You will see output something like this

```
Running tests
=============

Use   failregex filter file : webexploits, basedir: /etc/fail2ban
Use         log file : /var/log/nginx/mitchellkrog.com-REDIRECTS-access.log
Use         encoding : UTF-8


Results
=======

Failregex: 391 total
|-  #) [# of hits] regular expression
|   1) [105] ^<HOST> -.*GET.*(/.git/config)
|   3) [16] ^<HOST> -.*GET.*(/administrator/index.php)
|   4) [2] ^<HOST> -.*GET.*(/administrator/manifests/files/joomla.xml)
|   6) [6] ^<HOST> -.*GET.*(/ckupload.php)
|   8) [5] ^<HOST> -.*GET.*(/components/com_adsmanager/js/fullnoconflict.js)
....
....
....
|  68) [9] ^<HOST> -.*GET.*(/wp-content/plugins/wysija-newsletters/readme.txt)
|  69) [1] ^<HOST> -.*GET.*(/wp-content/themes/deep-blue/megaframe/megapanel/inc/functions.php)
|  70) [4] ^<HOST> -.*GET.*(/wp-content/themes/u-design/style.css)
`-

Ignoreregex: 0 total

Date template hits:
|- [# of hits] date format
|  [4262] Day(?P<_sep>[-/])MON(?P=_sep)Year[ :]?24hour:Minute:Second(?:\.Microseconds)?(?: Zone offset)?
`-

Lines: 4262 lines, 0 ignored, 391 matched, 3871 missed [processed in 2.50 sec] 
Missed line(s): too many to print.  Use --print-all-missed to print all 3871 lines
```

This confirms the webexploits.conf file is detecting hits in your logs for the exploits it covers.

************************************************
### 4 - Restart the fail2Ban Service

```sudo service fail2ban stop && sudo service fail2ban start```

************************************************
### 5 - Monitor your email for new notifications that this filter will now be sending.



