# Installation on debian based distros

Install prereqs: libasound2-dev, xastir, git and audacity

Audacity is being used as a quick and cheerful method to see the audio levels coming from the sound card to make sure it isn't spiking or distorted. Run the below command in a terminal window.

```
sudo apt-get install libasound2-dev xastir git audacity
```

We're going to pull down the source for Direwolf and compile it on the computer locally. If you're not sure about what you're doing, copying and pasting the below lines into the terminal will be enough to get everything running.

```
cd ~
mkdir git
cd git
git clone https://www.github.com/wb2osz/direwolf
cd direwolf
make
sudo make install
```

# Direwolf Configuration

Before attaching the USB sound card, run the below command to see what your setup looks like as it is. The below command lists playback devices.

```
aplay -l
```
My output:

```
sj@x230:~/Downloads$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: PCH [HDA Intel PCH], device 0: ALC269VC Analog [ALC269VC Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 3: HDMI 0 [HDMI 0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 7: HDMI 1 [HDMI 1]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 8: HDMI 2 [HDMI 2]
  Subdevices: 1/1
  Subdevice #0: subdevice #0 
```

Then attach the USB sound card and run the same command again. Look out for the change as this is what you'll need to take note of for Direwolf to capture and output audio. 

My output:

```
sj@x230:~/Downloads$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: PCH [HDA Intel PCH], device 0: ALC269VC Analog [ALC269VC Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 3: HDMI 0 [HDMI 0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 7: HDMI 1 [HDMI 1]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 0: PCH [HDA Intel PCH], device 8: HDMI 2 [HDMI 2]
  Subdevices: 1/1
  Subdevice #0: subdevice #0 
card 1: Device [C-Media USB Audio Device], device 0: USB Audio [USB Audio]
```

The device listed as USB Audio Device is what I'm looking for here. Yours might state a brand or model name. The two things we are taking note of are the card number and the device number. In my case, 1 and 0.

You can also run the below to check the recording device, but this usually matches the same as what is shown for the playback device.

```
arecord -l
```

Right now we'll configure Direwolf as read only and can add to the configuration later on to increase functionality. Open up direwolf.conf in your home directory using your favourite text editor.

Change MCALL NOCALL to MYCALL YOUR-CALL-SIGN and add -10 at the end. So for example MM6LOL-10. The "-10" is a SSID used to show you are operating on this device from a static, home station in my case. You can find other SSIDs (http://aprs.org/aprs11/SSIDs.txt)[here].

```
MYCALL MM6LOL-10
```

Remove the # from ADEVICE so that it looks like the below. If the output noted from "aplay -l" was 1 and 0, you can leave the rest as it is. If it differs, change the numbers to match what you noted. Enter the card number first, followed by the device number.

```
ADEVICE plughw:1,0
```

Save and close the file. That should be enough to get everything up and running as read only.

# Checking Audio Levels

Next we'll want to connect the output of your radio to the mic input of your soundcard. Once complete, open up audacity and check that the capture device is set to your USB sound card. The name should be the same as what the output from "aplay -l" showed.

In my case, I switch to a broadcast radio station to get a consistent output from the radio that we can use to confirm that the input isn't peaking or distorting.

If you're seeing something like the below, your audio levels are too high. This could be due to the system settings, but we'll tackle this from the source first. 

![alt text](https://i.imgur.com/YoEMWDl.png "audacity-distored")

On the radio, turn down the volume till you see something like the below. It doesn't need to be too loud for Direwolf to pick up on and we can fine tune later on.

![alt text](https://i.imgur.com/2yPuKUx.png "audacity-all-good")
