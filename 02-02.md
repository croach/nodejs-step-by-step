# Node.js: Step by Step - NPM

## Introduction

In our last episode we covered the NVM tool for managing the different versions of node that we need installed on our system for development. Today we're going to cover npm, a tool used managing software packages in Node.

So what is npm? Well, while its website states that npm officially doesn't stand for anything, it is in fact the package manager for node, so I'll let you draw your own conclusions on what the name really means. Like gems for Ruby, or pip for Python, it allows us to install and manage any number of third party libraries without all the worries of finding and installing dependencies and so on and so forth.

Finally, as a bonus, we'll put npm to use by installing another third party program from the maker of npm that should help make developing Node apps much less painful.

So, let's get started...

## Node's Package Manager (NPM)

NPM is one tool that I think makes node worth using even if you're not completely sold on all of node's other strengths. I consider it to be the new standard by which all package management systems should be judged. I hope by the end of this screencast you'll agree with me, and that you'll have some idea as to just how powerful and easy to use NPM is, and, in a future screencast, I hope to cover it a bit further by showing off just how easy it is to create and distribute your own packages. But that's enough praise for the program, let's go ahead and get it installed and start playing around with it a bit.

### Installation

Speaking of installing NPM, if you thought the install for nvm was easy, wait until you see the install for npm...

Did you get all that? No? Don't worry, I didn't accidentally edit out the npm install or anything it's just that npm comes already installed with all of the latest versions of node. I believe---and correct in the comments, if I'm wrong---anything after the 0.4 series (specifically, v0.4.3) should already come with npm installed. So, essentially---your done. That said, I did run into a few minor problems accessing the documentation for my install of NPM and I'll cover how I fixed the issue, just in case any of you run into the same problem.

### Documentation

Now npm loves Unix, and it shows in its documentation. All of the docs for npm are man pages and even the website docs are just an HTML version of the man pages. So it stands to reason that you'd probably want to have this very valuable bit of information right at your fingertips. However, if you try executing `man npm` from the command line, you may get back something to the tune of "No manual entry for npm". Now, you could also access the same man pages through the `npm help` command like so (run `npm help npm`), but if you're like me, you like everything to work as you'd expect it to and being able to call the `man` command to see a man page is just something that's gotta work, right?

Well, the fix that I've found for this is really quite easy and it's simply to upgrade your version of npm. Doing so will put a smile on the face of any lisp programmers since you'll actually be using npm to update npm---how very meta. Just type `npm update -g npm` at the command line to fetch and install the latest version of npm for the current version of node that you are using.

Once the update is finished, you should be able to type `man npm` and get the main man page for the npm program. Now you can also find help for each of the commands that npm supports by typing either `man npm-<command_name>` or `npm help <command_name>`. Both should work and both should show you the same information, namely the man page for the given command.

### Configuration

Now that we have the latest version of npm installed on our system, let's play around a bit and see what all it has to offer. npm is extremely configurable and has tons of cool features for you to explore. To start off, let's take a look at changing the default format for the documentation.

We just saw how we can view the documentation for npm via man pages, but what if you don't happen to be a fan of reading docs from the command line? Perhaps you'd like your documentation to be in the form of a web page? If so, there's a configuration setting just for you.

We'll be using npm's `config` command to change the 'viewer' setting, but before we do, let's take a look at what the `config` command has to offer us by typing `npm help config` and checking out the Synopsis section of its documentation. We can see here that we have four different options available to us---namely get, set, list, and delete---and we'll be putting all of them to use in this segment. At the bottom of this documentation, you'll find descriptions of each of the available settings that we can change, so let's find the description for the "viewer" setting before we exit back out ot the command line.

You can search for text in a man page by hitting the forward slash and then typing in the text that you're looking for, so let's type in `/viewer` and press enter. Here we can see that we have the option of setting the "viewer" configuration variable to be either "man", for man pages, or "browser", for HTML. Now that we know what are options are, let's exit back out and update the configuration.

