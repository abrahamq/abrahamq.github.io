# Clearing the CMOS memory on lenovo u410
So today I tried to turn on the Optimus Nvidia graphics card on my u410. I went into the BIOS and changed the setting, then exited saving changes. It gets a bit fuzzy after that. The laptop turned off and didn't come back up- just a black screen whenever I turned it on or tried to go into the BIOS through the side button. I looked at it wondering if I'd just bricked my laptop.

I unscrewed the bottom (warranty seal was already broken from the hard drive replacement earlier) and then disconnected the laptop's battery and the small watch battery as well for about 20 seconds. Then I connected both and tried to boot again- same symptoms- just a black screen on both normal boot and booting into the BIOS. Next, I shut it down and disconnected just the watch battery, leaving only the laptop battery connected. Now it booted and everything worked fine. I think that it was this last step that got rid of the bad stuff in the CMOS- just disconnect the battery and try to boot with it disconnected.

Afterwards, I once again tried to switch the video card option from just the Intel VGA graphics (which I had enabled to save battery life) into using the NVidia Optimus technology. This time it worked.

I'll be writing another post on getting Optimus working with bumblebee inside of ubuntu 12.04. Last time I tried this I bricked my computer, so wish me luck.
