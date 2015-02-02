## Uninstalling Chocolatey

Should you decide you don't like Chocolatey, you can uninstall it simply by removing the folder (and the environment variable(s) that it creates).  Since it is not actually installed on your system, you don't have to worry that it cluttered up your registry (the applications that you installed with Chocolatey or manually, now that's a different story).

### Folder
* C:\Chocolatey - for Chocolatey version < ```0.9.8.27```
* C:\ProgramData\chocolatey > ```0.9.8.27```

**NOTE:** If you upgraded from ```0.9.8.26``` to ```0.9.8.27``` it is likely that ```Chocolatey``` is installed at C:\Chocolatey, which was the default prior to ```0.9.8.27```.  If you did a fresh install of Chocolatey at version ```0.9.8.27``` then the installation folder will be ```C:\ProgramData\Chocolatey```

### User Environment Variables
* ChocolateyInstall