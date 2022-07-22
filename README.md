# NIM-System-REST-Adobe-Cloud
<p align="center">
  <img src="assets/logo.png">
</p>

## Current Available Data
- Users
- Groups
- Group Members

## Setup API Access
- Login to https://console.adobe.io/
- View Integrations
- Create Integration
- Access an API
- Adobe Service > User Management API
- You will then be asked for Name, Description and Public Key. 
  - A self-signed certificate will need to be generated on IAM Server, so the certificate and private key are available. 
  - Upload the public key file (must be a .CRT or .PEM)
  - See below snippets on how to generate a certificate/public key. If you have Server 2016+ you can use PowerShell to generate the key, otherwise use the OpenSSL alternative.
- Gather Client Credentials for access
