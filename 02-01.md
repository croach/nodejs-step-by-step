# Node.js: Step by Step - NVM

## Introduction

Hi guys. Well it's been a little while since our last session, and I do apologize for such a long hiatus, but I'm back now, and I'm looking forward to continuing this series on node development.

The next two sessions will be a little different from the first two. We've got a bit of a taste for node already in the previous sessions, and now I want to go off on a bit of a tangent and dive into some of the infrastructure that's grown up around node to help make developing in it so much easier. Over the course of the next two sessions we'll be taking a look at two tools that will help you manage your node development environment and make you more productive.

First on our list is the Node Version Manager, or nvm as it's usually referred to, and that's the tool that we'll be covering in today's session. As its name would suggest, it's a tool for managing multiple versions of Node.

Node is still in it's pre-1.0 phase and, as such, can be a bit volatile. As a developer it pays to have a way of locking a project into a specific version of Node once you've started developing with it, and nvm is going to be our tool of choice for doing just that.

So, apologies made, and summary out of the way, let's get right down to business.

## Node Version Manager (nvm)

In the interim since our last session, node has advanced from release 0.4.5 to 0.6.15. Considering the speed at which node is advancing, it's vital that we have a tool that allows us to create sandboxed development environments that are protected from all the volatility in the node world, and nvm is that tool.  Couple it with npm, which we'll be discussing in our next session, and you can create virtual environments around both specific versions of node and any third party libraries that you'll be using in your projects. If you're familiar at all with virtualenv for Python, you'll have some idea of what the combination of these two pieces of software has to offer.

### Installation

The installation of nvm is very simple, just go to its github page in your browser (you can find it by simply googling for nvm and, more than likely, it will be one of the first few results---if not the first). The installation is basically just a matter of storing the nvm directory somewhere in your file system and then sourcing the main shell script whenever you start a new shell. So, if you have git installed on your computer, you can just clone the repository to your local machine and rename it to .nvm in your home directory.

    git clone git://github.com/creationix/nvm.git ~/nvm

All this line is going to do is clone the nvm git repository to your home directory under the name nvm. Now I typically like to have these types of folders hidden, so I'm just going to go ahead and change the name of the nvm to .nvm to make make it hidden. This is a personal preference of mine, so feel free to skip this step, but just remember when updating your startup script, which we will be doing as our next step, you'll need to use the original name of the directory (i.e. nvm) rather than the name that I've just given it.

So, now that we've got the source for nvm installed on our system, the next step will be to add a little bit of code to our shell's config file, such as your .bashrc file, to make the environment aware of nvm. Let's go ahead and open that up now. So, here I'm going to pull up my .bashrc file---you'll need to place the following somewhere within this file:

    export NVM_HOME="$HOME/.nvm"
    if [[ -f "$NVM_HOME/nvm.sh" ]]; then
        source "$NVM_HOME/nvm.sh"
    fi

All that the little bit of code above does is first check for the existence of the `nvm.sh` file within the `.nvm` directory that you just created. Then, if it exists, it runs the file within the current shell creating several new shell functions for you to use. Now, you don't actually need to surround the `source` command with the check for the file's existence since we know it's there, but I like to have it there to keep the shell from screaming at me whenever I remove or move my .nvm directory for whatever reason.

Ok, now that we've got nvm installed, we just need to source our startup file to get nvm to be recognized in the current shell. To make sure everything worked properly, call `nvm` now and hopefully what you'll see is the help output for the command.

If you just saw some help information fly across your screen, you're all setup and ready to manage multiple versions of node.

### Exploring nvm

Now that you have nvm installed, let's play around with it a bit and see what all it has to offer and then get the latest version of node installed on our system.

There are a few important commands that you'll probably use quite often. The first of these is the `ls` command. Go ahead and type `nvm ls` at the command line and take a look at what gets printed out. Not much huh? Just an N/A (or a dot depending on your shell) and a second line that says `current:`. The reason your not seeing much here is that you haven't installed node on your system yet. Once you've got at least one node installed, the `ls` command becomes much more useful by listing all of the currently installed versions of node available for you to choose from as well as the current version in use and any aliases that you've created. Well I can't think of a better segue than that to start installing a copy of node. So, let's do that now.

