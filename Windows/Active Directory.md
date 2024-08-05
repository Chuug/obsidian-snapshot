

**Change user password**
```powershell
Set-ADAccountPassword [user] -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

**Force GPO update immediately**
```powershell
gpupdate /force
```

