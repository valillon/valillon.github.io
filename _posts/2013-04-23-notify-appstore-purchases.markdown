---
layout: single
title:  "Notifying daily AppStore reports with Automator"
excerpt: "This is an old post from Blogger, now migrated here, for notifying AppStore purchases with Automator (macOS)."
date:   2013-04-23 11:00:00
classes: wide
tags: 
    - java
    - programming
    - Automator
    - AppStore
    - sales
    - launchd
---

This is an old post migratted from [Blogger](http://thisshouldbethetitle.blogspot.com/2013/04/notifying-daily-appstore-reports-with.html). Some parts may be deprecated.
{: .notice}


In this [post]({{site.baseurl}}/2013/04/18/generate-appstore-purchases) I published a snippet to download AppStore sales reports of our apps. Therein a so called `GenAppStoreSalesReport.class` that requests reports according to their availability in the Apple servers. Since daily reports can be downloaded within 14 days after they have been created (normally the next day to the corresponding date issue), therefore it is more than convenient an automatic task to download our reports regularly. 

Although the mentioned snippet was coded in *Java* to facilitate cross-platform support and framework consistency with the *Autoingestion* file, this time I decided to use some OSX tools to better integrate the task on hand with macOS. Unix/linux users could achieve similar results with `cron`. Sorry Win users, this is definitively not for you : (. 

My first option was to pipe *iCal+Automator+NotificationCenter*, but the last *iCalendar* definitively didn't work at all. I suffered severe headache attempting to program daily alarms to open launch scripts. Every time I tried to configure a daily *iCal* alarm, the future days were immediately miss-configured. But I still wanted to have a nice daily message appearing in the notification center. Sometimes it can be disturbing or a little bit messy but in general I find it a great GUI tool to have around. So I finally end up with `launchd` instead of *iCal* and avoided `cron` since Apple seems to have deprecated its usage. 


## An executable application with Automator 

When opening *Automator* you'll be prompted to choose between several document types. Choose application. Then to create a workflow you just drag and drop items from the Actions and Variables at the left hand side panel. First create a simple shell script to change directory where your `GenAppStoreSalesReport.class` is located and run it (it's preferable `cd` than execute through the whole path to avoid java runtime errors). Then attach the action *Copy to Clipboard* to capture output info. 

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/notifier/shellScript.png" alt="">
    <figcaption>How to run the <i>GenAppStoreSalesReport</i> script in <i>Automator</i></figcaption>
</figure>

## Make the Notification Center notify you 

Once verified that you don't have any bash or java issues, follow these steps to make your notification center inform you with looovely messages about your sales reports. Otherwise jump to the next section to make your *Automator* workflow be daily executed. 

To make your notification center display the output data printed by your `GenAppStoreSalesReport.class` you need to add 3 items in a group: 

1. The first item recovers all data previously copied to the clipboard. This should be the output data normally printed by your class file when executed through the terminal. 
2. Then type one or several keywords to filter those lines you want to be displayed. In my case, the class file prints, among other data, the total AppStore sales with the string 'Total downloaded' followed by the number of product units. 
3. Then save that line to a variable (change your class file to print whatever you need and filter it properly to create as many text-variables as you need). 

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/notifier/contentsVariable.png" alt="">
    <figcaption>How to filter in <i>Automator</i> the purchased app units from reports.</figcaption>
</figure>

To deliver your text-variables to the notification center you can add either a final action `execute AppleScript` like [here](http://hints.macworld.com/article.php?story=20120831112030251) or install an action like [this](http://www.automatedworkflows.com/2012/08/26/display-notification-center-alert-automator-action-1-0-0/). For simplicity I did the last one with two sample text-variables. Save your workflow application wherever you like (I prefer to have it next to my GenAppStoreSalesReports and Autoingestion) and we're done with *Automator*. We just need to make it run daily. 

<figure class="align-center">
    <img src="{{ site.url }}{{ site.baseurl }}/assets/notifier/notificationCenter.png" alt="">
    <figcaption>How to display a purchases in the Notification Center.</figcaption>
</figure>

## Update your reports with *launchd*

As `cron` seems to be deprecated by Apple due to several reasons and *iCal* did NOT work for scheduled tasks, I saw myself forced to make `launchd` do the dirty work. 

The whole process involves to create a XML file (*.plist*) to be run daily (or whatever other schedule you like) and load it appropriately. 

Open terminal.app and type: 

```bash
$ cd ~/Library/LaunchAgents/
```

Then create your .plist file with a memorable name like: 

```bash
$ vi com.myAppName.updateSalesReports.plist
```

Then press the i-key, paste the following code, press `ESC` and save it by typing `:wq`. Note if you want to modify this file with the *vi* editor you must pay attention to the use of `TAB` instead of `SPACEs` (the last won't work). 


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
        <!-- Change this key 'true' to run it when login -->
        <key>RunAtLoad</key>
        <false/>
        <!-- Label with your .plist name-->
        <key>Label</key>
        <string>com.myAppName.updateSalesReports</string>
        <!-- Open your Automator application with the appropriate path-->
        <key>ProgramArguments</key>
        <array>
                <string>/usr/bin/open</string>
                  <string>/Users/me/Documents/Developer/nyAppName/SalesReports/myAutomatorAppStoreWorkflow.app/</string>
        </array>
        <!-- Schedule your AppStore report refreshing-->
        <key>StartCalendarInterval</key>
        <dict>
                <key>Hour</key>
                <integer>14</integer>
                <key>Minute</key>
                <integer>0</integer>
        </dict>
</dict>
</plist>
```

An alternative GUI option if you prefer is to open *Finder.app*, click on Go by holding the option-key. Click on Library, localize the *LaunchAgents* folder and once there, create a similar *.plist* file (with *Xcode* for instance). 

I don't mind to use terminal, it's 'pro' for many obvious reasons, but it's definitively not friendly, so I stay away from it as much as I can. In this case the terminal option could be faster for a first approach, but the GUI option is by far more friendly for modifying the path to your *Automator* application, in this case stupidly called *myAutomatorAppStoreWorkflow.app*, and your scheduled report updates, in this case every day at 14:00. 

Finally load your task by typing these two lines and we're all done! 

```bash
$ launchctl load ~/Library/LaunchAgents/com.myAppName.updateSalesReports.plist 
$ launchctl start com.myAppName.updateSalesReports
```

Your *.plist* will be anyway launched as a daemon every time you login into your session. While there are other possible options to configure your `launchd` daemons, here you don't need to be root for this stuff. Anyway it's recommendable to have a look to the manual and many websites talking about it. The next post I think I'll post how to generate charts with all this stuff. 

Happy coding : ) 

Coda: really idle readers go to play with bubbles at my web [www.tonoamusic.com](www.tonoamusic.com).

Unfortunately the *TOnOa Music* has been discontinued.
{: .notice--info}

