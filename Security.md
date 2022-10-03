<br />
<div align="center">
  <h3 align="center">Ansible reporting with ARA: Ansible Run Analysis</h3>
  <p align="center">
    Detailed analysis of the Ansible playbook runs
</div>




<br />
<div align="center">
  <h3 align="center">Set up basic security for the Elasticsearch</h3><br>
  <p align="left">
    In this tutorial, we are going to encrypt traffic between (elasticseach – kibana) and (elasticseach – packetbeat)<br>
</div>

Scenario
   - Primary Server <br>
       Ip adress      : 192.168.1.100<br>
       elasticsearch  : 192.168.1.100:9200<br>
       Kibana         : 192.168.1.100:5601<br>

   - Secondary Server <br>
       Ip adress      : 192.168.1.101<br>

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
  <img src="images/elastic-sertificate.png">