Before we change the configuration though, let's first take a look at the current value of the 'viewer' variable and verify that it is in fact set to "man". Type `npm config get viewer` and press return and you should see that the current value is indeed set to "man".

Now let's change the "viewer" variable to "browser" by typing `npm config set viewer browser` and pressing return. In true Unix fashion, if everything went well, we won't get any output, but just to make sure everything worked properly, go ahead and type `npm get viewer` and hit return to verify that "viewer" was properly changed (by the way, according to the man page, you can drop the config when doing a get or a set, which is why I was just able to get away with typing `npm get viewer`).

If "browser" was returned, you've successfully updated the "viewer" configuration setting and you're ready to start viewing the npm documentation in glorious technicolor, er, um, HTML. Go ahead and give it a try now by typing `npm help npm` into the command line. Hopefully, your default browser opened up with the documentation for npm in it. If not, you may need to install Chrome for this feature to work.

To revert the configuration, we can simply delete it. We can see why this is true by taking a look at our current npm configuration settings with the `list` command (or you can use `ls` for short). There are two things to notice from the output of the `list` command.

First, you can see that all the `config` command does is manage a user-specific configuration file called `.npmrc` found in your home directory. So you could simply bypass the command and set everything there by hand.

The next thing to notice is that the `list` command is only showing us the settings that we've updated. If we want to view all of the settings---including the default settings---we'll need to add the long, or `-l`, option to the command. Go ahead and try that now by typing `npm config ls -l` and you should see a whole slew of settings fly by.

Given that all we're doing by changing the "viewer" setting is essentially overriding the default value, it would stand to reason that we can simply delete the setting from our config file and it should revert back to the default. Let's do that now by typing `npm config delete viewer` and press return. A quick check with `npm get viewer` confirms that the configuration setting has been reverted and running `npm help npm` confirms it further by opening up the documentation in the terminal rather than the browser.

One final note, if you'd prefer to keep the default settings, but you still want to view the docs in the browser from time to time, npm let's you do so by setting a configuration variable via a command line option. Any configuration setting can be set via the `--key value` command line option. Let's try it real quick by opening up our main npm documentation again in the browser by typing `npm help npm --viewer browser`, and it should open up a browser window again containing the npm docs.

The main reason that I'm running through all the different ways to view the docs is that npm is extremely featureful and very powerful and it's worth it to get lost in the docs for a few hours just to get an idea of what all it has to offer. So after we finish up with this tutorial, you should do yourself a favor and pop open the main help page and just read through and try out stuff, you won't regret it.

### Tab completion

But before you go off and get lost in the docs, I still want to cover a few more features that you might find useful. So, let's go ahead and move onto the next one---tab completion.

If you type `npm completion` into the command line, you'll get back a print out of the shell function that will set everything up for us, but just how do we use it? Well, of course, we're going to go to the docs to find out, right? So, go ahead and type `npm help completion` to get some instructions on how to install it. The second line in the Description section tells us what to do. Specifically, we just need to copy the line in the Synopsis section above into our startup file for whatever shell we run.

Let's go ahead and set that up now by just copying this line and exiting the help, and then opening up our `.bashrc` file and pasting this right underneath the lines we added earlier for nvm. I'm going to go ahead and add another `if` clause around this command as well just to make sure that I have npm installed before it tries to run it. Let me add that now:

    if npm -v >/dev/null 2>&1; then
        . <(npm completion)
    fi

Now if you save that and open a new shell, hopefully everything executes properly and you don't see any error statements. If that's true, then try out the tab completion by typing `npm comp-TAB` and the rest should be filled in for you. Now this worked just fine for me in zsh, but for some reason, I had troubles with this setup in bash. So, if you're running bash, and, when you open a new shell you see several errors from npm starting with something like `Error: write EPIPE`, a workaround I've found is to simply replace the Synopsis line in the `.bashrc` file with the full text from the `npm completion` command. So, I'll go ahead and pop back out to my command line and run `npm completion` and copy that and go back to my `.bashrc` file and paste it within the `if` statement I added earlier. Now, if you open another shell, everything should be working correctly.

