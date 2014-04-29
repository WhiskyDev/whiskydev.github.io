---
layout: post
title: "Record Google Earth Using Tools That Don't Suck"
date: 2014-04-28 07:00:00
share: y
disqus: y
---

##The Problem

For April's tasting I wanted to have video clips that took us to the location of each distillery.  [Google Earth][1] does a really good job of rendering that.  The only problem is only the pro version can record videos natively, and the first guides I found on Google recommended using awful tools.  After a bit of experimenting I discovered that this doesn't have to be hard.  The key is using tools that **don't suck**.

##Set Up Google Earth
![Google Earth][2]
Search for the places you want to visit and add them to your list.  You can add a search result to your places from the right click menu.  It's possible to record a _tour_ that visits places automatically for you, but it leaves controls visible on the screen so I don't recommend it.

By default Google Earth's view is a little busy so turn off layers you don't want to see in the bottom-left pane.  I used only borders and country names.

Inside the _View_ menu set _Show Navigation_ to _Never_.  Maybe turn on the _Overview Map_ if you'd like to add some context.  The _View Size_ menu contains all the most common video formats so pick the one that matches your desired output.  I think 720p is usually a good choice.  Don't worry about the place and layer controls on the left, they won't be included in this video.

If you have multiple monitors leave Google Earth on your main monitor.  When we record only the main monitor gets recorded.

##Record with VLC
If you're not using [VLC][3] then your video player might be prettier but it definitely doesn't have more features.  VLC is rock solid and will play any audio or video file you throw at it.  It **doesn't suck**.

We're going to use a super-hidden feature in VLC to record the desktop.  From the _Media_ menu click _Open Capture Device_ and set _Capture Mode_ to _Desktop_.  Set the framerate to something sensible like 30.  Now for the real secret click the arrow beside _Play_ and choose _Convert_.

![Hidden VLC convert mode][4]

From this screen configure your output codec.  If you don't have a strong preference the defaults are probably fine.  You may want to turn off audio though.

Click _Browse_ to choose where to save the file.

We're now ready to record.  If you haven't already make sure Google Earth has visited each of your locations at least once this session so the tiled data is saved.  Click _Start_ and start going through the locations for your video.

##Crop and Cut with FFmpeg
[FFmpeg][5] is the workhorse that powers VLC's coding and decoding capabilities.  It also **doesn't suck**.  Technically we've already used it but now we're going to need the [command line tools][6].

Run ```ffmpeg -h``` and revel in the longest help output I have ever seen.

Only a few of the commands are relevant here.  There's the command to crop video:
```
ffmpeg -i YourMovie.mp4 -vf "crop=W:H:X:Y" YourCroppedMovie.mp4
```
and the command to cut into a shorter clip.
```
ffmpeg -i YourMovie.mp4 -ss HH:MM:SS -to HH:MM:SS -async 1 YourCutMovie.mp4
```

VLC is going to help us plug in the right values.  Open the recoded movie and find the crop video filter (_Tools_ ? _Effects and Filters_ ? _View Effects_ tab ? _Crop_ subtab).  Play with these values until only the Google Earth view is in the frame.

![VLC crop filter][7]

The _Top_ and _Left_ values replace _X_ and _Y_ in the command.  _Right_ - _Left_ and _Bottom_ - _Top_ replace _W_ and _H_ respectively.  If you chose a view size in Google Earth those dimensions will be the same width and height.

Play your recorded clip and note the times you want to start and stop the finished video.  When put all together the command will look something like this:
```
ffmpeg -i YourMovie.mp4 -vf "crop=1280:760:629:89" -ss 00:00:05 -to 00:01:03 -async 1 YourFinalMovie.mp4
```

##Execute That And You're Done
Yay!

  [1]: http://www.google.com/earth/
  [2]: /images/2014-04/google-earth.jpg
  [3]: http://www.videolan.org/vlc/index.html
  [4]: /images/2014-04/vlc-capture.png
  [5]: http://www.ffmpeg.org/
  [6]: http://www.ffmpeg.org/download.html
  [7]: /images/2014-04/vlc-crop.png