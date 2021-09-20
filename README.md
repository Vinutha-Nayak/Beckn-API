# ONDC API developed by NSDL eGov complied with Beckn Protocol
This page will contains the details of ONDC APIs Adaptors developed by NSDL eGov complied with Beckn Protocol

# Table of contents
1. [Introduction](#introduction)
2. [ONDC API Adaptor Flow](#paragraph1)
3. [Deployment](#paragraph1)
4. [Postgres DB script](#paragraph1)
5. [Sample Postman Collection](#paragraph1)
6. [Configuration Details](#paragraph1)
7. [NSDL BG Configuration Details](#paragraph1)
8. [NSDL Sandbox](#paragraph1)

# 1. Introduction
ONDC aims at promoting open networks developed on open-sourced methodology, using open specifications and open network protocols independent of any specific platform. NSDL eGov is helping building the ONDC API adaptors which is powered by Beckn Protocols for the netowrk participants who can easily run and integrate with their applications.
# 2. ONDC API Adaptor Flow
![alt text](https://github.com/dhiraj-nsdl/Beckn-API/blob/main/image/Adaptor%20Architecture%20flow%20updated.png)
# 3. Deployment
Two options are available:
1. Default - Run it as a microservice. (It can be also docker enabled) 
2. Integration with Java application. (Connect to NSDL eGov for more details)
# 4. Postgres DB script
Postgres DB scripts have been provided who wish to capture API transaction details in their database for audit.
# 5. Sample Postman Collection
Postman collection have been provided with sample json for each API services.
# 6. Configuration Details
  a. BAP - buyer node Configuration ("buyer node" compliant with Beckn BAP specifications)
1. Application.yml 
```bash
      a) server:
          port: 8080
      b) beckn:
          persistence:
                 type: http|db-postgres
                 audit-schema-error: true
          entity:
                     type: buyer
```
Description :
1. port: it is server port number on which this jar will run
2. beckn.persistence.type: the pipe separated value for persistence strategy. Currently allowed values are http & db-postgres. Value http means response will be pushed to the        url mentioned in the  http_entity_endpoint parameter. If db-postgres used then response will be saved in database. Any other value is not allowed.
3. beckn.entity.type: the allowed values are bap or bpp. Depending on the value provided, jar will all auto configuration internally and start working accordingly . Any other        value is not allowed.
    
    2. adaptor-config-bap.json
```bash
  Sample Json
        {
            "keyid": "nsdl.co.in|nsdl_bpp_1",
            "private_key": "XXXXXX",
            "api": [
              {
                "name": "on_search",
                "http_entity_endpoint": "http://localhost:8079/buyer/mock/on_search",
                "http_timeout": 1000,
                "http_retry_count": 3,
                "header_validity": 600000,
                "header_authentication": true
              },
               {
                "name": "on_select",
                "http_entity_endpoint": "http://localhost:8079/buyer/mock/on_select",
                "http_timeout": 1000,
                "http_retry_count": 3,
                "header_validity": 600000,
                "header_authentication": true
              },
              {
                "name": "lookup",
                "http_entity_endpoint": "https://pilot-gateway-1.beckn.nsdl.co.in/lookup",
                "http_timeout": 5000,
                "http_retry_count": 0,
                "header_validity": 600000,
                "header_authentication": true
              }
            ]
        }
```
Description: (As given in above example, add call back url configuration in array)
1. keyid: it is the id that is used while registering as buyer to beckn.
2. private_key: it is the private key of the buyer. This will be used while signing the authorization header.
3. name: name of the api like search/select/init.
4. http_entity_endpoint: http url for the entity endpoint of buyer.
5. http_timeout: http call timeout in milliseconds .
6. http_retry_count: http retry count in case of timeout error.
7. header_validity: auth header validity in milliseconds.
8. header_authentication: auth header validation check. Allowed values true or false. If false auth header validation will be skipped.

b. BPP - seller node Configuration ("seller node" compliant with Beckn BPP specifications)
1. Application.yml 
```bash
      a) server:
          port: 8080
      b) beckn:
          persistence:
                 type: http|db-postgres
                 audit-schema-error: true
          entity:
                     type: seller
```  
Description :
1. port: it is server port number on which this jar will run
2. beckn.persistence.type: the pipe separated value for persistence strategy. Currently allowed values are http & db-postgres. Value http means response will be pushed to the        url mentioned in the  http_entity_endpoint parameter. If db-postgres used then response will be saved in database. Any other value is not allowed.
3. beckn.entity.type: the allowed values are bap or bpp. Depending on the value provided, jar will all auto configuration internally and start working accordingly . Any other        value is not allowed.
    
    2. adaptor-config-bpp.json
```bash
Sample Json
       {
            "keyid": "nsdl.co.in|nsdl_bpp_1",
            "private_key": "XXXXXXX",
            "api": [
              {
                "name": "search",
                "http_entity_endpoint": "http://localhost:8079/seller/mock/search",
                "http_timeout": 1000,
                "http_retry_count": 3,
                "header_validity": 600000,
                "header_authentication": true
              },
              {
                "name": "select",
                "http_entity_endpoint": "http://localhost:8079/seller/mock/select",
                "http_timeout": 1000,
                "http_retry_count": 3,
                "header_validity": 600000,
                "header_authentication": true
              },
              {
                "name": "lookup",
                "http_entity_endpoint": "https://pilot-gateway-1.beckn.nsdl.co.in/lookup",
                "http_timeout": 5000,
                "http_retry_count": 0,
                "header_validity": 600000,
                "header_authentication": true
              }
            ]
        }
```  
Description: (As given in above example, add call back url configuration in array)
1. keyid: it is the id that is used while registering as seller to beckn.
2. private_key: it is the private key of the seller. This will be used while signing the authorization header.
3. name: name of the api like search/select/init. 
4. http_entity_endpoint: http url for the entity endpoint of seller. 
5. http_timeout: http call timeout in milliseconds .
6. http_retry_count: http retry count in case of timeout error.
7. header_validity: auth header validity in milliseconds.
8. header_authentication: auth header validation check. Allowed values true or false. If false auth header validation will be skipped.
  
# 7. BG - gateway node/Registry node Configuration Details
To get register at BG - gateway node/Registry node below field details required:
1. subscriber_id
2. Domain
3. Entity Type (BAP/BPP)
4. CallBack URL for On_Subscribe
5. Signing Public Key

The end points for BG Gateway Node are as below: ("gateway node" compliant with Beckn gateway specifications) <br />
a. Search : https://pilot-gateway-1.beckn.nsdl.co.in/search <br />
b. On_search : https://pilot-gateway-1.beckn.nsdl.co.in/on_search

The end points for Registry Node are as below: ("registry node" compliant with Beckn registry specifications) <br />
a. https://pilot-gateway-1.beckn.nsdl.co.in/subscribe <br />
b. https://pilot-gateway-1.beckn.nsdl.co.in/lookup

Mail your details at : dhirajp@nsdl.co.in

# 8. NSDL Sandbox
1. BG - Gateway Node:
As mentioned in section 7, provide details to get register with BG gateway node.

The end points for BG Gateway Node are as below: <br />
a. Search : https://pilot-gateway-1.beckn.nsdl.co.in/search <br />
b. On_search : https://pilot-gateway-1.beckn.nsdl.co.in/on_search

2. Registry Node:
As mentioned in section 7, provide details to get register with BG registry node.

The end points for Registry Node are as below: <br />
a. https://pilot-gateway-1.beckn.nsdl.co.in/subscribe <br />
b. https://pilot-gateway-1.beckn.nsdl.co.in/lookup
