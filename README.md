# Integration of Commons with Java EE Application

This document explains how to use the Encryption feature of the Interswitch Commons Library.
The encryption Library intercepts encrypted data, decrypts the data and Injects the content on your Resource Endpoint.
There are Four main steps to get this working in your project
1) Configure your properties file
2) Add the dependency to your pom
3) Implement the Algorithm Interface and provide the Algorithm to be used
4) Annotate your resource with @Encrypted

After these fours steps, you can test your application.
The details of each step is outlined below:

1) Configure your properties file
minAppVersion (double):  This is the minimum app version that this interceptor supports
stopRequestBelowAppMinVersion (boolean) : This tells the interceptor if it should reject request request that has the appversion below the minAppVersion

2) Add the dependency to your pom.xml
 <dependency>
        <groupId>com.vanso.proxy</groupId>
        <artifactId>proxy-commons</artifactId>
        <version>0.0.6-TEST1.11</version>
    </dependency>
3) Implement the algorithm interface which has the following definition

public interface EncryptionAlgorithm
{
    public String decrypt(String encryptedString, String key);
    public String encrypt(String plainString);
    public String appVersionKey();
    public Object appVersionRequiredError();
    public Object appVersionNotSupportedError();
    public String deviceIdKey();
    public Object deviceIdRequiredError();
}

decrypt(String encryptedString, String key) implement this method with your decryption algorithm agreed between your code and the client
encrypt(String plainString) this contains similar ecryption algorithm that the client uses before making api call to your endpoint resource.
appVersionKey(); specifies the key used in the header for app version
appVersionRequiredError(); returns the error response object if the client fails to send the app version as part of header in the request
appVersionNotSupportedError(); returns the error response object if the client's app version is less than minAppVersion configured in properties file
 deviceIdKey(); The key for the encryption is the device ID. Return the key used for deviceId in request header
  deviceIdRequiredError(); returns the error response object if the client fails to supply the device id
  
4) Annotate your method with @Encrypted

    @Encrypted
    @POST
    @Path("/hello")
    public Response hello (@FormParam("name") String name){
        return Response.status(200).entity("Hi: "+ name).build();
    }

Check the sample code below for how it works.


#### Sample properties file
minAppVersion=1.0
stopRequestBelowAppMinVersion=true

#### Sample Pom.xml
<dependency>
        <groupId>com.vanso.proxy</groupId>
        <artifactId>proxy-commons</artifactId>
        <version>0.0.6-TEST1.11</version>
    </dependency>
    
#### Implement Algorithm Interface
@Stateless
public class EncryptionAlgorithmImpl implements EncryptionAlgorithm
{
    public String decrypt(String encryptedString, String key){
    
    }
    
    public String encrypt(String plainString){
    
    }
    
    public String appVersionKey(){
    
    }
    
    public Object appVersionRequiredError(){
    
    }
    
    public Object appVersionNotSupportedError(){
    
    }
    
    public String deviceIdKey(){
    
    }
    
    public Object deviceIdRequiredError(){
    
    }
}
