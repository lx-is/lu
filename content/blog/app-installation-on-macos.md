---
title: "App uninstallation on macOS"
date: "2023-04-16"
description: "Apple's documentation suggests that deleting the .app file will suffice, but there's more to it."
tags:
- "macOS"
---

There's no good native solution to uninstalling apps on macOS. Apple's [documentation](https://support.apple.com/en-us/HT202235) suggests that deleting the .app file will suffice; while this works, almost every app leaves behind a plethora of files in various directories.

{{< rawhtml >}}
<small><em>this is such a low-hanging fruit blog post xd</em></small>
{{< /rawhtml >}}

### App Cleaner

The most popular tool for uninstalling apps is [AppCleaner](https://freemacsoft.net/appcleaner/). AppCleaner just works™. It isn't updated very often, but I think that speaks to the rigidity of it. There are, however, a few other tools that I prefer.

### Raycast

[Raycast](https://www.raycast.com/) is a Spotlight replacement, and in doing so it adds a ton of functionality. You can probably see where this is going.

In Raycast's Action menu for any app, buried at the very bottom is "Uninstall Application." This, as you would expect, uninstalls the selected app. Just like AppCleaner, it also catches those spare, miscellaneous files.

Using Raycast is my preferred uninstall method because, if I'm going to use Raycast (which I am), then AppCleaner is redundant. Raycast is also just joyful to use.

### Homebrew

Homebrew, the go-to package manager for macOS, has "casks" for installing practically any app.

> Homebrew Cask extends Homebrew and brings its elegance, simplicity, and speed to the installation and management of GUI macOS applications.

Each cask makes use of zap. Zap is a Homebrew command that is used to get rid of all those spare files upon uninstallation.

The [cask](https://github.com/Homebrew/homebrew-cask/blob/master/Casks/1password.rb) for 1Password, for example, includes the following zap:

```
zap trash: [
    "~/Library/Application Scripts/2BUA8C4S2C.com.1password*",
    "~/Library/Application Scripts/com.1password.1password-launcher",
    "~/Library/Application Support/1Password",
    "~/Library/Application Support/Arc/User Data/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/CrashReporter/1Password*",
    "~/Library/Application Support/Google/Chrome Beta/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Google/Chrome Canary/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Google/Chrome Dev/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Google/Chrome/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Microsoft Edge Beta/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Microsoft Edge Canary/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Microsoft Edge Dev/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Microsoft Edge/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Mozilla/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/Vivaldi/NativeMessagingHosts/com.1password.1password.json",
    "~/Library/Application Support/com.apple.sharedfilelist/com.apple.LSSharedFileList.ApplicationRecentDocuments/com.1password.1password.sfl2",
    "~/Library/Containers/2BUA8C4S2C.com.1password.browser-helper",
    "~/Library/Containers/com.1password.1password*",
    "~/Library/Group Containers/2BUA8C4S2C.com.1password",
    "~/Library/Preferences/com.1password.1password.plist",
    "~/Library/Saved Application State/com.1password.1password.savedState",
  ]
```

So, to make use of this, you'd have to install and uninstall your apps with Homebrew.

Homebrew casks are community maintained, but any issues are resolved fairly quickly from what I've seen.

