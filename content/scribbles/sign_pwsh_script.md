---
title: Sign Powershell Scripts
draft: true
tags: ['pwsh']
date: 2025-12-01
---


## Create a Self Signed Certificate

referenced from [official microsoft documentation](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_signing?view=powershell-7.5&viewFallbackFrom=powershell-7.3#methods-of-signing-scripts)

>[!info] Note
> User can create a self-signed certificate for which your computer is the authority that creates the certificate. 
> This certificate is free of charge and enables you to write, sign, and run scripts on your computer.
> However, a script signed by a self-signed certificate will not run on other computers.

### Set the Execution policy of the system to remote signed  
>[!info] Note
> The RemoteSigned policy allows you to run signed scripts or unsigned scripts that you create locally. 

```powershell
# run the command as admin
Set-ExecutionPolicy RemoteSigned
```

### PowerShell Scipt to create the certificate

```powershell
$params = @{
    Subject = 'CN=PowerShell Code Signing Cert'
    Type = 'CodeSigning'
    CertStoreLocation = 'Cert:\CurrentUser\My'
    HashAlgorithm = 'sha256'
}
$cert = New-SelfSignedCertificate @params
```


### Create a Signing Script 

Use the following script to sign scripts. Since we have **AllSigned** policy enabled by default, this script also needs to be signed first before use.

```powershell
# Add-Signature.ps1

[CmdletBinding()]
param(
    [Parameter(Mandatory=$true)]
    [string] $File
)

$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object {
    $_.EnhancedKeyUsageList -match "Code Signing" -and
    $_.NotAfter -gt (Get-Date)
} | Select-Object -First 1


Set-AuthenticodeSignature -FilePath $File -Certificate $cert
```

Sign the Add-Signature.ps1 script file using the following commands

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My -CodeSigningCert |
    Select-Object -First 1

Set-AuthenticodeSignature Add-Signature.ps1 $cert
```

The ==Set-AuthenticodeSignature== cmdlet adds the signature to the script file as a comment block at the end of the file. The comment block begins and ends with ==# SIG #==.

### Add signature script

Use the Add-Signature.ps1 to sign the custom scripts

```powershell
.\Add-Signature.ps1 -File "Path\to\script\file.ps1"
```
