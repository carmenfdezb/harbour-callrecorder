harbour-callrecorder
====================

Native call recorder for Jolla's SailfishOS. The latest version is 0.3.

**This is application requires the latest SailfishOS update (update10, SailfishOS 1.1) or later**

##Table of Contents

 - [Changes](#changes)
 - [Requirements](#requirements)
 - [Installation](#installation)
     - [From OpenRepos](#installation-from-openrepos)
     - [From RPM](#installation-from-rpm)
     - [From Sources](#installation-from-sources)
 - [Usage](#usage)
     - [Storage](#storage)
     - [Audio Format](#audio-format)
     - [UI](#call-recorder-ui)
 - [Known Issues](#known-issues)
 - [Troubleshooting](#troubleshooting)
 - [FAQ](#faq)
 - [Contacts](#contacts)

##Changes

###0.4
 - Fixed issues
  - Empty list placeholder appears when list is not empty ([#6](../../issues/6));
  - Workaround for Android microphone [issue](#android-applications-do-not-record-sound-after-recording-a-call).
 - New features
  - Choosing of save location and relocating already recorded files;
  - Choosing sampling rate and FLAC compression level.

###0.3-6
 - "Automatic startup" refuses to activate ([#8](../../issues/8)).
 
If you update from 0.3-5 and the call recording daemon is running, it will be shut down as a side effect of this update. Please start it again from UI using the cover action or with Settings -> Active.

###0.3
 - Ability to remove recordings ([#1](../../issues/1));
 - Settings page with ability to turn on/off the recorder, enable/disable automatic startup ([#2](../../issues/2));
 - Cover actions with quick starting/stopping the recorder;
 - Recording of an already ongoing call ([#2](../../issues/2)).

###0.2
 - Initial release

##Requirements

* SailfishOS 1.1 or later
* Granted permission for installation of untrusted software (Settings -> Untrusted software) 

##Installation

###Installation from OpenRepos

1. Install Warehouse application from [OpenRepos](https://openrepos.net/content/basil/warehouse-sailfishos)
2. Search for 'Call Recorder' in Warehouse
3. Enable repo and install it.

###Installation from RPM

1. Enable untrusted software installation at Settings -> Untrusted software.
2. Download the latest version RPM package which is located at https://dpurgin.github.io/. This page is best accessed from your Jolla device. The package you need *doesn't* say `debuginfo` or `debugsource` in its name.
3. If you have downloaded the package with the Jolla Browser, go to Settings -> Transfers and tap on the downloaded package, it prompts for installation after a while.
4. Tap on `Install` when prompted.
5. After installation is successful, a notification appears.
6. Make sure `Call Recorder` has appeared in the list of applications.
7. Make a call and check if the recording has appeared in `Call Recorder` application. If it hasn't, go to Troubleshooting section.

You don't need to have the UI application running all the time to have your calls recorded. They always are as soon as the service is enabled. 

###Installation from sources

This section assumes you are familiar with SailfishOS SDK and able to deploy your own project to a device. The project was developed and tested with SDK version 1407.

1. Clone the project from master branch or a tag to a directory of your choice. The master branch will always contain compilable bleeding edge code that doesn't necessarily work as you expect it to.
2. Open harbour-callrecorder.pro in SailfishOS SDK, configure the project to use armv7hl and i486 targets.
3. Run qmake, build, deploy. 

Luckily, all the dependencies should be resolved and downloaded automatically by the SDK. If not, just follow the error messages.

##Usage

The harbour-callrecorder is designed for unattended usage. Once properly installed, it records every GSM call you make or receive. The UI application doesn't need to be run to make this happen. Moreover, think of UI application as the way to access recorded calls only.

###Storage

Data generated by the application is stored at writable location under `/home/nemo/.local/share/kz.dpurgin/harbour-callrecorder/` (further referred to as `$DATA`).

All the recordings made by the daemon are initially stored at `$DATA/data`. You may change this using at the Settings page of the UI application.

List of recordings with their properties is stored in a database at `$DATA/callrecorder.db`. Please refer to `libcallrecorder/database.cpp`, `Database::Database()` for database structure.

###Audio Format
The recordings are initially encoded to FLAC, 22 kHz, mono, 16-bit LE, compression level 8. You may change sample rate and compression level at the Settings page of the UI application.

###Call Recorder UI

This application is a simple front-end for accessing the recordings which were made since you got the recorder working. It also has an ability to turn on/off the recording quickly.

####Start page -- Recordings

This is a list of all the recordings made by the call recording daemon. Each list item contains call type marker (incoming/outgoing), caller/callee ID, date and time, recording duration and file size. Tap on item gets to Details page. 

Pull-down menu provides access to About, Settings and Select recordings pages.

####Details Page

This page allows to listen to the recording. Player is situated in the lower part of the page. If an error occurred, player is replaced with textual error description. The recording can be seeked through using the progress bar above the Play button.

####About Page

Pretty self-explanatory. Click on the button at the bottom to view short license notice. Full license text is installed to `/usr/share/harbour-callrecorder/LICENSE`.

####Settings Page

**Recorder Daemon**

Turn the recorder on or off, enable or disable automatic startup on this page. If an item remains lit/unlit after tap, this means the underlying action didn't succeed. You might want to take a look at DBus session bus to see what's wrong.

**Location**

Type in save location or choose with a directory picker by pressing 'Browse' button. If location does not exist or is not writable, the text field is marked with red. After either browsing or typing in a new location, **press 'Save' button** to save it. If the old location contained recordings, they will be moved to the new location after 5 sec remorse. You may cancel this action but the old recordings won't be accessible from the UI. If the action was canceled accidentally, reset location to the old one, tap 'Save' and then set the new location again followed by tapping 'Save'.

####Directory picker page

Directory picker can be used to select a writable location. It opens with current path set to one entered in 'Location' text field. If path doesn't exist, the picker opens your Home directory. The directory you will peak by accepting dialog appears both at the bottom toolbar and at the top right corner of the dialog.

**List of directories**

This is a list of directories at current path. Short tap on an item to navigate to this directory. If directory doesn't contain any nested directories, placeholder appears. Although this doesn't mean that directory doesn't contain files. Long tap on an item to rename writable directory or remove empty writable directory. When renaming is not availble, this means this directory is not writable. When removing is not available, this means this directory is either read-only or not empty (may contain other files or directories).

**Bottom Toolbar**

The dialog contains a toolbar at the bottom for convenient navigation and directory creation. The buttons from left to right are: 'Navigate Up', 'Go to Home', 'Go to SD Card', 'New Directory'. 

'Navigate Up' is used to go to parent folder. You may also use 'Home' and 'SD Card' buttons of the bottom toolbar to quickly navigate to `/home/nemo` or to `/media/sdcard/<SD Serial Number>` respectively. 'New Directory' button can be used to create a new directory in a writable location. 

####Select recordings Page

Select recordings with a tap on a list item. Having items selected, you can make an action. Currently, the only supported action is remove. Choose 'Delete all' from the pulley menu to remove all the recordings, both selected and unselected.

##Known Issues

###Version 0.4

Currently none

###Version 0.3

####Automatic startup refuses to activate

See [#8](../../issues/8)

This issue appears on fresh installs of 0.3-5.

**Solution**

Update to later version.

**Workaround**

Being the root user issue the following command:

```
# chown --recursive nemo:nemo /home/nemo/.config/systemd
```

####Empty list placeholder appears sometimes when having small number of recordings

See [#6](../../issues/6)

**Workaround**

Restart the UI application

###Version 0.2
####Moving call to speaker when the number is still dialled

If the call is started and put on speaker before the other party takes the call, the call remains on speaker during dialing tones but goes back to earpiece as soon as the other party takes the call, although the speaker icon remains highlit. 

**Solution**

Update to later version

**Workaround**

Tap on the speaker icon twice to get it back to speaker *or* move the call to speaker after the other party takes the call.

####Switching from loudspeaker to earpiece may cause 500ms lack in the recording (doesn't affect the call itself)

This is due to asynchronous nature of PulseAudio events and internals of call recording process. It just needs some time to decide if the sound card really should go to other mode.

**Solution**

Update to later version

**Workaround**

None. Should be treated more as a feature. The 500ms switch time may change to lower values in future based on user experience.

####The other party is not recorded if the call is on headphones or bluetooth

Unfortunately this feature is still WIP.

**Solution**

Update to later version

**Workaround**

None. Do not reroute calls except to loudspeaker, if possible.

**There has been a report on TMO that BT switching works fine and the call gets recorded**

##Troubleshooting

### Calls were recorded after installation but after reboot it doesn't work anymore

Look into the settings page of the call recorder UI and see if 'Automatic startup' option is highlighted. If not, tap on it to activate. If it remains unlit or goes lit for a short period of time and then back to unlit, please see next section.

### The calls are not recorded

First, look into the Settings page of the call recorder UI and see if 'Active' option is highlighted. If not, tap on it to activate. If it remains unlit or goes lit for a short period of time and then back to unlit, please read further.

Please check if the service is enabled and running. You will need the Terminal application. Issue the following command in the terminal to make sure you have the latest status of systemd:

```
$ systemctl --user daemon-reload
```

Then issue the following command to see current status of the call recorder daemon (mind 'd' at the end and you don't need to enter $ sign, it's already there):

```
$ systemctl --user status harbour-callrecorderd
```

If the output looks like this (mind `disabled` at the second line):

```
harbour-callrecorderd.service - Call Recorder Daemon
   Loaded: loaded (/usr/lib/systemd/user/harbour-callrecorderd.service; disabled)
   Active: inactive (dead)
```

then try (mind `d` at the end):

```
$ systemctl --user enable harbour-callrecorderd
```

If you get `Failed to issue method call: No such file or directory` then do enabling manually with: 

```
$ mkdir -p /home/nemo/.config/systemd/user/user-session.target.wants
$ ln -s '/usr/lib/systemd/user/harbour-callrecorderd.service' '/home/nemo/.config/systemd/user/user-session.target.wants/harbour-callrecorderd.service'
$ systemctl --user daemon-reload
```

After enabling start the recorder if not yet:

```
$ systemctl --user start harbour-callrecorderd
```

If the service is up and running, it should say something like this:

```
harbour-callrecorderd.service - Call Recorder Daemon
   Loaded: loaded (/usr/lib/systemd/user/harbour-callrecorderd.service; enabled)
   Active: active (running) since Mon 2014-10-27 00:02:12 ALMT; 19s ago
 Main PID: 1136 (harbour-callrec)
   CGroup: /user.slice/user-100000.slice/user@100000.service/harbour-callrecorderd.service
           └─1136 /usr/bin/harbour-callrecorderd
```

### The UI application shows white screen

Check if you have SailfishOS 1.1 (Settings -> Sailfish OS updates).

### Android applications do not record sound after recording a call

This is an issue of underlying PulseAudio-related stuff which is likely to be fixed in the upcoming releases of SailfishOS. To work around use harbour-callrecorder 0.4 or use the following command:

```
$ pacmd set-default-source source.primary
```

##FAQ

###Does it require developer mode?
Generally, no. If you run into trouble, you might need the Terminal application or SSH connection to diagnose. 

#Contacts
If you have any questions not covered by this file, please contact me at <dpurgin@gmail.com>
