# multinode-elastic-stack
Mulltinode elastic stack with ssl encryption

chmod -R 777 cert 
cd cert 
docker-compose up -d 
docker exec -it escert bash

bin/elasticsearch-certutil ca --pem --out cert/cert.zip
unzip cert/cert.zip -d cert/
bin/elasticsearch-certutil cert  --pem --ca-cert cert/ca/ca.crt --ca-key cert/ca/ca.key --in cert/instance.yml --out cert/intial_cert.zip
unzip cert/intial_cert.zip -d cert/
exit

sudo chmod -R 777 ../cert 

cd elastic/es01
docker-compose up -d

cd elastic/es02
docker-compose up -d

docker exec -it es01 bash
bin/elasticsearch-setup-passwords auto

![image](https://user-images.githubusercontent.com/95764498/212750752-eb5fa0d8-a80d-46f4-9835-273aa7deac34.png)

 
exit

curl -u elastic:KvEkRC28xAbiXLsUzCAt --cacert cert/ca/ca.crt  https://es01.com:19200

curl -u elastic:KvEkRC28xAbiXLsUzCAt --cacert cert/ca/ca.crt  https://es02.com:29200

docker exec -it escert bash

bin/elasticsearch-certutil cert --pem --ca-cert cert/ca/ca.crt --ca-key cert/ca/ca.key -multiple --out cert/es03.zip

![image](https://user-images.githubusercontent.com/95764498/212749985-4df69bab-f8aa-4e8d-9aed-d0a3f56882b4.png)

unzip cert/es03.zip -d cert/

![image](https://user-images.githubusercontent.com/95764498/212750025-49602641-8bc6-4f92-9fc8-b755fa3df95e.png)

 
exit

sudo chmod -R 777 ../cert

cd elastic/es03
docker-compose up -d

curl -u elastic:KvEkRC28xAbiXLsUzCAt --cacert cert/ca/ca.crt  https://es03.com:39200

![image](https://user-images.githubusercontent.com/95764498/212750073-dd30ddb9-0f43-4760-81e0-99df2202f781.png)

curl -u elastic:KvEkRC28xAbiXLsUzCAt --cacert cert/ca/ca.crt  https://es03.com:39200/_cluster/health?pretty
 
![image](https://user-images.githubusercontent.com/95764498/212750093-cf71a01c-48f9-4462-bf68-8230bf6494e3.png)

curl --cacert ~/multinode-elastic-stack/cert/ca/ca.crt -u elastic 'https://es01.com:19200/_cat/nodes?v'

![image](https://user-images.githubusercontent.com/95764498/213105521-988c5eb0-0d69-407e-a943-86f3949f562c.png)














