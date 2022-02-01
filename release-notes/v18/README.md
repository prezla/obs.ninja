---
description: Version 18 release notes; released May 28th, 2021
---

# v18

## Features

### Guest related features

* Secondary screen share will now become disabled when the user is in a queue or after they have been transferred between rooms. Instead, the screen-share function/button will replace the primary stream with the screen share, rather than creating a secondary one. This isn't an ideal solution, but based on how transfer rooms work, this seems like a better solution.
* Fixed an issue where the mic test meter didn't work when using `&cleanoutput`.
* Fixed an issue where \&sticky conflicted with secondary screensharing
* Fixed an issue where when using a macbook m1 /w Chrome, while using `&noap`, the camera's output would crop to 360p if requesting 720p.
* iOS devices (iPhones) when used in a group room will have higher frame rates now between other guests. The quality might still not look great.
* Android and iPhones will be able to support more guests in a room now, before they starts to have problems. Mostly some optimization tweaks.
* Added two options to control the frame rate and quality of screen shares within a group room: `&screensharefps=2` or `&ssfps=2` and `&screensharequality=1` or `&ssq=1`
* The `&cleanish` flag will show the director direct overlay messages, while hiding most others things
* The TALLY light has changed from a colored shadow to an actual message that reflects the user's state in OBS Studio or Vingester.&#x20;

### Director-related features

* Added a "share website" button to the director's room. In broadcast mode, it will show the website full-screen, and revert back to the webcam once the website is taken down. You do not need to share your mic/cam to start screen sharing.

![Useful for broadcasting the video to the group using a third-party video sharing CDN, such as the provided meshcast.io](<../../.gitbook/assets/image (9).png>)

* Double clicking on the volume slider for a guest will set the gain to 100.
* Director can now list and change audio/video devices remotely, including speaker output of remote guests. Guests will be prompted to Accept the change-request though, for privacy reasons.
* Fixed an issue where the director's audio wouldn't be audible if already sharing a website with the room.
* Fixed an issue where sharing a website as a director showed up in the scene.
* Added a new button to the director's room that lets the director to dynamically change the Total Room Bitrate with a slider. Higher the quality, the more stress the guests and director will face. It will likely need some tweaking.

![](<../../.gitbook/assets/image (7).png>)

* You can use custom-scene names now, instead of just 0 to 8, and the buttons for these scenes will auto-appear if it detects the scene is available.

![](<../../.gitbook/assets/image (8).png>)

* ~~the room's broadcast mode publish h264 video by default now, instead of whatever the browser decides (normally vp8). This might reduce CPU load? You can set a codec on the guest's end to override this~~ \[Update: This change has been reverted, as it was using up all the NVidia hardware encoder allowances]
* When you refresh the director's room, your toggle-settings at the top are kept and restored, so you can refresh the director's room without losing your settings. Creating a new room (via the main page) will clear those saved settings though.
* Added support for `&quality`, `&framerate`, `&maxframerate`, and width/height for the Director. Default resolution is now 720p30fps.
* Added `&directorchat` (`&dc`), which will cause chat messages to go to ONLY the director.
* Added icons for video mute and raised-hands to the director's room.

![](<../../.gitbook/assets/image (15).png>)

* Added a few more toggles for scenes; portrait mode, chroma-green, 'fit video to area'.&#x20;
* Improved the blind-feature (added a backup-blinding method call) and provided some messaging for the end-user.

![](<../../.gitbook/assets/image (13).png>)

* `&cover` can be used to have a video be zoomed in and cropped, so it fills its window area completely. Useful if you don't want any gaps between videos.
* `&fadein=500` is a new parameter; also available as a director's room toggle. Has videos fade in smoothly; 500 = 500ms fade in time.

![](<../../.gitbook/assets/image (12).png>)

* `&tips` will show a help-screen on the guest joining (via @jcalado). Also a toggle in the director's room is available to enable this.
* Got rid of some flicker when removing a guest from a `scenetype=2`.
* Scene 2 to 8 now make the background green when active.
* Just to avoid possible confusion or any unforeseen edge case, the director will get a warning if one of their user-requests get rejected due to a director-permission issue.
* When Audio Processing is disabled, I now "disable" the mute and volume options. At the moment, these require the audio processing subsystem, so if it's not active, you can't change the gain setting. Depending on much of a problem this is, I can create another method of muting. (gain control strictly needs the web audio node tho, if on the publisher end, and not viewer end)

![](<../../.gitbook/assets/image (14).png>)

* Added `&margin`, which adds 10px around the videos for some spacing. Added as a toggle to the director's room and it can be customized. Defaults to 10px.

![](<../../.gitbook/assets/image (11).png>)

