https://stackoverflow.com/questions/7139338/change-jenkins-port-on-macos


Before modify the Jenkins port on macOS,you must pay attention to **the way of installation of Jenkins**.

Here I recommend you install Jenkins by 'Homebrew' if you want to deal with iOS project building,because you may meet some errors that the way of using `.pkg` to install,it's really hard to solve the problems.

I have installed Jenkins LTS by brew command:

`brew install jenkins-lts`

So my Jenkins plist file is here:

`/usr/local/Cellar/jenkins-lts/2.121.2/homebrew.mxcl.jenkins-lts.plist`

You can modify the `httpPort` value from default `8080` to the other value,and then save the file.

`<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd"> <plist version="1.0"> <dict> <key>Label</key> <string>homebrew.mxcl.jenkins-lts</string> <key>ProgramArguments</key> <array> <string>/usr/libexec/java_home</string> <string>-v</string> <string>1.8</string> <string>--exec</string> <string>java</string> <string>-Dmail.smtp.starttls.enable=true</string> <string>-jar</string> <string>/usr/local/opt/jenkins-lts/libexec/jenkins.war</string> <string>--httpListenAddress=127.0.0.1</string> <string>--httpPort=8383</string> </array> <key>RunAtLoad</key> <true/> </dict> </plist>`

`sudo launchctl unload` command will not work for you.You must try these commands to restart your Jenkins and make the port modification works.

`brew services stop jenkins-lts brew services start jenkins-lts`

``ifeegoo:~ ifeegoo$ brew services stop jenkins-lts Stopping `jenkins-lts`... (might take a while) ==> Successfully stopped `jenkins-lts` (label: homebrew.mxcl.jenkins-lts) ifeegoo:~ ifeegoo$ brew services start jenkins-lts ==> Successfully started `jenkins-lts` (label: homebrew.mxcl.jenkins-lts)``

**Note:If you installed Jenkins LTS,you must pay attention that your command must be `jenkins-lts`,not `jenkins`.**