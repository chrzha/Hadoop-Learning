# Enable https in springboot in development
Ref: https://www.thomasvitale.com/https-spring-boot-ssl-certificate/

# Getting an SSL certificate
```bash
keytool -genkey -keyalg RSA -alias thekeystore -keystore thekeystore -storepass changeit -validity 360 -keysize 2048
keytool -export -alias thekeystore -file thekeystore.crt -keystore thekeystore
keytool -import -alias thekeystore -storepass changeit -file thekeystore.crt -keystore "C:\Program Files\Java\jdk1.8.0_91\jre\lib\security\cacerts"
```

#Configuring SSL in Spring Boot

```bash
## configuration in springboot 2
server:
  ssl:
    key-store: classpath:thekeystore
    key-store-password: changeit
    key-store-type: jks
    key-alias: thekeystore
    key-password: changeit
  port: 8443
```
