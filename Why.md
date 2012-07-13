##Why Chocolatey?
###What is the purpose of Chocolatey?

How many times do you hear about an awesome tool/application from your friends and want to try it out? How much effort do you go through to set a tool and all of it's requirements up on your machine? Do you realize that all of this manual effort is a pain? It's a pain we are used to. It's something we just do and don't realize we are wasting time doing all of this manual process. And worse when we install applications. Next Next Next Finish.  
  
There is a better way. Once you start to use Chocolatey to silently install applications, tools, and to quickly set up things on your machine, you will start to realize that it is more of a global automation tool. That makes it an enabler, enabling you to do just about anything. And to repeat it anywhere in the world with little more than an internet connection.  
  
Because Chocolatey is built on top of the NuGet infrastructure, that means you can install packages from Chocolatey.org, NuGet.org, MyGet.org, file shares, directories, custom feeds and from private feeds. That means you can set up your own server (even private) and your own internal packages with more company specific packages.  
  
[[Tell me a story|ChocolateyStory]]  
  
### Why not use an existing windows package manager?
Believe me, I wanted there to be something existing on windows that would fit the bill. Package management is not a glorious job. Especially on Windows. It's taking all of the different native installers, archives, and straight executable downloads and making a simple interface for consumers to issue the same install command for all of them. Here are/were my needs:
  
* Good CLI that is simple to use.
* Main repository that takes packages contributions from the community (and is being maintained).
* Ability to use additional/multiple sources.
* Meta packages that can chain dependencies.
* Virtual packages.
* Packages should be easy to create / maintain.
* Packages should be concise and be able to be created without worrying about distribution rights.
* Unattended installs
* Installation of multiple packages with one command.
  
To date, chocolatey does all of this (virtual packages is coming).  
  