### Search and info

Ok, so we've read over the docs a bit, set up tab completion, and we're ready to start installing some packages, right? Great, but, what do we install? How do you even know what packages are available to you? Let's say, for example, that you're writing a command line script in node and you want to add a simple UI that uses the ncurses library. How would you go about finding out if such a library is even available for node? Well one answer is to use the builtin search functionality that comes with npm. Let's go ahead and give that a try now.

Let's continue with our scenario of finding an ncurses library and type `npm search ncurses` into the command line.

Now this could take a little while the very first time you do a search, because npm is creating an index of the available packages, but after that first time, it should go much faster.

It looks like there is an ncurses package available for node, but before we download and install it, let's just dig a bit deeper and make sure it's what we want (though in this particular situation it looks like beggars can't be choosers, since there's only one package available). For sake of argument though, let's say that we're planning on selling this application and we want to make sure that the license is compatible for commercial use. That's where npm's view (or info) command comes in handy.

The view command allows us to investigate a package's registry info. Go ahead and take a look at the docs for it by typing `npm help view`.

There's not much to this command, just give it a package name---and an optional version---and then specify the field that you want to view. Notice that the field can have subfields. I'll show you how that works in just a second, but, for now, let's exit back out of the help and try viewing the license information for the package. Type `npm view ncurses licenses` and return.

Looks like the ncurses package is developed under the MIT license, that's pretty liberal so selling our app shouldn't be an issue.

What about dependencies though? I don't really want to install anything with a ton of dependencies, so let's check the depencies for the package. Type `npm view ncurses dependencies`

And, hey look at that, no dependencies. Nice!

Finally, try typing `npm view ncurses` and hit return.

Notice that this time we didn't give the view command a field name and the result was a JSON object with all of the info on the ncurses library. The view command essentially returns either the full JSON object or the specified field that we want. The optional subfield now makes more sense doesn't it?

Let's say that I just can't stand using SVN, so I want to make sure that its repository is in something modern like Git or Mercurial. To do so, type `npm view ncurses `---by the way, while I'm thinking about it, you can use tab completion here as well, I just wouldn't advise using it on the package name, since that can take a little while---and now let's finish this using tab completion, type `repo-TAB`.`t-TAB` and hit return.

Looks like this one uses git, so I think we're good to go.

Well, I think that wraps up search and info, and I really can't think of any other goodies that I want to cover, so let's go ahead and start installing some packages with npm.

### Installing packages

Alright we've covered quite a bit of ground with npm today, but so far we haven't installed a single library. We'll get to that in a moment, but first, we need to discuss installing locally vs globally.

#### Local vs Global Installs

Whenever you install a package using npm you have a choice as to how you want to install it. The default is to install the package locally, but just what does that mean? Well, to understand this, you'll first need to understand how node's `require` function works.

Whenever `require` is called from within a node file, if the argument passed isn't a native module or a filepath, node will attempt to look up the module starting in the current directory, or the directory in which the calling file resides, and checking for the module in a directory called `node_modules`. If it doesn't find the module there, it goes on to the parent directory and does the same thing, and so on until the root directory is reached.

So, for example, as you can see, I'm in my home directory, and if I had a file that called `require` and I passed in the name `foo`, node would first look for `/Users/croach/node_modules/foo.js`. Since it doesn't exist, it would then go up a directory and look for `/Users/node_modules/foo.js` and finally it would check for `foo.js` in the node modules folder in the root directory.

What all this means is that whenever npm installs a package locally, it will first check for a node modules directory in the current directory and install it there if it exists, and create the directory if it doesn't. I want to show you how this works now, but let me first just create a sandbox directory for me to play around in. Now, try calling `npm install express`.

> If you're curious, Express is a web development framework based on ideas from Ruby's Sinatra framework and we'll use it later in this tutorial.

Ok, the package was installed, now let's do an `ls` to see that the node_modules directory was created and do an `ls node_modules` to see that the express framework has been installed within it. You can also do an `npm ls` to list all of the packages installed locally.

