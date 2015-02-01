#Install-ChocolateyShortcut
This adds a shortcut, at the specified location, with the option to specify 
a number of additional properties for the shortcut, such as Working Directory,
Arguments, Icon Location, and Description.

## Usage

    Install-ChocolateyShortcut $targetFilePath

Possible parameters to pass:
```
    ShortcutFilePath
    TargetPath
    WorkingDirectory
    Arguments
    IconLocation
    Description
```

## Examples

    Install-ChocolateyShortcut -shortcutFilePath "C:\test.lnk" -targetPath "C:\test.exe"
    Install-ChocolateyShortcut -shortcutFilePath "C:\notepad.lnk" -targetPath "C:\Windows\System32\notepad.exe" -workDirectory "C:\" -arguments "C:\test.txt" -iconLocation "C:\test.ico" -description "This is the description"