To install a version of node you invoke the `nvm install` command with the version number of the Node you want to install. For us that will be version 0.6.15, but an important thing to remember here is that the version number must be prefaced with a 'v' or else the install will not work. So, type the command `nvm install v0.6.15` at your command line now and you should see the download and installation begin. The installation is going to take a little while, so I'm just going to skip ahead to the end, so you may want to pause this screencast now until your installation is complete.

Now that we have a version of node installed on our system, let's take another look at the output of `nvm ls`. Notice that this time it's listing our newly installed version of Node. It also lists this version as the current version that we are using. You can confirm this by typing `node -v` and making sure that a version number, rather than an error message, pops up.

What would happen though, if we opened another terminal session? Would our system still default to the most recently installed version of node or something else?  Let's try it out and see what happens. If we open a new terminal window and type `node -v` again, this time we're greeted with an error message telling us that the 'node' command doesn't exist. If we type `nvm ls` we see why this is, we haven't selected a node to use as our current node. To do so we use the `use` command. So, let's go ahead and select version 0.6.15 as our current node. Type `nvm use 0.6.15`. With the `use` command the 'v' prefix is optional, though I'll show a reason why you might want to use it in just a second. Now if we type `nvm ls` again, we should see that our current node version is 0.6.15 and typing `node -v` this time should confirm it.

So, `nvm use` is the command we use to switch between all the different versions of node we have installed on our system. But how do we select one version to always be our default current version? Well you do that through the alias command.

nvm's alias command allows us to setup several different names for versions we use often, as a way of more easily identifying them. If we run `nvm help` we see here that we can create a new alias by typing `nvm alias` followed by the alias name and by the version number we want to associate with that name. So, for example, if we executed the command `nvm alias latest 0.6.15` then from that moment on, we could refer to version 0.6.15 as simply 'latest'. So the next time we wanted to use that version, we could just type `nvm use latest` and voila. Much easier than typing in the version number, and it allows us to label specific versions according to which projects we were using them for.

One alias in particular though that is treated specially by nvm is the 'default' alias. nvm will automatically load the version of node aliased to 'default' as the current version whenever a new shell session is created. So let's go ahead and set our default version now by typing `nvm alias default v0.6.15`. Now if we start a new shell session and type `node -v` we see that the latest version has already been selected for us and typing `nvm ls` will confirm that. Notice also that underneath the Current version of node is a list of all of our aliases that we've defined.  Since we've aliased the latest version of node to default, we probably don't actually need another alias for it, so let's go ahead and get rid of the 'latest' alias we created earlier by typing `nvm unalias latest` and then type `nvm ls` just to confirm that it's been removed.

There's one more thing I want cover before we finish up today, and that's an option that was recently added to nvm that supports tab completion in Bash. Adding this feature is extremely simple, it's just a matter of sourcing another file upon starting a new shell session like we did before for the installation of nvm.

So, let's open up our .bashrc file again and go back to where we put the few lines of code for sourcing nvm. Right below that we're going to place another few lines that will essentially do the same thing as before. As you can see here, we will first make sure that the bash_completion file exists, and then, if it does, we'll source that file.

Let's give it a try. Go ahead and save your changes and exit back out to the command line. Then source the .bashrc file to make sure that it's added the tab completion to our current session.  Then type `nvm h` and hit the tab button and it should complete the word help. Let's try that again with a version number. Type `nvm use v` then tab and it should complete the version 0.6.15 (assuming that you haven't installed anymore versions).  And, that's tab completion. Not an absolute must, but still a feature that makes using nvm much nicer all around.

### Conclusion

Well, that pretty much wraps everything up for today's episode. That should be just enough to get you started with nvm. The app is fairly simple to understand and you should be able to pick up the rest by just looking at the help output from `nvm help`.

As I mentioned quickly at the begining of this session, we'll be covering npm in our next episode. npm is the package manager for Node and compliments nvm quite nicely. The combination of the two make it extremely easy to create completely sandboxed development environments that you can switch between with just a few commands. We'll also install our very first Node package and put it to use helping us out with our development. So, please come back and join me again for part 2 of this look at some of the tools that you'll find indespensible in your Node development.

I'll see you then.


[nvm]:https://github.com/creationix/nvm
