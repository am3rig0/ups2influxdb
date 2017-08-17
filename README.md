# ups2influxdb
ups2influxdb pushes CyberPower UPS stats to InfluxDB

Container to run Matthew Carey's CyberPower UPS data collection python script  (https://github.com/barrycarey/Cyberpower-UPS-Stats-For-InfluxDB)

**Pre-reqs:** 

- Running instance of InfluxDB
- Running instance of CyberPower PowerPanel Business Edition
- config.ini

**Usage:** 

- populate config.ini with relevant details
- run container

``` docker run -d -v /srv/cyberpower-ups:/data --restart always am3rig0/ups2influxdb ``` 

**config.ini**
``` 
[GENERAL]
Delay = 2
Output = True

[INFLUXDB]
Address = influxdb.localnet
Port = 8086
Database = ups_db
Username = ups
Password = ups_password

[UPS]
Address = 172.16.0.123
Port = 3052

``` 

**Dockerfile**

``` 
FROM python
RUN pip install influxdb
RUN git clone https://github.com/barrycarey/Cyberpower-UPS-Stats-For-InfluxDB.git
RUN mv Cyberpower-UPS-Stats-For-InfluxDB/CyberpowerUpsStats.py /
RUN rm -rf Cyberpower-UPS-Stats-For-InfluxDB
VOLUME /data/
WORKDIR /data/
CMD ["python", "/CyberpowerUpsStats.py"]
``` 