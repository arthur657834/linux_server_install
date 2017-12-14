```
wget https://dl.influxdata.com/influxdb/releases/influxdb-1.3.7.x86_64.rpm
sudo yum localinstall influxdb-1.3.7.x86_64.rpm -y

vi /etc/influxdb/influxdb.conf
bind-address = "0.0.0.0:8088"

systemctl start influxdb
验证：
influx
Connected to http://localhost:8086 version 1.3.7
InfluxDB shell version: 1.3.7
> show databases;
name: databases
name
----
_internal
> quit

curl "http://10.1.50.199:8086/db?u=root&p=root" -d "{\"name\": \"collectd\"}"

curl -G 'http://10.1.50.199:8086/db/collectd/series?u=root&p=root&q=list+series&pretty=true'

```
