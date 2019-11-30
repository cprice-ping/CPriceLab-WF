This profile is the configuration for CPriceLab - Workforce use cases.  

On my host, after the initial Profile is pulled for the first time, I mount it as a volume so that it remains updated with any changes made to the configuration (i.e. not stored and pulled from GitHub). This is to show a common deployment method (Containers) using legacy configuration (Pets).

It is designed to be used in conjunction with my PD-Base Profile (https://github.com/cprice-ping/PD-Base), with PD being used for:  
* OAuth Clients
* OAuth Persistant Grants
* Authentication Sessions
* PF Admin Console credentials

## Configuration

**Architecture**  
![CPriceLab Architecture](https://github.com/cprice-ping/CPriceLab-WF/blob/master/CPriceLab%20-%20WF.png)

This configuration includes:

### Adapters
* HTML Form
* Identifier-First (Passwordless)
* PingID


### PingID - Special Considerations
The PingID adapter uses the secrets from your PingID tenant to create the proper calls to the service. As such, storing those values in a public location, such as GitHub, sound be considered **risky**.

For this Profile, you can place the `base64` encoded text from a `pingid.properties` file that will be placed into the PingID Adapter settings 

### Authentication Policy
Extended Property Selector
  * Basic (Kerb \ HTML Form --> PingID)
  * Passwordless (ID-First --> PingID)

The Authentication Experience is controlled by setting the `Extended Properties` on the Application.  

Authentication API
* HTML Form --> AuthN API Explorer  

### Extended Properties
* `Basic` (Kerb \ HTML Form --> PingID)
* `Passwordless` (ID-First --> PingID)
* _Anything Else_ (AuthN API Explorer)

### Applications
**SAML -- HTTPBIN**  
https://${PF_BASE_URL}/idp/startSSO.ping?PartnerSpId=HTTPBIN-SAML

**SAML -- PingOne for Enterprise**  
This is where most SP Connections are managed  

**WSFed -- AzureAD \ O365**  
https://login.microsoftonline.com?whr=cpricedomain.net
