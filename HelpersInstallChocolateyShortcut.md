# Install-ChocolateyShortcut
This adds a shortcut, at the specified location, with the option to specify 
a number of additional properties for the shortcut, such as Working Directory,
Arguments, Icon Location, and Description.

## Usage

```powershell
Install-ChocolateyShortcut -shortcutFilePath "<path>" -targetPath "<path>"
```

## Parameters
```
    ShortcutFilePath
    TargetPath
    WorkingDirectory
    Arguments
    IconLocation
    Description
```

## Examples

```powershell
Install-ChocolateyShortcut -shortcutFilePath "C:\test.lnk" -targetPath "C:\test.exe"
Install-ChocolateyShortcut -shortcutFilePath "C:\notepad.lnk" `
 -targetPath "C:\Windows\System32\notepad.exe" -workDirectory `
 "C:\" -arguments "C:\test.txt" -iconLocation "C:\test.ico" `
 -description "This is the description"
```

## Detailed Parameter Information

```
-ShortcutFilePath
  The full absolute path to where the shortcut should be created.  

-TargetPath
  The full absolute path to the target for new shortcut.  

-WorkingDirectory (Optional)
  The full absolute path of the Working Directory that will be 
  used by the new shortcut.  

-Arguments (Optional)
  Additional arguments that should be passed along to the new shortcut.  

-IconLocation (Optional)
  The full absolute path to an icon file to be used for the new shortcut.  

-Description (Optional)
  A text description to be associated with the new description.  
```