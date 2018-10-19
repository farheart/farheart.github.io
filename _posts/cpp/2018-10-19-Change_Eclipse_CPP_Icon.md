---
layout: post
title: Change Eclipse C++ Icon
category: Cpp
tags: Cpp
keywords: C++  Eclipse Icon
published: true
---

# Change Icon of Eclipse C++

I have both `Eclipse Java` and `Eclipse C++` on my MacOS, and both of them use the same icon, it is very confused to tell which is which on dock when both of them are launched. 

Is there a way to change one of the icon so that we can differentiate them easily? The answer is Yes.

## Change icon of Eclipse.app (C++)

Follow the steps from <https://www.idownloadblog.com/2014/07/16/how-to-change-app-icon-mac/>

> - Step 1: Use Finder to navigate to the Applications folder of your Mac and select the application for which you want to change the icon.
> - Step 2: Right-click the application for which you want to swap the icon, and select ‘Get Info’ (or press ⌘I on your keyboard).
> - Step 3: In the top left corner of the Info panel, you’ll see the app’s icon. From here, drag the new icon file over the original. Drop it when you see the green + bubble on your cursor. Alternatively, you can right-click on the new icon file and choose “Copy”, or select the file and press ⌘C. Return to the Info panel, highlight the original icon in the top left corner, and press ⌘V to paste.
>   - Note: to revert back, just Press Backspace when highlight the top left icon on the Info Panel
> - Step 4: If the application is already in your Dock, the new icon may not show up until you log out. To fix this, open Terminal (Applications > Utilities > Terminal), type “killall Dock” (without quotes), and press Enter.


Now, in `Applications` folder, the icon of Eclipse should already been changed to the new icon.

However, if you launched the Eclipse, you will find that on dock the icon will change from the updated back to the old one ...

How can we change the running icon as well? Please see next --

## Change icon on dock of MacOS after Eclipse C++ launched

### Initial Trial

> Ref: <https://gist.github.com/marlonbernardes/d3d7fd75ee689c2b989b>
> 
> 1. Navigate to Eclipse's installation folder
> 2. Open eclipse.ini and find the '-product' flag. Mine was something like this: -product org.eclipse.epp.package.jee.product
> 3. Open the plugins folder and look for your product folder. It looks similar to your product flag (mine was  org.eclipse.epp.package.jee_4.4.0.20140612-0500)
> 4. All you need to do now is to replace the png files (javaee-ide_x32.png, javaee-ide_x64.png, ...) with the icons that you want.

Initially I follew the step of above link, unfortunately, it doesn't work ...

### Final solution

- Go to `/Application` folder, right click on `Eclipse_Cpp_Photon.app`, select `Show Package Contents`
- Go to `/Applications/Eclipse_Cpp_Photon.app/Contents/Resources`
- Replace the `.icns` to the new icon (You may go here to convert `.png` icon to `.icns`)

![](/assets/posts/2018-10-19-Change_Eclipse_CPP_Icon/2018-10-19-Change_Eclipse_CPP_Icon_20181019131225.png)

Final Results:

![](/assets/posts/2018-10-19-Change_Eclipse_CPP_Icon/2018-10-19-Change_Eclipse_CPP_Icon_20181019142953.png)



