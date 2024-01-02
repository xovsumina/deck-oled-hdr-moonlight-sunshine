# deck-oled-hdr-moonlight-sunshine
Quick guide on how I set up HDR streaming from my PC to my shiny new Steam Deck OLD

I've been messing with Sunshine and Moonlight w/HDR support for a couple days now and I think I've gotten it to a point I like. Note that this requires Windows 11:

In this setup I use a virtual display used primarily for casting to the deck

## On your PC (Windows 11)

* Install [Virtual-Display-Driver](https://github.com/itsmikethetech/Virtual-Display-Driver)
  * Note that I had to add 1280, 800, 90 to the option.txt file to get the refresh rate to appear later on, I'd suggest doing the same
  * Once the driver is installed, configure the new display to be 1280x800, refresh rate of 90Hz, and enable HDR
  * Optional: I used the [Windows HDR Calibration tool](https://support.microsoft.com/en-us/windows/calibrate-your-hdr-display-using-the-windows-hdr-calibration-app-f30f4809-3369-43e4-9b02-9eabebd23f19) to help tweak HDR but I don't think it's necessary. I actually did this calibration from the Deck itself
* Install [MultiMonitorTool](https://www.nirsoft.net/utils/multi_monitor_tool.html)
  * This will be used to set the virtual display to be primary. This is important as you'll see in a sec
  * I just extracted the .exe into C:\Program Files\Sunshine\tools
  * Run the tool and take note of the Short Monitor IDs of both the virtual display, and your main display
* Install [Sunshine](https://app.lizardbyte.dev/Sunshine/?lng=en-US#Download)
  * Now by default Sunshine streams the "primary" display unless you configure a specific display. Unfortunately, Windows isn't consistent about naming its displays so one day your virtual display might be \\.\DISPLAY5 and the next day \\.\DISPLAY2 which is why I opted to use this MultiMonitorTool to switch primary displays whenever you start a game. Also Sunshine currently only supports this naming convention for now. The other option would be to use something like QRes to dynamically adjust your primary display's resolution, refresh rate, etc but I liked the option of having a dedicated display while keeping my main monitor's config untouched. This is also nice because a lot of games will start on your primary display by default
  * If you were previously using Nvidia gamestream, either disable that or set Sunshine to use a different port. Out of the box Sunshine uses the same port as gamestream and both applications can't bind to it at the same time
  * When you add a game, click the "Add Commands" box which will let you define a "do" and "undo" command. This is what will swap the primary displays when you start and stop a game
  * In the "Do" use MMT to set your virtual display as primary. For me, I set the command as cmd /C "c:\Program Files\Sunshine\tools\MultiMonitorTool.exe" /SetPrimary LNX0000
Similarly for the Undo, use the same command except use the ID for your main monitor
Because it was a pain to do this do/undo command for every game, what I did was create 2 separate "games" that toggle either the virtual display or my main display as primary using the commands above. I'm still playing around with this since you have to exit this "game" after the display is updated but the idea is I run the "pre-game" application to set the virtual display as primary, play my games, then run a "post-game" application to revert my main display back

## Other things (yeah I know this is a lot)
* Occasionally my PC will go to sleep or lock itself which becomes a pain if I try to remote in from the deck so what I did was disable the lock screen completely using steps from [this article](https://learn.microsoft.com/en-us/answers/questions/1283398/how-do-i-remove-the-lock-screen-entirely-from-wind)
* Set up Wake on LAN on your PC. Everyone's setup will be different so look up how to do this on your own. This is nice to have along with no lock screen since Moonlight has the ability to turn on your PC remotely assuming you're on the same network

## On your Deck (Finally!)
* Go to Desktop mode and install Moonlight from the Discover app. The latest builds should have support for HDR
* Add the game to steam as a non-steam application
* Go back to the Deck's gaming mode, start Moonlight, and click on the cog in the upper right to configure and click on "Enable HDR" somewhere on the lower right of the pane (use the touch screen to scroll and click)
* Hopefully Moonlight will have found your Sunshine instance. Start a game and enjoy! Be sure to confirm that the Deck is using HDR from the quick menu
* L1 + R1 + Start + Capture is your friend and will suspend the game, putting you back in Moonlight
* Holding down the Start button for about 3 seconds or so lets you use the right trackpad as a mount, with A as left click and B and right. This might also come in handy, like when your desktop decides to lock itself anyway after a wake (still haven't figured this one out yet)
