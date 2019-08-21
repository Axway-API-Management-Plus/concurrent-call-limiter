
# API Concurrency call limiter

Axway API-Manager allows you to easily apply quotas to your APIs on System- and Application-Level which makes sure, that
only a configured number of requests will be processed by an API or API-Method in a configured timeframe.  
However, in some situations backend-applications providing the services, may not be fully functional 
(e.g. during maintenance, etc.) and perform API-Requests slower than expected. In that situations, more API-Requests 
are coming in, than backend-service can process. As a result a queue of so called Inflight-API-Requests 
is growing up, until all API-Worker threads in the API-Gateway are used. The API-Gateway becomes unresponsive also for other APIs.    

In that situations not the number of API-Requests over time matters, instead it is important to 
limit the total number of concurrent/parallel API-Requests.  

The Request- and Routing-Policies delivered with this projects allow you to configure by API, by API-Method 
or if nothing specific is configured as a default a concurrency limit. The maximum number of Inflight-Requests for an API.
The configuration is saved inside the Key-Property-Store (KPS) and validated at runtime by the API-Gateway.  


## API Management Version Compatibilty
This artefact can be used with Axway API Management version 7.5.3 and higher

## Prerequisites
Nothing special

## Installation
If you would like to protect your backend-systems from getting overloaded by too many API-Requests at the same time or make sure a slow-perfoming APIs isn't slowing down your platform, you can install the so called Call-Limiter policies delivered with this project by the following steps:
1. Clone this project on your local harddisk  
`git clone https://github.com/Axway-API-Management-Plus/concurrent-call-limiter.git`
2. Import the Policy-Fragment from file: `src/API-Call-Limiter-Polices.xml`  
This gives you two new policies in container: `Plattform Protection Tools`:  
`MaxConcurrentApiRequestsRequestPolicy` & `MaxConcurrentApiRequestsResponsePolicy`  
3. Additionally a Cache-Config is imported used to store the actual number of Inflight-API-Requests  
4. Import the KPS-Collection from file: `src/API-Call-Limiter-KPS.xml`  
5. The new Policies are meant to be API-Manager Request- and Response-Policies and executed on every API-Request.   
As it is very likely, that you are already using Request- and Response-Policies in your API-Manager, 
you need to wire up the new Call-Limiter-Policies into your existing policies for instance using a 
Policy-Shortcut.   
If you are using API-Manager version 7.6.2 or higher it's recommended to register the Call-Limiter-Policies as 
Global Request- and Response-Policies to be executed by every API.  
6. Once you have imported everything and deployed the configuration set, you can configure the behavior of the Call-Limiter.  

__Important note:__ You have to use both, the Request- AND Response-Policy, as the requests-policy is increment 
the Inflight-Number and the Response-Policy is decrementing it.  

This is an overview about what you have after you have imported both XML-Fragments:
![Call-Limiter-Components](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/API-Call-Limiter-Components.png)

## Configuration
Assuming you have correctly configured the call limiter policies and they are now running during API request processing, a globally configured threshold of 100 concurrent requests per API! is already active: 
![Global-Default](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/call-limiter-default-limit-execution.png)

In order to configure the total number of concurrent API-Requests you can use KPS-Web-UI or REST-API provided by 
the API-Gateway Manager:
![Global-Default](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/kps-collection-concurrency-limit.png)

### Configure a global default
To override the hardcoded default limit of 100 concurrent requests create the following `default` entry into the table: Concurrency Limit:
![KPS-Default](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/kps-default-limit.png)

### Configure an API limit
To define a limit for an API, no matter which API-Method is called, create an entry like this:
![API-Limit](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/sample-stockquote-limit.png)
The API-Path you define is the exposure path of the API-Manager API. Not the path of the Backend-API.  
![API-Limit](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/api-exposure-path.png)  


### Configure an API-Method limit
Lastly you can define a limit on a per method basis like this:
![API-Limit](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/sample-stockquote-current-limit.png)

The columns comment and lastUpdate are just text-field and fully optional.  

## Runtime-Example Audit-Messages  
#### Default configured in KPS  
![API-Limit](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/audit_messages_kps_default.png)
#### Generic API-Limit configured    
![API-Limit](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/audit_messages_api-limit.png)
#### Too many concurrent API-Requests    
![API-Limit](https://github.com/Axway-API-Management-Plus/concurrent-call-limiter/blob/master/images/audit_messages_too_many.png)

## Changelog
- 0.0.1 - 21.08.2019
  - Initial version


## Limitations/Caveats
- Define a limit per Methods using different HTTP-Verbs

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management-Plus/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"


## License
[Apache License 2.0](/LICENSE)