Notice that express is listed at the top of what looks like some type of tree structure. This is the dependency tree for express, so it's safe to assume that all of these packages were installed along with express. But, when we did an `ls` on the node modules directory we didn't see any of these other packages, so where are they installed?

Well, that's part of the beauty of the way that node's `require` function works. Remember I said that it looks first inside of a node modules folder in the same directory as the file that's requiring the module. Well that would mean that the modules required by express would be found within a node modules folder inside of the express folder, right?

Try doing an `ls` on the express folder. Aha, so there's another node modules folder in there. Now do an `ls` on that folder. Now we're seeing the four direct dependencies of express that we in the dependency tree printed out by `npm ls` earlier.

The beauty of this mechanism is that it allows each module to install whatever dependencies it needs locally preventing any sort of version clashing that typically shows up with the use of shared dependencies.

So local installs look like the way to go for your package installation needs, so then what are global installs for? Well, some packages have executables that come along with them, essentially commands that you can run that perform tasks for you. For those, you might want to install them globally. Express is one such package that comes with a command line program that you can use to create application skeletons much like the rails program for Ruby on Rails does. To install the Express package globally, you simply need to add the `-g` option to the install command. Try it out now, by typing `npm install express -g`.

Now we can call `express -h` to see the help output for the `express` program. If you'd like to see what all you have installed globally, just type `npm ls -g`. Pretty much anything that you can do with npm you can do globally by adding the `-g` option. Now let's go ahead and remove the express package globally by calling `npm uninstall express -g`. If we call `express -h` again, this time we should get back a "command not found" error.

So, installing packages with executables globally works, but what if we want don't want to muddy our global space with a bunch of packages that we only need for specific projects. I do a lot of programming in python and I love the virtualenv library for creating virtual environments, essentially like little gated communities where I can install whatever I want and never have to worry about polluting the main environment.

Well, with just a little code added to our .bashrc file we can create something pretty similar, and I'm going to show you how you can do that now.

#### Virtual Environments

Before we add the bash functions that will create our virtual environments, I want you to understand just what it is that these functions will be doing. So, let's first take another look in the local node modules directory, this time with the `-a` option.

Notice a hidden diectory in there that we didn't see before called `.bin`. If you take a look inside of there, you'll see a symbolic link that points right to the express command in the locally installed package. This `.bin` folder is where npm links all of the executables that each package provides. So, if we can just get that folder added to our PATH environment variable whenever we `cd` into a node application's directory, we would gain access to each package's command line programs without the need to install them globally, and that's exactly what the bash functions that I've created will do for you.

I'm not going to go over the code for the bash functions here, since they're unrelated to our topic---however, if you're trully intersted, just ask in the comments and I'll be more than happy to post some notes on how the code works---but, for now, let's just head on over to the github repository that I've setup for these screencasts and grob a copy of those functions.

If we go to 02.md (and, yes, of course, I number them starting from 00) we can scroll down to very near the bottom of this script and copy the code. Then jump back to the terminal and open up your .bashrc file and paste the code right below the tab completion code for npm. Then, exit back out and source the .bashrc file and that should do it.

Now if you cd out of the current directory and then back into it, the virtual environment should have been created. You can check by calling `echo $PATH` and making sure that the current directory has been added to the PATH variable, but you can also just call `express -h` again to make sure that we can call it. To further make sure, call `which express` to see that it is, in fact, calling the one stored in the local `node_modules/.bin`. The nice thing about the virtual environment code is that when you leave the node application's directory, it will reset your environment. So, if we cd back out of this directory and try to call `express -h` again, you'll notice we get the "command not found" error once again.

#### Installing Supervisor

Now let's cd back into our app directory because we are going to finally install a package that we are going to put to use in the rest of this series. This package is written by the same guy that wrote npm and it's one package that I think every reall node developer needs to have.

The package that we're going to install is called supervisor and what it does is run a node service and restart it whenever a change has been made to it. So, we no longer have to shutdown our server and restart by hand everytime we make a change to our file, instead supervisor will now do that for us.

