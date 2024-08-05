#rdp 

**Connect to windows via xFreeRDP**
```shell
xfreerdp /u:[user] /p:[password] /v:[server] /dynamic-resolution /cert:ignore +clipboard /drive:share,/rdp
```

**Set belgian french keyboard**
```powershell
Set-WinUserLanguageList -LanguageList "fr-BE" -Force
```

**Add folders/files in Environment Variables**
Execute
```text
sysdm.cpl
```

In `Advanced` tab, go in `Environment Variables...`

`Edit` the `Path` and click `New` and add the path

It is **not** recursive so add every directory containing the executables

