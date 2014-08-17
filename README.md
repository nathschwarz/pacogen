PaCoGen - Passive Code Generator
=======

The PaCoGen ist meant to do ease the creation of new code-files.
You define placeholders in your profile and call them later in your template-files. No duplication, no need to update a bunch of templates. Change the content of the placeholder and that's it - any future created document will have the updated version.

###Requirements
This script should run solely with bash version 4 and upper.

###'Why Bash?'
I chose to write PaCoGen in Bash to extend my knowdledge in its usage and scripting. Maybe it isn't as neat as it would be in another language, but that wasn't the target. However to make it actually usable is the second scope.

##How to use
###Input
PaCoGen works with just one input - the file that should be created. This input is parsed in the form of `$filename.$fileextension`.

###Profiles
Create a file with your profile - e.g. one for work and one for your private projects. These can look like this:
```shell
LICENSE=GPLv2
AUTHORSHIP=##AUTHORNAME## (##AUTHORMAIL##)
AUTHORNAME=Some Name
AUTHORMAIL=some.name@gmail.com
```
You can reuse placeholders, no matter where. You can use one before defining it, it will still be replaced with the content. But take care not to mess up with the names, or PaCoGen will throw an error.

PaCoGen will use `default.profile` (or the file that is symlinked to `default.profile`), unless `-p` is defined. Valid input for `-p` are the `.profile`-files in the main-folder of pathogen.

###Templates
Templates are extremely easy - they are just like your typical code file, just that you can call the variables defind in the profiles using placeholders - which are the name of the variable surrounded with two hash marks. A python template would look something like this:
```python
# ##LICENSE##

# ##AUTHORSHIP##

def main():
    

if  __name__ =='__main__':
    main()
```
If we run `pcg newFile.py`, `##LICENSE##` and `##AUTHORSHIP##` will be replaced with the content defined in your profile-file. Note that you have to comment the placeholders out, if the content should be commented. It would look like this:
```python
# GPLv2

# Some Name (some.name@gmail.com)

def main():
    

if  __name__ =='__main__':
    main()
```

Want to create a class for java?
```java
// ##LICENSE##

/*
 * What is this class good for?
 * @author ##AUTHORSHIP##
 * @version 0.1
 */
public class ##FILENAME## {
    /*
     * Describe what's going on in the main-method.
     */
    public static void main(String[] args) {
         
    }
}
```
If the above is our `template.java` we can run `pcg test.java`, which creates this:
```java
// GPLv2

/*
 * What is this class good for?
 * @author Some Name (some.name@gmail.com)
 * @version 0.1
 */
public class test {
    /*
     * Describe what's going on in the main-method.
     */
    public static void main(String[] args) {

    }
}
```
As you can see, the filename is being parsed from the input to PaCoGen and can be used as a variable for the template-files.

PaCoGen will choose `template.$fileextension`, unless `-t` is defined. In that case `$templatename.$fileextension` is being used. Valid input for `-t` are the files within the templates-folder.

###Functions
As mentioned earlier, we can also define template functions: `$functionname.function.$fileextensions`.
These function templates can also inherit placeholders and get either a generic name or make use of the `##FUNCTIONNAME##` placeholder.

Lets look at an example, we have the template `template.java`:
```java
/*
 * What is this class good for?
 * @author ##AUTHORNAME##
 * @version 0.1
 */
public class ##FILENAME## {
##FUNCTIONS##
}
```
The function files `main.function.java`:
```java
    /*
     * Describe what's going on in the main-method.
     */
    public static void ##FUNCTIONNAME##(String[] args) {
         
    }
```
and `get.function.java`:
```java
    /*
     * Get ##FUNCTIONNAME##.
     */
    public int get##FUNCTIONNAME##(String[] args) {
        return ##FUNCTIONNAME##;
    }
```
See how the placeholder is directly together with the get? This comes into play in a minute. Now we run the command `pcg -f main:main -f get:runner output.java` - and output.java now contains this:
```java
/*
 * What is this class good for?
 * @author Some Name
 * @version 0.1
 */
public class test {

    /*
     * Describe what's going on in the main-method.
     */
    public static void main(String[] args) {

    }
    /*
     * Get runner.
     */
    public int getrunner(String[] args) {
        return runner;
    }
}
```
And you have a complete java-class prepared, ready to start coding!

The parameters for the function-flag is set as `$functiontemplatename:$functionname` - means the the string in front of the colon (`$functiontemplatename`) is the name of the template-file in your templates-folder (e.g. `get.function.java`. The string after the colon (`$functionname`) determines the name of the function in the generated file.

PaCoGen will remove ##FUNCTIONS## if no function were defined to add.

###Define folders for profiles and templates
If you want to use your own folders (e.g. because you are syncing your configuration-files via your own git), you can export the variables `PACOGEN_PROFILES` and `PACOGEN_TEMPLATES` in your shell configuration file:
```shell
export PACOGEN_PROFILES="$HOME/path/to/profiles"
export PACOGEN_TEMPLATES="$HOME/path/to/templates"
```

###Content-Flag
There is a content flag '-c'. If you specified a content-placeholder ("##CONTENT##") in your template the file specified after this will be inserted at that location. I don't really know if this has a good use at all but hey - why not? 

##Use PaCoGen everywhere
All roads lead to rome:
* expand your PATH variable (`PATH="$PATH:/path/to/pacogen/folder/"`)
* copy `pcg` to your `~/bin` (if `~/bin is in your `PATH`) or `/usr/bin`
* create an alias `alias="/path/to/patogen/script"`
* create a symlink from your `~/bin` to pcg (if `~/bin` is in your `PATH`)
* copy `pcg` into you shell configuration file
* copy `pcg` into your `.functionsrc`
* ...

Just be shure to have the shell variables exported, if pacogen doesn't reside in `~/pacogen/`.

##Contact
There are multiple ways to reach me, the best are these:
* eMail: nathan.notwhite@gmail.com
* Freenode-IRC: `/msg nath_schwarz`
* Or file an issue here on github.
