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


## Generate Certificate
```
### Create Self-Signed Certificate (only works Windows 8.1, 10, Server 2016+ ###
$Name           = "NIM"
$ExportPath     = "C:\Data"           # Path to export cert
$Password       = "MySecretPassword"  # Passphrase on cert
$CertTtlMonths  = 24                  # Lifespan of the cert in months
 
$SecurePwd      = ConvertTo-SecureString -String $Password -Force -AsPlainText
$Certificate    = New-SelfSignedCertificate -DnsName $Name -CertStoreLocation "cert:\LocalMachine\My" -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" -HashAlgorithm "SHA256" -NotAfter (Get-Date).AddYears($CertTtlMonths)
Get-ChildItem -Path ("cert:\localMachine\My\" + $Certificate[0].Thumbprint ) | Export-PfxCertificate -FilePath "$ExportPath\AdobeCloud.pfx" -Password $SecurePwd
  
  
### Export Public Key (Required Server 2016+) ###
Export-Certificate -FilePath "$ExportPath\AdobeCloud.cer" -Cert $Certificate -Type CERT -NoClobber
CertUtil -Encode "$ExportPath\AdobeCloud.cer" "$ExportPath\AdobeCloudB64.crt"
```

##Export Public/Private Key using OpenSSL (Alternative)
```
## Export Private Key ##
"C:\Program Files (x86)\GnuWin32\bin\openssl.exe" pkcs12 -in <PFX PATH> -nocerts -out <PRIVATE KEY PATH>
 
## Export Cert ##
"C:\Program Files (x86)\GnuWin32\bin\openssl.exe" pkcs12 -in <PFX PATH> -clcerts -nokeys -out <CERTIFICATE PATH>
 
## Remove PassPhrase from Cert ##
"C:\Program Files (x86)\GnuWin32\bin\openssl.exe" rsa -in <PRIVATE KEY PATH> -out <RSA PRIVATE KEY PATH>
```
