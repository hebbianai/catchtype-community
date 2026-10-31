# What Catchtype sends over the internet

[한국어](network.md)

This page lists, for each platform, what the Catchtype apps send over the internet: what, when, where and why. It also says how to turn each one off and how to check for yourself.

- We checked this in the code on October 31, 2026: macOS and Linux 0.3.0-beta.14, Windows 0.3.0-beta.9.1, and the iPhone, iPad and Android code of the same day.
- When what we send changes, we update this page and say so in the update notes.

## Your keys and text

**The keys you press and the text you type never leave your device.** Korean or English is always decided on the device. This promise will not change. The only exceptions are things you choose to send yourself.

Today:

- There is no account. Nothing the apps send over the internet carries an ID for your device.
- Feedback: what you write in the feedback window is sent. If you choose "Attach Debug Log", the debug log is sent too. It holds each changed word before and after, so it is part of what you typed. It also holds the app name and the kind of input field, with times. Before you send, you can see it with "Show What Will Be Sent…".
- Moving words between devices: you move your learned words and a few settings yourself, by QR code or file. This does not use the internet, and Catchtype does not receive them. A file goes only where you send it.

## At a glance

| | Connects on its own | Connects only when you click |
|---|---|---|
| macOS | Update check once a day (can be turned off) | Installing a new version, sending feedback |
| Windows | Update check once a day (can be turned off) | Getting a new version, sending feedback |
| Linux | Update check once a day (can be turned off) | Nothing. Your browser opens feedback and downloads |
| iPhone and iPad | Nothing | Nothing. Your browser or the App Store opens links |
| Android | Nothing. It has no internet permission | Nothing. Your browser opens links |

On every platform, links in the menu and settings (this site, the privacy policy, the app stores) open in your browser or the store app. The apps do not fetch them.

## macOS

| What | When | Where | Why | How to turn it off |
|---|---|---|---|---|
| Update list (`GET /appcast.xml`) | About once a day, and when you click "Check for Updates" in Settings → About | `catchtype.com` | To see whether a new version or in-app news is out | Turn off "Check for updates once a day" in Settings → General |
| New version (`.dmg`) | Only when you choose to install in the update window | `catchtype.com` → `zm0bwixmbplndzsg.public.blob.vercel-storage.com` | To install the new version | Nothing downloads unless you click install |
| Feedback | Once, when you click Send in "Send Feedback…" | `catchtype.com`, plus `uoergtzcamayoebneffb.supabase.co` if you attach files | To deliver the feedback you chose to send | Sent only when you click |

