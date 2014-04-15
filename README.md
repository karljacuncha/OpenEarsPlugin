# OpenEars / Cordova plugin

This is a PhoneGap/Cordova plugin for OpenEars:
http://www.politepix.com/openears/
http://cordova.apache.org/ 
http://phonegap.com/

Versions: OpenEars 1.7, for Codova 3.x


## Disclaimer:

I was just experimenting with this for a project in work and only developed it enough for me and that demo project.
This is more of a tutorial/set of instructions on how to get OpenEars working, rather than a finished plugin.
I'm not an iOS developer (in fact, I've quite grown to hate Xcode and Objectionable-C), so this probably could be done better.


## Pre-requisites


Standard Cordova plugin development setup for iOS:
Cordova & Xcode with command line tools
Nodejs, Coffeescript (and Stylus for the CSS in the demo app)

- http://cordova.apache.org/docs/en/3.4.0/guide_hybrid_plugins_index.md.html
- http://nodejs.org/
- http://coffeescript.org/
- http://learnboost.github.io/stylus/




## Inital Setup


	mkdir OpenEarsPlugin
	
	cd OpenEarsPlugin

	git clone Clone this repo


#### Download OpenEars:

http://www.politepix.com/openears/

http://cdn5.politepix.com/wp-content/uploads/OpenEarsDistribution.tar.bz2

And save the Framework folder to `OpenEarsPlugin/OpenEars/src/ios/Framework`

	cd demo

	cordova add platform ios

	cordove add plugin ../OpenEars

	cordova build ios

And this will fail...

The `OpenEars.framework` and `Slt.framework` don't get added to the Xcode project correctly
(at time of writing, April 2014 - this may change with an update to Cordova or Xcode but for now, they need to be added manually.) 

Open the Xcode project file:  `demo/platforms/ios/HelloCrodova.xcocdeproject`

In Project Navigator > Resources you should see `AcousticModelEnglish.bundle` and `AcousticModelSpanish.bundle` and they should be fine.

In Project Navigator > Frameworks you should see `OpenEars.framework` and `Slt.framework` but in red and not found.
Remove these entries, and add them again in the 'Linked Frameworks and Libraries' section of the Project Editor (just click the plus, then "Add Other...")

...and that should be enough.

Build & Run from Xcode onto your device and you should see a pretty basic listen & speak interface (similar to the OpenEars native sample app).


## Using the plugin in your own app:

For the full Javascript interface, see:
[OpenEars.coffee](OpenEars/www/OpenEars.coffee.md) 

For just the demo app usage, see:
[index.coffee](demo/www/js/index.coffee.md) 







### Sources:

http://www.politepix.com/openears/tutorial/
http://cordova.apache.org/docs/en/3.4.0/guide_platforms_ios_plugin.md.html
https://github.com/rtelep/phonegap_openears
https://github.com/zhaozhao123/iosTTSPlugin



