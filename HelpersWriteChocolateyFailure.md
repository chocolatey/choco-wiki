#Write-ChocolateyFailure

`Write-ChocolateyFailure $packageName $failureMessage`  
  
##Description
Notes an unsuccessful chocolatey install.    
     
##Parameters
###$packageName
This is an arbitrary name.  
Example: `'7zip'`  
  
###$failureMessage
This is the message logged back to the main chocolatey window..  
Example: `"$($_.Exception.Message)"`  
  
##Examples
`Write-ChocolateyFailure 'StExBar' "$($_.Exception.Message)"`  
  
[[Helper Reference|HelpersReference]]  