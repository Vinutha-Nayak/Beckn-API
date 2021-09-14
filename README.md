# ONDC API developed by NSDL eGov complied with Beckn Protocol
This page will contains the details of ONDC APIs Adaptors developed by NSDL eGov complied with Beckn Protocol

# Table of contents
1. [Introduction](#introduction)
2. [ONDC API Adaptor Flow](#paragraph1)
3. [Postgres DB script](#paragraph1)
4. [Sample Postman Collection](#paragraph1)
5. [Configuration Details](#paragraph1)
6. [NSDL BG Configuration Details](#paragraph1)

# 1. Introduction
ONDC aims at promoting open networks developed on open-sourced methodology, using open specifications and open network protocols independent of any specific platform. NSDL eGov is helping building the ONDC API adaptors which is powered by Beckn Protocols for the netowrk participants who can easily run and integrate with their applications.
# 2. ONDC API Adators Flow
![alt text](https://github.com/dhiraj-nsdl/Beckn-API/blob/main/image/Adaptor%20Architecture%20flow%20updated.png)
# 3. Postgres DB script
Postgres DB scripts have been provided who wish to capture API transaction details in their database for audit.
# 4. Sample Postman Collection
Postman collection have been provided with sample json for each API services.
# 5. Configuration Details
  a. ONDC Buyer Node Configuration
1. Application.yml 
```bash
      a) server:
          port: 8080
      b) beckn:
          persistence:
                 type: http|db-postgres
          entity:
                     type: bap
```
Description :
1. port: it is server port number on which this jar will run
2. beckn.persistence.type: the pipe separated value for persistence strategy. Currently allowed values are http & db-postgres. Value http means response will be pushed to the        url mentioned in the  http_entity_endpoint parameter. If db-postgres used then response will be saved in database. Any other value is not allowed.
3. beckn.entity.type: the allowed values are bap or bpp. Depending on the value provided, jar will all auto configuration internally and start working accordingly . Any other        value is not allowed.
    
    2. adaptor-config-bap.json
```bash
         {
           "keyid": “xxx",
           "callbackurladaptor": “xxx",
           "callbackurlapi": “xxx",
           "privatekey": “xxx",
           "algo": "ed25519",
           "timeout": 1000,
           "retrycount": 3,
           "authenticate": true
          }
```
Description:
1. keyid: it is the id that is used while registering as BAP to beckn.
2. callbackurladaptor: it is the call back url on which the BPP will send the response.
3. callbackurlapi: it is the url of BAP to which the adaptor will finally send the response of callback. If not provided the callback response will end in adaptor.
4. privatekey: it is the private key of the BAP. This will be used while signing the authorization header.
5. algo: it is the algorithm used while signing the authorization header. Should be ed25519.
6. timeout: it is the timeout in milliseconds used while making post call to BPP.
7. retrycount: number of time the retry should occur incase of timeout.
8. authenticate: check for verification of authorization header. It takes only boolean value.

b. ONDC Seller Node Configuration
1. Application.yml 
```bash
      a) server:
          port: 8080
      b) beckn:
          persistence:
                 type: http|db-postgres
          entity:
                     type: bpp
```  
Description :
1. port: it is server port number on which this jar will run
2. beckn.persistence.type: the pipe separated value for persistence strategy. Currently allowed values are http & db-postgres. Value http means response will be pushed to the        url mentioned in the  http_entity_endpoint parameter. If db-postgres used then response will be saved in database. Any other value is not allowed.
3. beckn.entity.type: the allowed values are bap or bpp. Depending on the value provided, jar will all auto configuration internally and start working accordingly . Any other        value is not allowed.
    
    2. adaptor-config-bpp.json
```bash
         {
           "keyid": “xxx",
           "callbackurladaptor": “xxx",
           "callbackurlapi": “xxx",
           "privatekey": “xxx",
           "algo": "ed25519",
           "timeout": 1000,
           "retrycount": 3,
           "authenticate": true
          }
```  
Description:
1. keyid: it is the id that is used while registering as BPP to beckn.
2. callbackurladaptor: it is the call back url on which the BPP will send the response.
3. callbackurlapi: it is the url of BPP to which the adaptor will forward the request after validation.
4. privatekey: it is the private key of the BPP. This will be used while signing the authorization header.
5. algo: it is the algorithm used while signing the authorization header. Should be ed25519.
6. timeout: it is the timeout in milliseconds used while making post call to BAP as part of callback.
7. retrycount: number of time the retry should occur incase of timeout.
8. authenticate: check for verification of authorization header. It takes only boolean value.
  
# 6. ONDC Gateway/Registry Node Configuration Details
To get register at ONDC Gateway/Registry Node below field details required:
1. subscriber_id
2. Domain
3. Entity Type (BAP/BPP)
4. CallBack URL for On_Subscribe
5. Signing Public Key

The end points for ONDC Gateway Node are as below:
1. Search : https://pilot-gateway-1.beckn.nsdl.co.in/search
2. On_search : https://pilot-gateway-1.beckn.nsdl.co.in/on_search

Mail your details at : dhirajp@nsdl.co.in
