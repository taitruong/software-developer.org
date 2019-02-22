---
layout: post
category: post
title: Fix broken packages, Ubuntu 16.04, possible cause by git and atom
date: 2017-12-18
excerpt: "If everything fails use with caution: sudo dpkg --remove --force-remove-reinstreq package_name"
tags: [broken package, ubuntu, 16.04, dpkg, git, atom]
comments: true
---

I recently tried an apt-get update and upgrade and got this output:
```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
5 not fully installed or removed.
Need to get 0 B/184 kB of archives.
After this operation, 0 B of additional disk space will be used.
Do you want to continue? [Y/n]
dpkg: error processing package libcurl3-gnutls:amd64 (--configure):
 package is in a very bad inconsistent state; you should
 reinstall it before attempting configuration
dpkg: dependency problems prevent configuration of git:
 git depends on libcurl3-gnutls (>= 7.16.2); however:
  Package libcurl3-gnutls:amd64 is not configured yet.

No apport report written because the error message indicates its a followup error from a previous failure.
No apport report written because the error message indicates its a followup error from a previous failure.
dpkg: error processing package git (--configure):
 dependency problems - leaving unconfigured
dpkg: dependency problems prevent configuration of curl:
 curl depends on libcurl3-gnutls (= 7.47.0-1ubuntu2.5); however:
  Package libcurl3-gnutls:amd64 is not configured yet.

dpkg: error processing package curl (--configure):
 dependency problems - leaving unconfigured
dpkg: dependency problems prevent configuration of virtualbox:
 virtualbox depends on libcurl3-gnutls (>= 7.16.2); however:
  Package libcurl3-gnutls:amd64 is not configured yet.

dpkg: error processing package virtualbox (--configure):
 dependency problems - leaving unconfigured
dpkg: dependency problems prevent configuration of virtualbox-qt:
 virtualbox-qt depends on virtualbox (= 5.0.40-dfsg-0ubuntu1.16.04.2); however:
  Package virtualbox is not configured yet.

dpkg: error processing package virtualbox-qt (--configure):
 dependency problems - leaving unconfigured
No apport report written because MaxReports is reached already
                                                              No apport report written because MaxReports is reached already
                  Errors were encountered while processing:
 libcurl3-gnutls:amd64
 git
 curl
 virtualbox
 virtualbox-qt
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

I then tried to uninstall/purge each broken package and after that tried an apt-get
```
$ sudo apt-get remove --purge virtualbox

Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  dkms libvncserver1 virtualbox-dkms
Use 'sudo apt autoremove' to remove them.
The following packages will be REMOVED:
  virtualbox* virtualbox-qt*
0 upgraded, 0 newly installed, 2 to remove and 0 not upgraded.
5 not fully installed or removed.
Need to get 0 B/184 kB of archives.
After this operation, 90.2 MB disk space will be freed.
Do you want to continue? [Y/n]
(Reading database ... 305275 files and directories currently installed.)
Removing virtualbox-qt (5.0.40-dfsg-0ubuntu1.16.04.2) ...
Purging configuration files for virtualbox-qt (5.0.40-dfsg-0ubuntu1.16.04.2) ...
Removing virtualbox (5.0.40-dfsg-0ubuntu1.16.04.2) ...
Purging configuration files for virtualbox (5.0.40-dfsg-0ubuntu1.16.04.2) ...
Processing triggers for man-db (2.7.5-1) ...
Processing triggers for hicolor-icon-theme (0.15-0ubuntu1) ...
Processing triggers for shared-mime-info (1.5-2ubuntu0.1) ...
Processing triggers for desktop-file-utils (0.22-1ubuntu5.1) ...
Processing triggers for bamfdaemon (0.5.3~bzr0+16.04.20160824-0ubuntu1) ...
Processing triggers for gnome-menus (3.13.3-6ubuntu3.1) ...
Processing triggers for mime-support (3.59ubuntu1) ...
dpkg: error processing package libcurl3-gnutls:amd64 (--configure):
 package is in a very bad inconsistent state; you should
 reinstall it before attempting configuration
