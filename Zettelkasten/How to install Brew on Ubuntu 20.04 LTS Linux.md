# How to install Brew on Ubuntu 20.04 LTS Linux

[Heyan Maurya](https://www.how2shout.com/linux/author/heyan/ "Posts by Heyan Maurya")  Last Updated: April 27, 2021 [UBUNTU](https://www.how2shout.com/linux/category/ubuntu/ "View all posts in Ubuntu") [NO COMMENTS](https://www.how2shout.com/linux/how-to-install-brew-ubuntu-20-04-lts-linux/#respond)

**Homebrew** is one of the popular package managers for Mac OS X but can be installed on Linux as well to download and install various packages. Homebrew Cask extends Homebrew with support for quick installation of applications like Google Chrome, VLC, and more.

On Linux, it is known as Linuxbrew. On Ubuntu Linux, we already have an APT package manager with a wide range of applications and other packages to install, then what is the need for **Linuxbrew?**

**What is the difference between APT and Homebrew or Linuxbrew?**

**1.** Both **HomeBrew** and APT’s main goal is the same that is the installation of various packages using the command line. However, on one hand, APT is the native and well-integrated manager of Debian-based systems including Ubuntu, HomerBrew is a third-party package manager that the user can install manually.

**2.** Difference in the command syntax, if a user is already familiar with the brew command line on macOS and new to Ubuntu then he or she can use it to install programs without learning new command hooks. However, if you are a user of Ubuntu or Debian, then for sure you don’t need to install it.

**3.** Homebrew maintains a separate user-owned directory, thus no need to run it with `sudo` to install applications.

**4.** Where apt-get is generally designed to overwrite previous versions; in the brew, it compiles packages and saves them according to its version in subdirectories. This means we can have multiple versions of a package on the same machine at the same time, however, only one of them will get symlinked into your main Homebrew hierarchy.

**5.** APT cleanup or uninstall older packages automatically with the update, whereas, in Homebrew, a user needs to run the brew cleanup command.

Contents [[show](https://www.how2shout.com/linux/how-to-install-brew-ubuntu-20-04-lts-linux/#)]

## HomeBrew installation on Ubuntu 20.04 Linux

### 1. Open a command terminal

Run terminal and then first, issue an update command-

Copy Me

sudo apt update

Copy Me

sudo apt-get install build-essential

### 2. Install Git on Ubuntu 20.04

For setting up LinuxBrew on Ubuntu 20.04 or 18.04, we need to install GIT on our system, here is the command for that…

Copy Me

sudo apt install git -y

### 3. Run Homebrew installation script

The official website of Brew offers a pre-build script to install download and install Homebrew using the command line on any available Linux such as CentOS, RHEL, OpenSUSE, Linux Mint, Kali, MX Linux, POP!OS and others.

Here is the command, just run it-

Copy Me

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

### 4. Add Homebrew to your PATH

To run the brew command after installation, we need to add it in our system path…

Copy Me

eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

[![Home Brew installation on Ubuntu 20.04](https://www.how2shout.com/linux/wp-content/uploads/2021/04/Home-Brew-installation-on-Ubuntu-20.04.jpg "Home Brew installation on Ubuntu 20.04")](https://www.how2shout.com/linux/wp-content/uploads/2021/04/Home-Brew-installation-on-Ubuntu-20.04.jpg)

### 5. Check Brew is working fine

To ensure everything is working correctly to use brew, we can run its command-

Copy Me

brew doctor

It may give the warning to install GCC and to remove that simply install it using brew-

Copy Me

brew install gcc

### 6. Uninstall it from Linux

If you want to remove Homebrew, then here is the brew uninstallation script, also available on GitHub.

Copy Me

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"

[![Brew uninstall Ubuntu script](https://www.how2shout.com/linux/wp-content/uploads/2021/04/Brew-uninstall-Ubuntu-script.jpg "Brew uninstall Ubuntu script")](https://www.how2shout.com/linux/wp-content/uploads/2021/04/Brew-uninstall-Ubuntu-script.jpg)

**Other Articles:**

-   [How to install Brew on WSL- Windows subsystem for Linux](https://www.how2shout.com/linux/install-brew-on-wsl-windows-subsystem-for-linux/)
-   [Install SNAP on Linux Mint 20](https://www.how2shout.com/linux/how-to-install-snap-on-linux-mint-20/)
-   [Install latest Linux Kernel on Ubuntu 20.04 Focal Fossa via PPA](https://www.how2shout.com/linux/install-linux-kernel-ubuntu-20-04/)
    
-   

#### RELATED POSTS

[

![Visual Code Studio installation on Ubuntu Linux using .deb file](https://www.how2shout.com/linux/wp-content/uploads/2021/06/Visual-Code-Studio-installation-on-Ubuntu-Linux-using-.deb-file-400x200.jpg)

](https://www.how2shout.com/linux/3-ways-install-visual-studio-code-in-ubuntu-using-terminal/)

[Heyan Maurya](https://www.how2shout.com/linux/author/heyan/ "Posts by Heyan Maurya") [UBUNTU](https://www.how2shout.com/linux/category/ubuntu/ "View all posts in Ubuntu")

## [3 Ways to install Visual studio code in Ubuntu using terminal](https://www.how2shout.com/linux/3-ways-install-visual-studio-code-in-ubuntu-using-terminal/ "3 Ways to install Visual studio code in Ubuntu using terminal")

[

![photoshop CS6 interface on Ubuntu 20.04](https://www.how2shout.com/linux/wp-content/uploads/2021/07/photoshop-CS6-interface-on-Ubuntu-20.04-400x200.png)

](https://www.how2shout.com/linux/install-adobe-photoshop-cs6-on-ubuntu-20-04-lts-linux/)

[Heyan Maurya](https://www.how2shout.com/linux/author/heyan/ "Posts by Heyan Maurya") [UBUNTU](https://www.how2shout.com/linux/category/ubuntu/ "View all posts in Ubuntu")

## [How to install Adobe Photoshop CS6 on Ubuntu 20.04 LTS Linux](https://www.how2shout.com/linux/install-adobe-photoshop-cs6-on-ubuntu-20-04-lts-linux/ "How to install Adobe Photoshop CS6 on Ubuntu 20.04 LTS Linux")

[

![Etherpad Lite installation on Ubuntu 20.04 Server](https://www.how2shout.com/linux/wp-content/uploads/2021/10/Etherpad-Lite-installation-on-Ubuntu-20.04-Server-e1633853580858-400x200.png)

](https://www.how2shout.com/linux/how-to-install-etherpad-lite-on-ubuntu-20-04-lts/)

[Heyan Maurya](https://www.how2shout.com/linux/author/heyan/ "Posts by Heyan Maurya") [UBUNTU](https://www.how2shout.com/linux/category/ubuntu/ "View all posts in Ubuntu")

## [How to Install Etherpad Lite on Ubuntu 20.04 LTS](https://www.how2shout.com/linux/how-to-install-etherpad-lite-on-ubuntu-20-04-lts/ "How to Install Etherpad Lite on Ubuntu 20.04 LTS")

[

![Kibana Dashboard on Ubuntu 22.04](https://www.how2shout.com/linux/wp-content/uploads/2022/01/Kibana-Dashboard-on-Ubuntu-22.04-400x200.png)

](https://www.how2shout.com/linux/how-to-install-kibana-dashboard-on-ubuntu-22-04-20-04-lts/)

[k0fpsa](https://www.how2shout.com/linux/author/k0fpsa/ "Posts by k0fpsa") [UBUNTU](https://www.how2shout.com/linux/category/ubuntu/ "View all posts in Ubuntu")

## [How to install Kibana Dashboard on Ubuntu 22.04 | 20.04 LTS](https://www.how2shout.com/linux/how-to-install-kibana-dashboard-on-ubuntu-22-04-20-04-lts/ "How to install Kibana Dashboard on Ubuntu 22.04 | 20.04 LTS")