* If the director sets `&trb` within their URL, it now will change the total room bitrate dynamically for connected guests in the room to that value. So, `?director=rrr&trb=1000` will have the video quality double (as the default is 500 normally).
* Added a somewhat experimental flag `&ltb` or `&limittotalbitrate`, which _tries_ to limit the total outbound bitrate to some max total value, via the publisher's side. This could be useful if you are broadcasting video as a director to the room, but only have a fixed amount of upload bandwidth or CPU. \
  \
  This bitrate limit does not include data used by audio, webp, or meshcast in its calculation. It is _loosely_ accurate, but no promises. It is applied to scenes and solo viewers as well - not just guests.\
  \
  Example use: `https://vdo.ninja/?director=asdfdfssdfddd&trb=1000ltb=5000`

### Electron capture app related

* Added screen sharing support within the Electron app; must "elevate" the app via right-click menu to do this though.
* The electron capture app will use a true full-screen mode (instead of full window mode) when setting things to full-screen.
* Added out-of-date version messaging; lets you know you need to update.
* Added user-prompt support (for passwords/transfer rooms).
* CLI-based window x/y positioning (via jacalado) added.
* Added the ability to change titles of windows via menu in electron capture.
* The last link used is stored in the Electron Capture app now, for easier use.&#x20;
* Updated electron capture to support 60-fps screen capture.

![The Electron Capture app homepage got a facelift, via @jcalado.](<../../.gitbook/assets/image (10).png>)

### Miscellaneous changes

* Improved the remote monitoring tool so it now shows re-transmitted data as a percentage, instead of an absolute value that was hard to understand.
* Added mute support to meshcast.io; so if you mute in OBSN, it will also mute in meshcast now.
* Fixed an issue with the stats where RTT was a total, and not an average.
* The stats view clears now when the remote client disconnects, rather than showing frozen stats, as it was confusing.
* Added the ability to send a keyframe to the iFRAME API.
* The Discord server has a new custom-made bot to help with support: [https://github.com/steveseguin/discordbot ](https://github.com/steveseguin/discordbot)
* Detailed the `&fullscreen` command, which is useful if you want to use the Electron capture app as a webcam source, where you would window-capture your webcam into OBS, rather than using a virtual-camera to pull from OBS into VDO.Ninja. Things use less CPU this way, since virtual-cam is not used. Details on it here: [https://docs.obs.ninja/source-settings/fullscreen](https://docs.obs.ninja/source-settings/fullscreen)
* Fixed an issue with the video control-bar showing when it may not have been desirable.
* `&optimize=0` will set the audio to 0-bitrate as well; useful for saving bandwidth/CPU in OBS when a scene is not visible.
* Added the option to remotely change the audio or video source via the IFRAME api.
* Fixed a bug where scenes of type 3 to 8 didn't initialize the current state correctly.
* When a scene loads, it will now sync the video's mute-state with the director as well; before it just synced the display-state.
* Fixed a bug where `&chatbutton=0` didn't hide incoming chat messages
* The documentation has moved to Gitbook.com, away from a simple Wiki. The new documentation site has a search bar, images, and better organization. (Thank you @jcalado for the help)
* Fixed a couple bugs, including one where the image thumbnail for the virtual background would break in some scenarios.
* The speedtest now has an option at the bottom to switch to a mode that tests screen sharing, instead of just webcam.
* Made a free Youtube Chat Overlay chrome extension thing; hosted here: [http://chat.overlay.ninja/](http://chat.overlay.ninja)
* Added numerous articles to the docs, such as this one about overheating and ideas to help prevent it: [https://docs.obs.ninja/common-errors-and-known-issues/overheating](https://docs.obs.ninja/common-errors-and-known-issues/overheating)
* Updated the guides section a bit: [https://guides.obs.ninja/](https://guides.obs.ninja)
*   Added a MIDI transport option. This lets you route all MIDI messages from one computer to another computer. Example usage:

    ```
    https://vdo.ninja/?view=Nwz2C7d&midiin=1
    https://vdo.ninja/?midiout=0&push=Nwz2C7d
    ```

    * `&midiin={midi output device index; defaults to all}` (or `&midipull` / `&mi`) -- allows for receiving of remote midi. Device indeces starts at 1, where an index of 0 implies "all".
    * `&midiout={midi input device index; defaults to all}` (or `&midipush` / `&mo`) -- allows for sending of remote midi. Device indices starts at 1, where an index of 0 implies "all".
    * It's important to not send and receive between two tabs locally if from the same midi device, as that will create a feedback loop; computer won't like it.
    * Check the console log or [https://vdo.ninja/midi](https://vdo.ninja/midi) to see which midi device is what device index.

### Personal handshake server option

* Added an experimental option that lets advanced users use a basic/generic websocket service as a personal handshake server; useful for air-gapped private deployments of the service.&#x20;
* A socket server has been developed and provided that can be used as a personal handshake server for this use case. Documentation included:\
  [https://github.com/steveseguin/websocket\_server](https://github.com/steveseguin/websocket\_server)
* Support for piesocket.com has also been added as a third-party handshake-server service option. If using piesocket, you can just do `&pie=APIKEY` to use that service, without deploying any code or servers yourself. The free tier is quite generous and I have no affiliation with them.