---
layout: post
comments: true
title:  "Palm Veer"
date:   2014-06-29 09:43:43
categories: WebOS Preware
---

### Background
This post might contain significant amount of my personal opinion, beware. I
recently had to connect up an HP Palm Veer, that I bought a while ago.
Hardware asside (but make sure you have the Touchstone, Palm wireless charger),
here is what you need to get started.  

### WebOS Nation
[WebOS Nation][wosn] forums are probably the best source of information about
modern Palm development. If you are interested in getting communtiy patches,
you might need an account there.

### App catalog update
HP released an app catalog update on July 23, 2013. If you reading this now,
then you are already late, which means that you would not be able to update the
App Catalog or install any apps. Luckily enough the restriction is only a "time
bomb", so here is what you do:

- Cancel all failed application installations.
- Change the date to July 1, 2013 (and disable "network time").
- Go to the app catalog and search for "udpate". Find and install "App Catalog Update". If that does not work, try restarting the phone with the date still beeing 2013-07-01.
- Set the date back, turn on "network time".

[Source][appcatalog-fix]

### Community Software
Community software for WebOS is installed via QuickInstall. It works for both
applications and OS patches (more on that later). Sources can be found on
[github][quickinstall-src] (to my best knowledge). 

Main piece of software you would want to install is [Preware][preware], a
package manager.  After getting it, you would not need to use QuickInstall,
unless you are developing your own homebrew software or installing patches.

#### Google synch patch
In short, a while back Google changed how their authentication works, and
'official' WebOS has not caught up with it. To fix this, just install [the
patch][gsync] from WebOS Nation using QuickInstall.

[appcatalog-fix]: http://h30434.www3.hp.com/t5/webOS-Hardware-and-Software/App-Catalog-not-working-Missed-the-update-Try-this/td-p/2823109
[preware]: http://www.webosnation.com/how-install-homebrew-apps-your-touchpad-or-webos-smartphone
[quickinstall-src]: https://github.com/JayCanuck/webos-quick-install
[gsync]: http://forums.webosnation.com/webos-patches/327662-patch-google-sync-https-fix-unknown-error-sign.html?f=240#post3416720
[wosn]: http://www.webosnation.com/
