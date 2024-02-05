#  How to secure Mosquitto MQTT
1. change conf file  /etc/mosquitto/conf.d/default.conf
  ```
  allow_anonymous false
  password_file /etc/mosquitto/passwd
  listener 1883 localhost
  listener 8883
  cafile /opt/certs/ca.crt
  certfile /opt/certs/bothwin.hopto.org.server.crt
  keyfile  /opt/certs/bothwin.hopto.org.server.key
  require_certificate true
  use_identity_as_username true
  ```

2. create selfs-gned ca certificate
   ```
   openssl genrsa -des3 -out ca.key 2048
   openssl req -new -x509 -days 1826 -key ca.key -out ca.crt   
   ```
   
3. create server certificate
   ```
   openssl genrsa -out server.key 2048
   openssl req -new -out server.csr -key server.key
   openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 360
   ```

   Common Name  :  server.example.com   <------- match   server.example.com:8883
   
4. create client certificate
   ```
   openssl genrsa -out client.key 2048
   openssl req -new -out client.csr -key client.key
   openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 360
   ```
   
   Common Name  :  client.example.com   <------- match   client id
   