- The update list request has no body and no query. Its User-Agent is just `Catchtype`. It does not include the app version, the macOS version or device details (the update tool's system profile is off).
- The standard headers macOS adds to web requests are sent. They include your preferred language list (`Accept-Language`).
- Updates are handled by [Sparkle](https://sparkle-project.org/), an open-source update tool. The update list and the new version are used only if their signatures are valid. Nothing installs on its own. When a new version is found, you only see a notification and a menu item.
- With automatic checks off, the app does not connect on its own, and in-app news is hidden. "Check for Updates" checks once, when you click it.
- What feedback sends: what you wrote, a way to reach you if you gave one, the app version, the OS name and version, your language, any photos (up to 3) and text file (1) you attach, and the debug log only if you choose it. Before you send, the window shows the app version, the OS and the attachment names. If sending fails, it is not retried on its own. How long we keep feedback is in the [privacy policy](https://catchtype.com/en/privacy?utm_source=github&utm_medium=referral&utm_campaign=community&utm_content=network_doc_feedback).
- Moving words between devices ("Send My Words") does not use the internet. The app draws a QR code on screen or makes a `.catchtype` file. A file goes only where you send it (AirDrop, Mail and so on).
- Permissions: Catchtype asks for Accessibility. It uses it to see keys typed into other apps, put in the corrected text and read the text before the cursor. The first time it has an update or secure-input notice to show, it also asks to show notifications.
- Catchtype for macOS is not an App Store app and runs outside the sandbox, so macOS does not limit its internet use. What it sends today is only what this table lists.

## Windows

| What | When | Where | Why | How to turn it off |
|---|---|---|---|---|
| Version file (`GET /version.json`) | At most once a day, at the earliest 1 minute after the app starts | `catchtype.com` | To see whether a new version or in-app news is out | Turn off "Check Once a Day" in the tray menu |
| New version (`.zip`) | Only when you click "Download Version …" in the menu or the new-version notification | `catchtype.com` → `zm0bwixmbplndzsg.public.blob.vercel-storage.com` | To replace the app with the new version | Nothing downloads unless you click |
| Feedback | Once, when you click Send in "Send Feedback…" | `catchtype.com`, plus `uoergtzcamayoebneffb.supabase.co` if you attach files | To deliver the feedback you chose to send | Sent only when you click |

- The version file request has no body. The only header Catchtype sets is the User-Agent, `Catchtype`, and cookies are off. The standard headers Windows adds are sent, such as Host, Connection and `Cache-Control: no-cache`. It follows the Windows proxy settings.
- With automatic checks off, the app makes no requests on its own, and the new-version item never appears. The setting is kept as `updateCheckOff` in `%APPDATA%\Catchtype\settings.json`.
- Before replacing anything, the app checks the zip's size, its SHA-256 and its Ed25519 signature against the key built into the app. If any of them does not match, nothing is changed.
- Feedback sends the same as on macOS.
- Permissions: none. The installer does not use the internet.

## Linux (IBus)

| What | When | Where | Why | How to turn it off |
|---|---|---|---|---|
| Version file (`GET /version.json`) | At most once a day, at the earliest 1 minute after the IBus input method starts | `catchtype.com` | To show a desktop notification when a new version is out | Turn off "Check Once a Day" in the IBus menu |
| Feedback form | When you choose "Send Feedback…" | Your browser opens `catchtype.com/feedback` | An IBus input method cannot open a window, so it uses the form on the site | Opens only when you choose it |

- The version file is fetched with the system's libcurl. Without libcurl, nothing is fetched. The input method sets no User-Agent and does not turn on cookies. It follows redirects only to https, up to 5 times.
- With automatic checks off, the input method does not connect to the internet. The setting is kept as `updateCheckOff` in `~/.config/catchtype/settings.json`.
- There is no in-app update on Linux. Clicking the new-version notification opens the download page in your browser. The `.deb` we ship today does not add an apt repository.
- The feedback form address carries the app version and your distribution name (`PRETTY_NAME` from `/etc/os-release`), as `?platform=linux&app=…&os=…`. So opening the page alone sends these two to catchtype.com. What you write is sent when you click Send in the form.
- Permissions: none, as an IBus input method. The `.deb` needs libcurl only for this version check.

## iPhone and iPad

- **The keyboard has no code that connects to the internet.** Every time before we upload the app, an automatic check makes sure no networking framework or socket call is in the keyboard. If one is found, we do not upload.
- **The app makes no requests of its own either.** The same check makes sure there is no web request code (URLSession) anywhere in the app.
- In the app, "Report a problem on the website" opens `catchtype.com/feedback` in your browser, and "Write a public App Store review" opens the App Store. From there, your browser and the App Store take over.
- The app sometimes asks iOS to show the system review prompt. The App Store shows and handles that prompt.
- Permissions: it does not ask for the camera, microphone, location, contacts, photos or notifications.

### Why the keyboard asks for "Allow Full Access"

On iOS, a keyboard needs "Allow Full Access" for the things below. The Catchtype keyboard uses it for these three only.

1. A light vibration when you press a key (iPhone).
2. The on-device storage it shares with the Catchtype app (an App Group). Settings you change in the app reach the keyboard right away, and words the keyboard learned show up in the app.
3. The Paste suggestion for what you just copied. Copied text is read following the iPhone's paste permission ("Paste from Other Apps" in Settings).

The warning iOS shows when you turn it on is the same text for every keyboard. Full Access would let a keyboard use the internet, but the Catchtype keyboard has no code for it.

**Without it:** typing and the Korean/English decision work the same. There is no vibration, and settings you change in the app may not reach the keyboard (it depends on the iOS version). You can change them in the keyboard's quick settings instead (touch and hold the logo key). There is no Paste suggestion, and words the keyboard learned do not show up in the app.

### Moving words between devices

The iPhone's own camera app reads the QR code on your computer screen. The Catchtype app uses neither the camera nor the internet. From the phone to the computer, you send a `.catchtype` file with the share sheet. The file goes only where you send it.

## Android

- **The Android keyboard has no internet permission (`android.permission.INTERNET`).** Android does not let an app without it connect to the internet.
- The release build asks for no permissions at all. If a library we use adds one, the build stops.
- Vibration uses the system's touch feedback, so it needs no vibration permission.
- To use the keyboard, you turn on Catchtype in Android settings. The warning shown then ("may be able to collect all the text you type") is the same text for every keyboard.
- App data is excluded from Google cloud backup and from device-to-device transfer.
- In Settings, "Report a problem" opens `catchtype.com/feedback` in your browser, "Leave a review" opens Google Play, and "Privacy policy" opens `catchtype.com/privacy`.
- The app sometimes asks for Google Play's review card. The Play Store app on your phone shows and handles it.
- Moving words between devices works as on iPhone. Your phone's camera app reads the QR code, and from the phone to the computer you share a file.

## Check it yourself

Blocking the connections does not change typing or the Korean/English decision. What stops working is the update check and, on macOS and Windows, getting a new version in the app and sending feedback. To use Catchtype with no connection from the start, block Catchtype in a firewall app and turn off automatic checks. On Windows and Linux, the app checks at most once in 24 hours, at the earliest 1 minute after it starts. Restarting it does not make it check sooner.

- **macOS:** With [LuLu](https://objective-see.org/products/lulu.html) (free, open source) or [Little Snitch](https://www.obdev.at/products/littlesnitch/), you can see where Catchtype connects and block it. Clicking "Check for Updates" in Settings → About makes one connection right away, which makes it easy to see. In Terminal, `lsof -i -n -P | grep -i catchtype` shows open connections.
- **Windows:** Use [Wireshark](https://www.wireshark.org/) with the filter `dns.qry.name contains "catchtype"`, or a firewall app such as [GlassWire](https://www.glasswire.com/). In Windows Resource Monitor, look for `Catchtype.exe` on the Network tab.
- **Linux:** Use Wireshark (same filter), `sudo nethogs`, or `sudo ss -tnp | grep ibus-engine-cat`. The version check happens at most once a day, so a tool you leave running works best.
- **iPhone and iPad:** Turn on Settings → Privacy & Security → App Privacy Report to see the domains each app contacted. You can also route the iPhone through a proxy on your computer (mitmproxy, Proxyman and so on).
- **Android:** Use an app that logs connections per app through a local VPN, such as [PCAPdroid](https://github.com/emanuele-f/PCAPdroid) (free, open source). With a computer, `adb shell dumpsys package com.hebbianai.catchtype` shows the permissions it asks for. The internet permission is not there.

## If a future feature needs the internet

Features that need the internet may come, such as Pro purchases and license checks, syncing between devices, or cloud processing for voice dictation. When they do:

- Each one can be turned on and off by you.
- When what we send changes, we say so in the update notes and on this page.

## The engine and the source code

Catchtype's input engine is not open source. That is why we describe what the apps do on this page, so anyone can check it with the methods above.

Questions are welcome in the [community](https://github.com/hebbianai/catchtype-community/discussions) or at hebbianai@gmail.com.
