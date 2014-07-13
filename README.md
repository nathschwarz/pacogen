PaCoGen - Passive Code Generator
=======

The pacogen ist meant to do ease the creation of new code-files.
You define shell-variables in your profile and call them later in the template-files. No duplication, no need to update a bunch of templates. Change the variable and that's it - any future created document will have the updated version.

##Why bash?
Because bash is, from the set of languages I am able to use, available on all systems I use. From install on.

#How to use
##Profiles
Create a file with your profile - e.g. one for work and one for your private projects. These can look like this:
```shell
LICENSE="# GPLv2"
AUTHORNAME="Some Name"
AUTHORMAIL="some.name@gmail.com"
AUTHORSHIP="# Author: $AUTHORNAME\n# Mail: $AUTHORMAIL"
```
As you can see you can define recurring variables and call them after defining - they work just like any other shell variables (as they are just shell variables). If you have a short version of your license (or generally a short license) you can even define this license and use `$(cat LICENSE)` or `$(cat SMALLLICENSE)`.

##Templates
Templates are extremely easy - they are just like your typical code file, just that you can call the variables defined in your profile. A python template would look something like this:
```shell
LICENSE

AUTHORSHIP

CLASSDOC

def main():
    

if  __name__ =='__main__':
    main()
```
