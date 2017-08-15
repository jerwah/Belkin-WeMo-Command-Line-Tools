# Belkin-WeMo-Command-Line-Tools

<img align="center" src="WeMo.jpg"/>

Install with NPM by going to the [NPM package page](https://www.npmjs.com/package/belkin-wemo-command-line-tools#star), or, to get deep down and dirty with the code you can jump to the [GitHub project page](https://github.com/agilemation/Belkin-WeMo-Command-Line-Tools)

...not that you can get very dirty in such beautiful code :-)

This package provides a 'wemo' command that can be used to control your Belkin WeMo power switch (and can be easily extended to cater for other similar devices or new methods not yet implemented).  Written entirely out of lowest common denominator tools (such as curl, sed and awk) that will be on any current flavour of Linux/Unix (inc. Mac OSX) so this is a package that requires no external dependencies to be provided prior to use!

I have been pleasantly surprised by the high initial uptake of this utility, there must be a lot of these devices out there! :-)  Any problems/comments/suggestions, please get in touch via GitHub or by email agilemation@gmail.com

N.B.  Initially inspired by the script found on this [Blog Post](http://moderntoil.com/?p=839) and with thanks to 'sklose' for putting the WSDL on a [GitHub Repository](https://raw.githubusercontent.com/sklose/WeMoWsdl/master/BasicService.wsdl)

## Installation
### Global
For global installation of the package use:
```
    $ npm install -g belkin-wemo-command-line-tools
```
That will install the latest version of the package that has hit the NPM repository, globally to your system, (usually in /usr/local/bin) which as long as that is in your PATH variable, you can then simply run the 'wemo' command whenever you want without having to worry about where it is.

### Local
For local installation, if you run `npm install belkin-wemo-command-line-tools` without the `-g` flag, it will install it into your project locally under `./node_modules/belkin-wemo-command-line-tools` - doing this will require you to prefix the wemo command with a path that can locate it.

# Basic Usage
## Getting started with the CLI
If you have globally installed the 'wemo' command, you can now run it in your terminal without any arguments, which will show you the usage page:
```
    $ wemo

    Belkin WeMo Command Line Tool v1.0.27

    Script to control the Belkin WeMo power switch written entirely in shell
    and constructed out of commands that every computer should already have.

    Copyright © 2015 James Borkowski, @github: agilemation
    Third-party trademarks mentioned are the property of their respective owners.

    Usage: wemo -h [ IP/HOSTNAME ] -a [ ACTION ]                         \
                [[ -ver ] | [ -? ]] | [ -s ] | [[ -v ] | [ -V ] | [ -t ] | 
                 [ -T ]] [[ -tt ]] 

    Arguments:
    [ -h | --host ]    IP_ADDRESS | HOSTNAME
    [ -a | --action ]  ON | OFF | GETSTATE | GETSIGNALSTRENGTH | GETNAME |
                       SETNAME NAME

    Optional Arguments: 
    [ -pm ] Minimum Port Number ( Default: 49152 )
    [ -px ] Maximum Port Number ( Default: 49154 )
    [ -s | --silent ]        
    [ -v | --verbose ]        
    [ -V | --very-verbose ]   } 
    [ -t | --trace ]          } [ -tt | --trace-time ]  
    [ -T | --trace-raw ]      }
    [ -ver | --version ]
    [ -? | --help ]
```

## Getting the state
- To get the state you can use a command such as
```bash
$ wemo --host powerswitch1.lnd --action GETSTATE
```
With no other flags, as long as the switch can be contacted, the script should return either `ON` or `OFF` depending on its current state.

## Turning a switch ON or OFF
- To turn a switch ON or OFF you can use a command such as...
```bash
$ wemo --host powerswitch1.lnd --action ON
```
(or)
```bash
$ wemo --host powerswitch1.lnd --action OFF
```
## Getting the signal strength
- To get the signal strength of the switches WiFi connection you can use a command such as
```bash
$ wemo --host powerswitch1.lnd --action GETSIGNALSTRENGTH
```
With no other flags, as long as the switch can be contacted, the script should return a number that relates to the current signal strength.

## Getting the switch name
- To get the name that the switch is configured to think it is you can use a command such as
```bash
$ wemo --host powerswitch1.lnd --action GETNAME
```
This command should upon successful execution return the name that the switch believes it is called. 

## Setting the switch name
- To set the name that the switch is configured to think it is you can use a command such as
```bash
$ wemo --host powerswitch1.lnd --action SETNAME [NAME_TO_USE]
```
Personally (for sanitys sake) I always name any switches I setup with the same name as their DNS hostname.  I point that hostname at a fixed IP address, one that is issued by a DHCP server (and ensured static as it is reserved for the switches Ethernet MAC address).  However, you can call yours anything you like, this command should upon successful execution return the name that the switch believes it is now called.  This brings us nicely to...

## Networking
The Belkin WeMo switches that I have come across so far have all served their REST control interface on TCP port `49153`, however it is rumoured that there are some out there in the wild that use the TCP port `49152`.  If you have to access the switch from behind a firewall or need to connect to it from the internet (if your home network is using NAT on your internet router) then you will need to make sure that you either allow both these ports to traverse (with a port mapping/firewall rule/both - depending on your setup), or identify which is used on your device with the `telnet` command.  I will be adding the ability to do all this to the tool soon but until then you can test which is open as follows:
```bash
$ telnet powerswitch1.lnd 49153
```
If it is not the port that it runs on you will get something such as:
```
telnet: connect to address 127.0.0.123: Connection refused
telnet: Unable to connect to remote host
```
...whereas if it is the port that your switch is listening on, you will get something like:
```
Trying 111.111.111.111...
Connected to powerswitch1.lnd.
Escape character is '^]'.
```

## Uses
<img align="right" src="https://badoocdn.com/v2/-/-/i/badoo-logo.2.png"/>
This script was written for those that want to control the Belkin WeMo series of devices from command line scripts, so not using your phone or any other interface to a human pressing a button.  Specifically I wrote it to allow the easy reboot of a USB hub that connected many mobile devices to a computer that performed automated regression testing of changes to software being developed at Badoo (8th largest social network).  This was needed to ensure that the phones connection was started afresh prior to a test run, both establishing a baseline and increasing stability of the tests about to be executed.  Similarly, if you wish to control any WeMo devices from a script this is a tool that will help you do that without much else to worry about.  If your device is not catered for at the moment, get in touch and I will work with you to add in the methods it uses.  Alternatively, just look at the source in the ‘wemo’ executable and submit a pull request.  :-)

Enjoy!

