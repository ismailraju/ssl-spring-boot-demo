## SSL Spring-boot Demo

### Check keystore in env path
```keytool -help``` otherwise go to this path 
```cd %JAVA_HOME%\bin``` and run ```keytool -help```.


The two most common formats used for keystores are JKS, a proprietary format specific for Java, and PKCS12, an industry-standard format. JKS used to be the default choice, but since Java 9 it's PKCS12 the recommended format

 #### Generate JKS keystore:

```keytool -genkeypair -alias springboot -keyalg RSA -keysize 4096 -storetype JKS -keystore springboot.jks -validity 3650 -storepass password```


 #### Generate PKCS12 keystore:
 
 
 ```keytool -genkeypair -alias springboot -keyalg RSA -keysize 4096 -storetype PKCS12 -keystore springboot.p12 -validity 3650 -storepass password```
 
 
 PS C:\Users\Asus\Downloads\ssl-demo\ssl-demo> keytool -genkeypair -alias springboot -keyalg RSA -keysize 4096 -storetype PKCS12 -keystore springboot.p12 -validity 3650 -storepass password
 What is your first and last name?
   [Unknown]:  Ismail Raju
 What is the name of your organizational unit?
   [Unknown]:  Software Firm
 What is the name of your organization?
   [Unknown]:  Ismail Soft
 What is the name of your City or Locality?
   [Unknown]:  Dhaka
 What is the name of your State or Province?
   [Unknown]:
 What is the two-letter country code for this unit?
   [Unknown]:  BD
 Is CN=Ismail Raju, OU=Software Firm, O=Ismail Soft, L=Dhaka, ST=Unknown, C=BD correct?
   [no]:  yes
   
   
   
   Let's have a closer look at the command we just run:
   
   genkeypair: generates a key pair;
   alias: the alias name for the item we are generating;
   keyalg: the cryptographic algorithm to generate the key pair;
   keysize: the size of the key;
   storetype: the type of keystore;
   keystore: the name of the keystore;
   validity: validity number of days;
   storepass: a password for the keystore.
   
   
   
   
   To check the content of the keystore following the JKS format, we can use keytool again:
   
   keytool -list -v -keystore springboot.jks
   
   
   To test the content of a keystore following the PKCS12 format:
   
   keytool -list -v -keystore springboot.p12
   
   
   
   
   #### Convert a JKS keystore into PKCS12
   Should we have already a JKS keystore, we have the option to migrate it to PKCS12; keytool has a convenient command for that:
  
  
  keytool -importkeystore -srckeystore springboot.jks -destkeystore springboot.p12 -deststoretype pkcs12
  
  
  
  
#### Use an existing SSL certificate
   
 In case we have already got an SSL certificate, for example, one issued by Let's Encrypt, we can import it into a keystore and use it to enable HTTPS in a Spring Boot application.
 
 We can use keytool to import our certificate in a new keystore.
 
 keytool -import -alias springboot -file myCertificate.crt -keystore springboot.p12 -storepass password
 
 
 
 
   
   
#### Redirect to HTTPS with Spring Security
   
   
   
   
#### Multiple connectors for HTTP and HTTPS
    
    Spring allows defining just one network connector in application.properties (or application.yml). We used it for HTTPS and relied on Spring Security to redirect all HTTP traffic to HTTPS.
    
    What if we need both HTTP and HTTPS connectors in Tomcat and redirect all requests to the second one? We can keep the HTTPS configuration in the application.yml file and we set up the HTTP connector programmatically.
    
    
   
   
   
   
   
   
   
#### Distribute the SSL certificate to clients
   
   
   
   
#### Extract an SSL certificate from a keystore
   We have stored our certificate inside a keystore, so we need to extract it.
   
   
   
   keytool -export -keystore springboot.p12 -alias springboot -file myCertificate.crt
   
   
   
   
#### import an SSL certificate inside the JRE keystore
   
   keytool -importcert -file myCertificate.crt -alias springboot -keystore $JDK_HOME/jre/lib/security/cacerts
   
   
   
   
   
   
   
   
   
   
