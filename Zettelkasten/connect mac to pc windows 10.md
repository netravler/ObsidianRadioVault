# How to connect Mac and Windows 10 PC and share files over a network

### File sharing between a Windows 10 PC and a Mac (running Mac OS X or macOS) is more complex than you'd imagine. Here we show you how to share files between two networked computers - a Mac and a Windows 10 PC

By [Lucy Hattersley](https://www.macworld.co.uk/author/lou-hattersley/), Contributor![Lucy Hattersley](https://www.macworld.co.uk/cmsdata/author/3488813/lou-hattersley-mw_thumb100.jpg) | 13 Oct 16

![share windows 10 mac files network](https://www.macworld.co.uk/cmsdata/features/3647667/share_windows_10_mac_files_network_thumb800.jpg "Click for slideshow")

_How do I send files from my Windows 10 PC to my Mac over a network?_

Connecting a Windows 10 computer to a Mac is more complex than you'd imagine.

In macOS you just use the Shared section of a Finder window to locate nearby Macs (or use AirDrop). Windows 10 users use Workgroups to network machines together. But it is strangely difficult to connect a Windows 10 machine to a Mac over a network.

The windows PC will appear in Shared on your Mac, but clicking it and clicking Connect As... reveals just "There was a problem connecting to the server".

No matter. In this article we show how it's done. Here's how to connect a Mac and a Windows 10 PC and share files over a network.

**See also:**

[macOS  vs Windows 10 comparison](https://www.macworld.co.uk/review/mac-os-x-el-capitan-vs-windows-10-comparison-3615299/)

[Run Windows 10 on your Mac using VirtualBox](https://www.macworld.co.uk/how-to/install-windows-mac-3497251/)

[How to install Windows on Mac | How to run Windows on Mac](https://www.macworld.co.uk/how-to/install-windows-mac-3497251/)

[Manage Macs on a Windows-based network](https://www.macworld.co.uk/how-to/use-your-mac-on-windows-based-network-3594407/)

## How to share files between a Windows and Mac

Here's how to connect to your Windows PC from a Mac and copy files to (and from) each machine.

![How to connect Mac and Windows 10 PC and share files over a network: Find your IP address on Windows PC](https://www.macworld.co.uk/cmsdata/features/3647667/windows_to_mac_ip_address.jpg "Click for slideshow")

1.  Make sure both your Windows 10 machine and your Mac are connected to the same network.
2.  Click Cortana in Windows 10 and enter "Command Prompt". Open the Command Prompt app. Maximise the screen so you can get a good view of everything.
3.  Enter **ipconfig** and press Return.
4.  Locate your IP address. It'll be marked as IPv4 Address and either under Ethernet adapter or Wireless LAN Adapter Wi-Fi (depending on your address). It'll be four sets of numbers, eg :192.168.0.20
5.  Now jump over to your Mac. Press Command + K.
6.  Enter smb:// and the ip address. IE, "smb://192.168.0.20" and press Return.
7.  Click Registered User and enter the username and password you use to sign on to the Windows 10 PC. This might be your Microsoft Account and Password or the User ID and Password for your account.
8.  Click Connect. It can take a few minutes to connect over a Wi-Fi connection.
9.  A window marked "Select the volumes you want to mount" will appear with one option "Users". Click OK.
10.  Open a Finder window.
11.  You will see the SMB share marked as the IP address in Shared in the sidebar. Click it, click Users and your username. Here are all the files on your Windows PC.

You can copy files to and from the Windows machine from here.

![How to connect Mac and Windows 10 PC and share files over a network: Share files between Windows 10 and Mac](https://www.macworld.co.uk/cmsdata/features/3647667/windows_mac_smb.jpg "Click for slideshow")

**Read next:** [How to connect a MacBook to a TV](https://www.macworld.co.uk/how-to/connect-tv-3591712/ "How to connect a MacBook to a TV") | [How to connect an iPhone to a Windows 10 PC](https://www.macworld.co.uk/how-to/how-connect-iphone-windows-10-pc-3648165/ "How to connect an iPhone to a Windows 10 PC")