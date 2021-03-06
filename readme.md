[![Build Status](https://travis-ci.org/ZekeSnider/Jared.svg?branch=master)](https://travis-ci.org/ZekeSnider/Jared)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Swift 5.0](https://img.shields.io/badge/Swift-5.0-orange.svg?style=flat)](https://developer.apple.com/swift/)

<a name='Jared'/>

# Jared - A Chat Bot for iMessage

<a name='Download'/>

## Download Links  
Please check out the latest [release](https://github.com/ZekeSnider/Jared/releases/latest) page an up to date download.

## What is Jared?  
A powerful and easily extensible iMessage bot. It makes it possible to add chat bot features to any iMessage conversation. It currently includes some basic scheduling commands built in. API integrations, games, custom emotes, and much more can be added by installing plug-ins. 

Any pull requests and new GitHub issues are much appreciated! If you would like to develop a plugin for Jared, see the plugin section below. I'm always available on [Twitter](https://twitter.com/zekesnider) if you have any ideas/suggestions.

## How it works  
Jared reads from the Messages database on a set interval and queries for new messages. It provides a routing framework for actioning on messages, and uses AppleScript to send outgoing messages. It's also multi-threaded so it can take care of multiple requests at once. Jared allows expansion via `.bundle` plugin files. This allows commands to be added without modifying the main Jared code base. 

I've tried using private APIs such as MessagesKit to send/receive messages to no avail so far. If you have any leads on this front I'd love to hear about it.

## Installation  
![Jared Main Window](/Screenshots/MainWindow.png)

Jared must be run a machine running macOS with an active messages account logged in. It has only been tested on 10.14 Mojave. It may work on old versions of macOS but this is not guaranteed as there may have been changes to the message database's schema. If you don't want Jared posting as you, it is recommended that you create a new Apple ID and user account on your mac, and run it in the background under that user. That way it's not using your main Apple ID.

1. Download the Jared app and move it to the applications folder.  
See [download section](#Download) at the top. 

2. Allow Jared "Full Disk Access" in System Preferences.
This is required because of macOS permissions that limit access to the messages database. 

3. Run Jared.app

4. (Optional) Grant access to contacts
You can (optionally) allow Jared access to your contacts so that it can provide and update names of contacts. The contacts are used to set/retrieve names only. Nothing is sent to any server.

5. (Optional) Allow inbound network connections
If you have a firewall enabled on your mac, and you wish to use the REST API, you will need to allow Jared to accept inbound network connections as necessary. 

6. Grant access to automate Messages
If you are running macOS Catalina or later, you may see a system dialog requesting access to automate message when using Jared. This allows Jared to send messages.


Once you have Jared setup you can type `/help` to get a list of commands. `/help,[command name]` will give you specific information. Use `/reload` to reload plugins.

## Commands

Type `/help` in a group chat with Jared to see a full list of commands. Type `/help,"name of command here"` to get detail on that specific command. 

### Built in commands
For reference, here is a list of the commands built in to Jared. Because functionality can be added with plugins, the built in functionality is kept light.

+ Thank You: Thanks Jared
+ `/ping`: Check if the chat bot is available
+ `/version`: Get the version of Jared running
+ `/send`: Send a message repeatedly
+ `/schedule`: Schedule messages
+ `/name`: Change what Jared calls you
+ `/whoami`: Get your name

### Configuration  
A configuration file is located at `~/Library/Application Support/Jared/config.json` which enables disabling of commands if desired. See [config-sample.json](config-sample.json) for an example.

## REST API
Jared contains a web server with a REST API that can be enabled. For more information, [check out the REST API documentation](restapi.md).

## Webhooks
Jared supports webhooks for sending metadata about incoming and outgoing messages. To configure webhooks, add them to the `config.json` mentioned above. For more info on the webhooks api, [check out the webhook documentation](webhooks.md).

## Plugins  
Plugins are loaded dynamically from the `~/Library/Application Support/Jared/Plugins` folder. To install a module, drag it in there and then send `/reload` to Jared. 


### Plugin List  
+ None yet!  
If you developed any plugins, please contact me a link so I can add a link here! I will be working on a few extra modules of my own as well, and will add them here when they are complete.

### Development  
If you would like to develop your own plugins, you need to build a .bundle to be loaded by Jared. You must include the [JaredFramework.framework](/JaredFramework/JaredFramework.framework) in your project and define a public subclass of RoutingModule. The bundle must set this class as the principle class in `Info.plist`. `Info.plist` must also contain a string for `JaredFrameworkVersion`, the current version number is `J2.0.0`.

To include JaredFramework you have 3 options:
1. Manually include it in the project
2. Use [Carthage](https://github.com/Carthage/Carthage)
3. Use [CocoaPods](https://cocoapods.org) (coming soon)

Take a look at the [Sample project](/SampleModule) to see how the project should be configured. The README there contains instructions for how to build a plugin. Also look at the modules in contained in the main project for examples of more complicated routings.  
