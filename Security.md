
<br />
<div align="center">
  <h3 align="center">Set up basic security for the Elasticsearch</h3><br>
  <p align="left">
    In this tutorial, we are going to encrypt traffic between (elasticseach – kibana) and (elasticseach – packetbeat) in cluster environment<br>
</div>

Scenario
   - Primary Server <br>
       Ip adress      : 192.168.1.100<br>
       elasticsearch  : 192.168.1.100:9200<br>
       Kibana         : 192.168.1.100:5601<br>

   - Secondary Server <br>
       Ip adress      : 192.168.1.101<br>
       elasticsearch  : 192.168.1.101:9200<br>

   <img src="images/packetbeat1.JPG">
   
## [Elasticsearch]
#### Create Certificate Directory (Primary Server & Secondary Server)
   ```sh
  	mkdir /etc/elasticsearch/config/
  ```
#### Generate TLS Certificate (Primary Server)
   ```sh
  	/usr/share/elasticsearch/bin/elasticsearch-certutil cert
    out : /etc/elasticsearch/config/elastic-certificates.p12
    pass  : ""
    or
    /usr/share/elasticsearch/elasticsearch-certutil cert -out config/elastic-certificates.p12 -pass ""
  ```
  <img src="images/cluster_secure.JPG">

#### Add privileges (Primary Server)
   ```sh
  	chown -R elasticsearch:elasticsearch /etc/elasticsearch/config/config
  ```
#### TLS on Elasticsearch (Primary Server)
Add this lines on /etc/elasticsearch/elasticsearch.yml  (Primary server)
   ```sh
  	xpack.security.enabled: true
    xpack.security.transport.ssl.enabled: true
    xpack.security.transport.ssl.verification_mode: certificate 
    xpack.security.transport.ssl.keystore.path: config/elastic-certificates.p12 
    xpack.security.transport.ssl.truststore.path: config/elastic-certificates.p12 
  ```
#### Transfer config/elastic-certificates.p12 to Secondary server (Primary server)
   ```sh
  	scp -R /etc/elasticsearch/config/elastic-certificates.p12 192.168.1.101:/etc/elasticsearch/config/
  ```
#### set permission (Secondary server)
   ```sh
    chown -R elasticsearch:elasticsearch /etc/elasticsearch/config/
  ```
#### Creat password for elastic cluster (Primary server)
   ```sh
  	/usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto
  ```
  result: (nb : the following password generated randomize from elasticsearch-setup-passwords )
   ```sh
Changed password for user apm_system
PASSWORD apm_system = x2YhXMDfXnJwa8VFZ7rl
Changed password for user kibana_system
PASSWORD kibana_system = OnGPpEa9rF5rZE72bpqM
Changed password for user kibana
PASSWORD kibana = G7pPrZE2b5rMFEa9nqOp
Changed password for user logstash_system
PASSWORD logstash_system = IPQn9tDF9nZ4ZQgkuoUb
Changed password for user beats_system
PASSWORD beats_system = mANJ1OVhMBUhRk45Rjlm
Changed password for user remote_monitoring_user
PASSWORD remote_monitoring_user = oGz552Zyg5ED8vYdnSkh
Changed password for user elastic
PASSWORD elastic = XiAqjTzAZ8rXOKzc1bI5

  ```
#### Creat password for elastic cluster (Secondary Server) [for testing purpose chose the same passwords as Primary server]
/usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive

  <img src="images/elasticsearch-setup-passwords.JPG">
