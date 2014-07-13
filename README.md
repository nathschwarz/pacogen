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

Pacogen will use the file symlinked to default.profile, unless `-p` is defined.

##Templates
Templates are extremely easy - they are just like your typical code file, just that you can call the variables defined in your profile. A python template would look something like this:
```shell
LICENSE

AUTHORSHIP

def main():
    

if  __name__ =='__main__':
    main()
```
So here, `LICENSE` and `AUTHORSHIP` will be replaced with the content defined in your profile-file. 
Want to create a class for java?
```java
LICENSE

IMPORTS

/*
 * What is this class good for?
 * @author AUTHORNAME
 * @version 0.1
 */
public class FILENAME {
    /*
     * Describe what's going on in the main-method.
     */
    public static void main(String[] args) {
         
    }
}
```
As you can see, the filename is being parsed from the input to pacogen and can be used as a variable for the template-files.

Pacogen will choose `template.$fileextension`, unless `-t` is defined. In that case `$templatename.$fileextension` is being used. 