Let's do a search for supervisor since I don't quite remember if the package is called node-supervisor or supervisor. Type `npm search supervisor`. With this search, we get several packages back, but the first one in the list called simply supervisor looks like the one we want.

So, if you added the virtual environment code from our last section to your .bashrc file, you can install the package locally by `npm install supervisor`. This package does provide a single executable, so you'll either want to use the virtual environment code or you'll want to install it globally.

Now that we've got it installed, let's give it a try. In your app's directory, call `supervisor app.js`. That will start our app's server up. Then let's go to our browser and hit the new post form at http://localhost:8000/posts/new/. Now let's make a quick change to our app.js file to see how supervisor works. What I'm going to do is change the URL for our new post page from /posts/new to /posts/brandnew. Save that change and notice that supervisor has spit out something. It's telling us that the server has crashed and then restarted and is running once again on port 8000. If we now refresh our page in the browser, we see that the URL has indeed changed. Let's try out the new URL, and that looks good. Now let's go back over to our code and change the URL back and save. Supervisor once again stopped and restarted the server. Now we can change the URL in the browser back to new, and voila, there is our new posts page.

## Conclusion

Alright, well that pretty much wraps up everything. I know this has been a long episode, but there was just so much to cover.

Over the course of the past two episodes you've learned about, what I consider to be, the most important tools in the node developer's arsenal. NVM is a great way to manage multiple node versions on one machine, and if you don't know and understand npm, you're going to find that you are continuously reinvinting the wheel when developing in Node. Finally, we installed, and took a look at supervisor, a really handy little program that helps shrink our development time by making it possible to see our changes take effect immediately in the browser.

I hope that you've all gotten something out of this and the last episode and I look forward to getting in back into the development of our app in our next session. So, please, join me again for the next episode of Node: Step by Step.

I'll talk to you all again very soon.




## Virtual Environment Code (.bashrc)

    # Checks that the child directory is a subdirectory of the parent
    is_subdirectory() {
        local child="$1"
        local parent="$2"
        if [[ "${child##${parent}}" != "$child" ]]; then
    	    return 0
        else
    	    return 1
        fi
    }

    # Activates a new environment
    activate_env() {
        # Check if the directory we've cd'ed into is a node environment directory
        # (i.e., it contains a node_modules folder) and that a node envrionment
        # does not already exist before creating a new one.
        if [ -d "node_modules" ] && [ -z "$_ENV_DIR" ]; then

            # Save the old PATH variable so we can revert back to it when we leave
            # the environment
    	    export _OLD_PATH="$PATH"

            # An environment is essentially nothing more than an environment
            # variable (_ENV_DIR) pointing the parent directory of our node
            # environment. Create the variable and point it to $PWD.
    	    export _ENV_DIR="$PWD"

            # Add the bin folder for all local NPM installs to the PATH
            export PATH="$(npm bin):$PATH"

            # If an activation script exists, execute it
            if [ -e ".activate" ]; then
    	        source .activate
            fi
        fi
    }

    # Deactivates the current envrionment
    deactivate_env() {
        # Make sure that an envrionment does exist and that the new
        # directory is not a subdirectory of the envrionment directory
        if [ -n "$_ENV_DIR" ] && ! is_subdirectory "$PWD" "$_ENV_DIR"; then

            # Run the deactivation script if it exists
    	    if [[ -e "$_ENV_DIR/.deactivate" ]]; then
    	        source "$_ENV_DIR/.deactivate"
    	    fi

            # Revert back to the original PATH
    	    export PATH="$_OLD_PATH"

            # Destroy the environment
    	    unset _ENV_DIR
    	    unset _OLD_PATH
        fi
    }

    env_cd() {
        builtin cd "$@" && deactivate_env && activate_env
    }

    alias cd="env_cd"


[npm]:http://npmjs.org/
[npm-g-vs-l]:http://blog.nodejs.org/2011/03/23/npm-1-0-global-vs-local-installation/
