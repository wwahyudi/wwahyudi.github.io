---
layout:     post
title:      Why does copying over RDP sometimes not work?
date:       2015-11-05
summary:    Copying over RDP sometimes not work
categories: windows
---

Sometimes when you RDP, you loose come clipboard functionality, even when you have the "Clipboard" option enabled, you may still have problems! Then close and re-open RDP windows  won’t help. The problem caused by rdpclip.exe on the remote server. __In this case, use Task Manager to kill and restart the rdpclip.exe process on local and remote machines.__

The reason explained by this blog:
<blockquote>
The only way I really knew to fix the clipboard transfer was to close my session and restart it. That meant closing the tools I was using like Visual Studio, Management Studio and the other ancillary processes I have running as I work and then restarting all of it just to restore the clipboard. But today I found a good link on the Terminal Services Blog explaining that what is really happening. The clipboard viewer chain is somehow becoming unresponsive on the local or remote system and events on the clipboards are not being relayed between systems. It is not necessarily a lock being put in place but some sort of failed data transmission. It then goes on to explain the 2 steps you can take to restore the clipboard without restarting your session.
Use Task Manager to kill the rdpclip.exe process
Run rdpclip.exe to restart it
</blockquote>

And some explaination from [Microsoft](https://blogs.technet.microsoft.com/enterprisemobility/2006/11/16/why-does-my-shared-clipboard-not-work-part-1/).

Here are the sources:
* [From superuser.com](http://superuser.com/questions/552108/why-does-copying-over-rdp-sometimes-not-work);
* [From brennan.offwhite.net](http://web.archive.org/web/20100213100756/http:/brennan.offwhite.net/blog/2007/01/18/fixing-copypaste-for-remote-desktop-sessions/).

Also, when your rdpclip.exe not running automatically, you may try [this option](https://social.technet.microsoft.com/forums/windowsserver/en-US/d92ad1c3-826f-496b-8145-bb31615c55fe/rdpclipexe-not-running-automatically).

And the last but not the least, if you are a geek, you may [try this](http://www.remkoweijnen.nl/blog/2007/10/25/rdp-clipboard-fix/).