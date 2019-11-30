This profile is the configuration for CPriceLab - Workforce use cases.  

On my host, after the initial Profile is pulled for the first time, I mount it as a volume so that it remains updated with any changes made to the configuration (i.e. not stored and pulled from GitHub). This is to show a common deployment method (Containers) using legacy configuration (Pets).

It is designed to be used in conjunction with my PD-Base Profile (https://github.com/cprice-ping/PD-Base), with PD being used for:  
* OAuth Clients
* OAuth Persistant Grants
* Authentication Sessions
* PF Admin Console credentials

## **Configuration**

![CPriceLab Architecture](CPriceLab%20-%20WF.png)

This configuration includes:

### Adapters
* HTML Form
* Identifier-First (Passwordless)
* PingID

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

### PingID - Special Considerations
The PingID adapter uses the secrets from your PingID tenant to create the proper calls to the service. As such, storing those values in a public location, such as GitHub, sound be considered **risky**.

For this Profile, you can place the `base64` encoded text from a `pingid.properties` file that will be placed into the PingID Adapter settings -- use the `env_var` variable of `PID_BASE64`

-----
### **Sessions**  
Sessions and Policies are also being used to demonstrate reduced user interaction for authentication (not necessary for `Passwordless`):  

* HTML Form (within the same Browser) -- Persistant Session for 7 days
* PingID -- **always** triggers, but PingID Policy configured to watch for:
  * New Device (requires PingID App OTP) 
  * Med / High risk IP Address
  * Geo-velocity checking
  * Recent Authentication -- 12 hrs

The User experience of this should be that:  
* Every week, a full Username \ Password is required
* Every day, an interactive response to PingID is required

-----
### **Applications on PingFed**
**SAML -- HTTPBIN**  
https://${PF_BASE_URL}/idp/startSSO.ping?PartnerSpId=HTTPBIN-SAML

**SAML -- PingOne for Enterprise**  
This is where most SP Connections are managed  

**WSFed -- AzureAD \ O365**  
https://login.microsoftonline.com?whr=cpricedomain.net

My AzureAD Tenant is configured to use the *SupportsMFA* flag, and so PingFed is responsible for **any** MFA requests that AzureAD requires