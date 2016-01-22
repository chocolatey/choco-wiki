# How To Install or Upgrade a Package Without Running Scripts

So you've updated your applications manually or through some other means or you want to make Chocolatey aware of some previously installed applications without actually running the installation scripts.  

**NOTE**: This is considered an advanced scenario - make sure you read and understand everything here before trying it.

## Installing a Package Without Running Install Script

The first thing to understand is that in most instances when you want to bring Chocolatey knowledgeable about an already installed application, if there is a version match out there (wherever your sources point), then this process is so much better. 

We normally recommend just calling the install as usual with no special switches and letting it handle the install - normally this turns into a no state change scenario for the computer (that is, the end state is that your application is still installed and Chocolatey now knows about it).

However if you are sold on skipping the automation scripts and tricking Chocolatey into believing that a script is installed.

## Upgrade a Package Without Running Scripts
