---
title: "Set Windows 7 as WiFi HotSpot"
date: 2010-08-26
categories: 
  - "technical"
---

Windows 7 comes with a new feature for sharing your computers internet connection. Now you can do it wireless. Well, As long as your system’s wireless network adapter supports Windows 7 “virtual wi-fi” feature, you can do it.

Next you need to check if your network card has the latest driver to support the feature. Intel, Broadcom, Realtek and some other NIC makers provide such drivers.

If the two above conditions are met, you are good to read on …

Open an elevated command prompt (Type “CMD.EXE” in the start menu search bar, then right clicking the cmd.exe link and selecting “Run as Administrator”).

- In the command window, type: **`netsh wlan set hostednetwork mode=allow ssid=MyNet key=MyPassword`** Replace “MyNet” with the name you want to use for the wireless network and replace “MyPassword” with your chosen password for accessing it)
- Press Enter.
- Now type: **`netsh wlan start hostednetwork`**
- Press Enter.
- Open Control Panel and go to Network and Sharing Center.
- Click Change Adapter Settings.
- Right click your Internet connection and select Properties.
- Click the Sharing tab.
- Check the box that says “Allow other network users to connect.”
- Choose your virtual wi-fi adapter and click OK.

Now wi-fi enabled computers within range should see your new wireless network and others can connect to it if they know the password.
