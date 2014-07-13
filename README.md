PaCoGen - Passive Code Generator
=======

The PaCoGen ist meant to do ease the creation of new code-files.
You define shell-variables in your profile and call them later in the template-files. No duplication, no need to update a bunch of templates. Change the variable and that's it - any future created document will have the updated version.

##Why bash?
Because bash is, from the set of languages I am able to use, available on all systems I use. From install on.

#How to use
##Input
PaCoGen works with just one input - the file that should be created. This input is parsed in the form of `$filename.$fileextension`.

##Profiles
Create a file with your profile - e.g. one for work and one for your private projects. These can look like this:
```shell
LICENSE="# GPLv2"
AUTHORNAME="Some Name"
AUTHORMAIL="some.name@gmail.com"
AUTHORSHIP="# Author: $AUTHORNAME\n# Mail: $AUTHORMAIL"
```
As you can see you can define recurring variables and call them after defining - they work just like any other shell variables (as they are just shell variables). If you have a short version of your license (or generally a short license) you can even define this license and use `$(cat LICENSE)` or `$(cat SMALLLICENSE)`.

PaCoGen will use the file symlinked to default.profile, unless `-p` is defined. Valid input for `-p` are the `.profile`-files in the main-folder of pathogen.

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
As you can see, the filename is being parsed from the input to PaCoGen and can be used as a variable for the template-files.

PaCoGen will choose `template.$fileextension`, unless `-t` is defined. In that case `$templatename.$fileextension` is being used. Valid input for `-t` are the files within the templates-folder.

##Define folders for profiles and templates
If you want to use your own folders (e.g. because you are syncing your configuration-files via your own git), you can export the variables `PACOGEN_PROFILES` and `PACOGEN_TEMPLATES` in your shell configuration file:
```shell
export PACOGEN_PROFILES="$HOME/path/to/profiles"
export PACOGEN_TEMPLATES="$HOME/path/to/templates"
```

##Use PaCoGen everywhere
All roads lead to rome:
* expand your PATH variable (`PATH="$PATH:$HOME/path/to/pacogen/folder/"`)
* copy `pcg` to your `~/bin` or `/usr/bin`
* create an alias `alias="~/path/to/patogen/script"`
* create a symlink from your `~/bin` to pcg
* ...
Just be shure to have the shell variables exported, if pacogen doesn't reside in `~/pacogen/`.