dpkg: dependency problems prevent configuration of git:
 git depends on libcurl3-gnutls (>= 7.16.2); however:
  Package libcurl3-gnutls:amd64 is not configured yet.

dpkg: error processing package git (--configure):
 dependency problems - leaving unconfigured
No apport report written because the error message indicates its a followup error from a previous failure.
No apport report written because the error message indicates its a followup error from a previous failure.
dpkg: dependency problems prevent configuration of curl:
 curl depends on libcurl3-gnutls (= 7.47.0-1ubuntu2.5); however:
  Package libcurl3-gnutls:amd64 is not configured yet.

dpkg: error processing package curl (--configure):
 dependency problems - leaving unconfigured
Errors were encountered while processing:
 libcurl3-gnutls:amd64
 git
 curl
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

But it did not help - even with -f option it did not work:
```
$ sudo apt-get remove --purge virtualbox -f

Reading package lists... Done
Building dependency tree       
Reading state information... Done
Package 'virtualbox' is not installed, so not removed
The following packages were automatically installed and are no longer required:
  dkms libvncserver1 virtualbox-dkms
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
3 not fully installed or removed.
Need to get 0 B/184 kB of archives.
After this operation, 0 B of additional disk space will be used.
dpkg: error processing package libcurl3-gnutls:amd64 (--configure):
 package is in a very bad inconsistent state; you should
 reinstall it before attempting configuration
No apport report written because the error message indicates its a followup error from a previous failure.
No apport report written because the error message indicates its a followup error from a previous failure.
dpkg: dependency problems prevent configuration of git:
 git depends on libcurl3-gnutls (>= 7.16.2); however:
  Package libcurl3-gnutls:amd64 is not configured yet.

dpkg: error processing package git (--configure):
 dependency problems - leaving unconfigured
dpkg: dependency problems prevent configuration of curl:
 curl depends on libcurl3-gnutls (= 7.47.0-1ubuntu2.5); however:
  Package libcurl3-gnutls:amd64 is not configured yet.

dpkg: error processing package curl (--configure):
 dependency problems - leaving unconfigured
Errors were encountered while processing:
 libcurl3-gnutls:amd64
 git
 curl
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

I also tried all of kinds like:
```
sudo apt-get clean
sudo apt-get autoremove
sudo apt-get update --fix-missing
sudo apt-get upgrade --fix-missing
sudo apt-get autoclean
sudo dpkg --configure -a
sudo apt-get upgrade && sudo apt-get -f install
```

I also remove some sources from /etc/apt/sources.list.d like:
```
sudo mv google-chrome.list google-chrome.list.save
```

But I could not upgrad it. Then a colleague send me this interesting link about [How to delete broken packages in ubuntu](https://askubuntu.com/questions/525088/how-to-delete-broken-packages-in-ubuntu) using the option '--force-remove-reinstreq'. As the man page explains for this option:
```
   Package flags
       reinst-required
              A package marked reinst-required is broken and requires reinstallation. These packages cannot be removed, unless forced with  option
              --force-remove-reinstreq.
```

So used it we caution!

# Solution
So I did for each broken package this:
```
# syntax: sudo dpkg --remove --force-remove-reinstreq package_name
sudo dpkg --remove --force-remove-reinstreq virtualbox
# use apt-get upgrade and check whether there is a broken package ...
sudo apt-get upgrade
# ... if so then repeat the tasks again
```

At some point I was possible doing an apt upgrade again.

# Possible Solution
It seems that the Git verion Atom is using was different to the Git version I installed before. So it would have resolve the problem by uninstalling Git and Atom. After this I installed only Atom (which includes Git).

Since I did the other approach I cannot reproduce this problem anymore. But maybe somebody gets into the same problem and try this first ;).

Cheers, Mr. T
