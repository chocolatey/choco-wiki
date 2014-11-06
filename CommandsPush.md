# Chocolatey Push (cpush)
##New as of v0.9.8.13.  
Pushes a package to chocolatey.org (or somewhere else if source is specified).  
`choco push filename.nupkg` or shortcut with 
`cpush filename.nupkg`.  
  
###NOTE: To use this command, you must have your API key saved for chocolatey.org or the source you want to push to.  
In order to save your API key, copy the access key from your chocolatey.org account into the following command:
`NuGet SetApiKey <your key here> -source https://chocolatey.org/`  
  
To get nuget.exe, you can install `nuget.commandline` with Chocolatey. 
  
##Parameters
###PackageName
File name of the .nupkg file to push.
  
###Source (optional)
Source (directory, share or remote URL feed) to which we push the package.  
Defaults to official Chocolatey feed. 
  
##Examples
`choco push somefile.nupkg`  
  
`cpush somefile.nupkg`  
  
`cpush somefile.nuspec -source http://somewhere.com/`  
  
##Common Errors
 `Failed to process request. 'The specified API key does not provide the authority to push packages.'
  The remote server returned an error: (403) Forbidden..`  
This means the package already exists with a different user (API key).  The package could be unlisted. Please contact one of the administrators of chocolatey.org if you see this and you don't see a good reason for it.  
  
[[Command Reference|CommandsReference]]