# instantclient-python
<div align=left> 
  <img src="https://img.shields.io/badge/oracle-F80000?style=flat-square&logo=oracle&logoColor=white">
  <img src="https://img.shields.io/badge/docker-2496ED?style=flat-square&logo=docker&logoColor=white">
  <img src="https://img.shields.io/badge/RHEL-EE0000?style=flat-square&logo=red hat&logoColor=white">
  <img src="https://img.shields.io/badge/oraclecloud-F80000?style=flat-square&logo=icloud&logoColor=white">
  <br>
  <br>
</div>

Databases in oraclecloud free-tier are deleted after a certain period of inactivity. To prevent deletion, free-tier users need to update the database periodically. This docker image was created for just such a need.<br>
It provides sqlplus, sqlldr and python by default. Tested on oraclecloud VM.Standard.E2.1.Micro for x86 and VM.Standard.A1.Flex for arm.<br>
Alpine's final build image size was larger than ubuntu's. So this was built with ubuntu.

* * *

### How to use

Build docker image.

```sh
sudo docker build -t instantclient-python /data/instantclient-python/dockerfile
```

Create hr schema through sqlpus or refresh by accessing db every day with python program.
```sh
sudo docker-compose --file docker-compose-d*******.yml rm -f -s && sudo docker-compose --file docker-compose-d*******.yml up -d && sudo docker exec -it instantclient-python-d******* date
sudo docker exec -it instantclient-python-d******* python3 /data/refresh_d*******.py
sudo docker exec -it instantclient-python-d******* sqlplus ADMIN/x*****************************@d*******_high
sudo docker exec -i instantclient-python sqlplus HR/Z*****************************@d*******_high < /data/instantclient-python/data/hr_drop.sql
sudo docker exec -i instantclient-python sqlplus HR/Z*****************************@d*******_high < /data/instantclient-python/data/hr_make.sql
```

Below is an example running using docker. Run it once a day using a scheduler such as crontab.
```sh
#!/bin/bash
log_file=/tmp/refresh_d*******.log
log_file_1=/tmp/refresh_d*******_telegram_1.log
docker run -i --rm --name=instantclient-python-d******* --user=0:0 \
-e TZ=Asia/Seoul \
-v /data/instantclient-python/data:/data:rw \
-v /data/instantclient-python/wallet_d*******:/opt/oracle/instantclient/network/admin:ro \
instantclient-python:latest \
python3 /data/refresh_d*******.py > $log_file
{
  echo "$0"
  cat $log_file
} > $log_file_1
log=$(< $log_file_1 tail -c 4096)
if [ -z "$log" ]; then
  exit 1
fi
/usr/bin/curl --data-urlencode text="$log" https://api.telegram.org/bot**********************************************/sendMessage?chat_id=**********
```

* * *

### Contact and license

<a href="mailto:xqbty8po-dntco43u@yahoo.com" target="_blank"><img src="https://img.shields.io/badge/yahoo!-6001D2?style=flat-square&logo=yahoo!&logoColor=white"/></a>
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
