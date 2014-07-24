---
layout: post
comments: true
title:  "Palm Veer Experience Report"
date:   2014-06-29 09:43:43
categories: WebOS Preware
---

This is my esperience report setting up my HP Palm Veer. I am not a WebOS
hacker myself, so some of the information may sound opinionated or naive.

Hardware is mostly out of scope of this post, though I would definitely
recommend getting Touchstone (Palm wireless charger).

[WebOS Nation][wosn] forums are probably the best source of information about
modern Palm development. If you are interested in getting communtiy patches,
you might need an account there.

Here are the trials that I had to go through.

### Intermittent network failure
I am getting this: http://h30434.www3.hp.com/t5/webOS-Hardware-and-Software/Veer-intermittent-problem-unable-to-send-receive-calls/td-p/2112718

There is no known fix and I don't have any idea why that is happenning.

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

TODO now the catalog is blank. It is a sertificate error, need to find a fix.

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

### Facebook
Adding a Facebook account results in 'synching account' spinning forever.

TODO find a fix

### Outdated synergy services
Most of the synergy services are not kept up to date, so you would get the
"syncing accout sign" spinning forever. Those situations tend to break the
phone's connectivity, so disable the account if you see that. I only had
limited luck with Gmail (haven't tried generic emails though).

[appcatalog-fix]: http://h30434.www3.hp.com/t5/webOS-Hardware-and-Software/App-Catalog-not-working-Missed-the-update-Try-this/td-p/2823109
[preware]: http://www.webosnation.com/how-install-homebrew-apps-your-touchpad-or-webos-smartphone
[quickinstall-src]: https://github.com/JayCanuck/webos-quick-install
[gsync]: http://forums.webosnation.com/webos-patches/327662-patch-google-sync-https-fix-unknown-error-sign.html?f=240#post3416720
[wosn]: http://www.webosnation.com/
