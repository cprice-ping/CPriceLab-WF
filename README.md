This profile is the configuration for CPriceLab - Workforce use cases

It is designed to be used in conjunction with my PD-Base Profile (https://github.com/cprice-ping/PD-Base), with PD being used for:  
* OAuth Clients
* OAuth Persistant Grants
* Authentication Sessions

## Deployment
* Copy the `docker-compose.yaml` and `env_vars` files to a folder
* Modify the `env_vars` file to match your environment
* Launch the stack with `docker-compose up -d`

## Configuration

To access the Admin UI for PF go to:  
https://{{PF_HOSTNAME}}:9999/pingfederate

Credentials:  
`Administrator` / `2FederateM0re`

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
**SAML -- Generic App**  
https://${PF_BASE_URL}/idp/startSSO.ping?PartnerSpId=Dummy-SAML

**SAML -- PingOne for Enterprise**  
https://${PF_BASE_URL}/idp/startSSO.ping?PartnerSpId=Dummy-SAML

**WSFed -- AzureAD \ O365**  
https://login.microsoftonline.com?whr=cpricedomain.net
