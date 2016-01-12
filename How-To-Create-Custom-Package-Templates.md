Fill this out

If you drop a template into `$env:ChocolateyInstall\templates` folder, you can use `-t name_of_template` to generate a choco template based on that. So for:

![image](https://cloud.githubusercontent.com/assets/63502/9952997/0603b334-5da2-11e5-8b6d-65a5dbf5d8e0.png)

You would call `choco new pkgname -t organization` and choco will use the template folder instead of the built-in template.

To replace the built-in template, you should put a folder in the template with the name of "default". Then choco will use that instead of the built-in template with no need to specify a template name.