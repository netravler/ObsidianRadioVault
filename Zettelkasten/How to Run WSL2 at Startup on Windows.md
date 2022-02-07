[![David Littlefield](https://miro.medium.com/fit/c/96/96/1*sKlMOPY2X0X4EEYDcn-VBg.png)](https://medium.com/@david-littlefield?source=post_page-----c4567d6c48f1-----------------------------------)

[David Littlefield](https://medium.com/@david-littlefield?source=post_page-----c4567d6c48f1-----------------------------------)


## THE FOUNDER’S GUIDE:

# How to Run WSL2 at Startup on Windows

## The expanded version with explanations and screenshots

![](https://miro.medium.com/max/700/1*yaEnX4jaxGjYwT4jcaLOcA.png)

Image by [Bram Van Oost](https://unsplash.com/photos/soFr1hofDfU)

> “The [condensed version](https://medium.com/p/4ccca87487c0) of this article uses copy and paste code to help you get the outcome ASAP ⚡”

## Open PowerShell:

_PowerShell_ is a command-line [shell](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#05c0) and object-oriented scripting language that’s used to automate administrative tasks and configure system settings It can be used to automate practically anything in the operating system. It also replaced Command Prompt as the default system shell for Windows 10.

1.  Press “⊞ Windows”
2.  Enter “PowerShell” into the search bar
3.  Click “Run as Administrator”

![](https://miro.medium.com/max/700/1*xr_-IDQJImC1M_LtuYvjaA.png)

## Create the Visual Basic Script File:

The _Echo_ command is used to print text it receives from the argument to the screen or to a computer file. It can be used in shell scripts and batch files to print the output of other commands or as part of other commands to insert text. It can also overwrite the text in an existing file or append it at the end.

1.  Copy the command from below these instructions
2.  Paste the command into PowerShell
3.  Press “Enter”

echo "" > $HOME\run_wsl2_at_startup.vbs

![](https://miro.medium.com/max/700/1*vrqFIIk9BSAGnB6cZ8x8Qg.png)

## Open the Visual Basic Script File:

The _Visual Basic Script File (vbs)_ is a script file that contains source code to be executed with a Windows Scripting Host program. It can be run using Wscript which is intended for interactive window programs. It can also be ran using Cscript which is intended for non-interactive console programs.

1.  Copy the command from below these instructions
2.  Paste the command into PowerShell
3.  Press “Enter”

notepad $HOME\run_wsl2_at_startup.vbs

![](https://miro.medium.com/max/700/1*hLfdTtdJDmm7ZxdmE2QNMA.png)

## Edit the Visual Basic Script File:

Task Scheduler can’t start WSL2 in the background as expected at startup because background processes are run in session zero. It causes programs that run from the command-line to start without a window. It can also be easily fixed with a [visual basic script](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#df30) that controls how the process starts.

1.  Find the Ubuntu version from below these instructions
2.  Copy the provided code
3.  Paste the code into Notepad
4.  Click the “File” menu
5.  Click “Save”

**Ubuntu 20.04:**  
set object = [createobject](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#7f80)("[wscript](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#994e).[shell](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#adae)")   
object.[run](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#eb04) "wsl.exe --distribution Ubuntu-20.04", 0**Ubuntu 18.04:**  
set object = createobject("wscript.shell")   
object.run "wsl.exe --distribution Ubuntu-18.04", 0

![](https://miro.medium.com/max/700/1*-nc1llwKeHK7lFS5_A8uqQ.png)

## Open Task Scheduler:

_Task Scheduler_ is a program that automatically performs tasks when certain criteria are met. It can start programs, execute commands, and run scripts at specified times or after specified time intervals and events. It can also be programmed using the graphical user interface or command-line interface.

1.  Press “⊞ Windows”
2.  Enter “Task Scheduler” into the search bar
3.  Click “Task Scheduler”

![](https://miro.medium.com/max/700/1*lQKR7WkseBs-4MbV0NrrOA.png)

## Create the Task:

The _Task_ is the work the Task Scheduler performs according to the action, trigger, principal, and settings that are specified by the user. It refers to the action that’s taken using the principal account in response to the trigger. It also includes the action to take in the circumstances listed in the settings.

1.  Click “Create Basic Task” in the right panel
2.  Copy the text from below these instructions
3.  Paste the text into the “Name” text field
4.  Click “Next”
5.  Select “When the Computer Starts”
6.  Click “Next”
7.  Select “Start a Program”
8.  Click “Next”

Windows Subsystem for Linux 2 (WSL2)

![](https://miro.medium.com/max/696/1*-gSBRoPV7f1vGBhxev3k2Q.png)

## Specify the Action:

The _Action_ is used in Task Scheduler to specify the work that’s performed when a task is triggered. I can launch programs, execute commands, run scripts, and open files. It can also perform a single action and multiple actions which can include up to 32 actions that are executed sequentially.

1.  Copy the path from below these instructions
2.  Paste the path into the “Program/Script” text field
3.  Click “Next”
4.  Click “Yes”
5.  Check “Open the Properties Dialog…”
6.  Click “Finish”

%USERPROFILE%\run_wsl2_at_startup.vbs

![](https://miro.medium.com/max/696/1*5S1cwQQVixLhXA-PVzjwHQ.png)

## Specify the Trigger:

The _Trigger_ is used in Task Scheduler to set the conditions that determine whether a task is performed. It can start a task using time-based triggers and event-based triggers. It can also start a task using a single trigger and multiple triggers which can include up to 48 triggers that all start the task.

1.  Click “Triggers” in the top tab bar
2.  Click “At Startup”
3.  Click “Edit”
4.  Check “Delay Task For”
5.  Enter “30 Seconds”
6.  Click “OK”

![](https://miro.medium.com/max/591/1*bIX_CkexAEsj7pGC5Wj1og.png)

## Run in the Background:

The _Run Whether User Is Logged in or Not_ option is used in Task Scheduler to run a task even if the user is logged out. It runs the task in the background without a window which can’t be used interactively. It also causes processes that are started by the task to operate in the background without a window.

1.  Click “General” in the tab bar
2.  Select “Run Whether User Is Logged On or Not”
3.  Check “Run With Highest Privileges”

![](https://miro.medium.com/max/632/1*sB6QXB9q0-35_EBhbHrugQ.png)

## Remove the Time Limit:

_The Stop the Task if It Runs Longer Than_ setting is used in Task Scheduler to limit the length of time that a task is able to run. It stops the task after the specified time limit is reached which prevents the task from taking too long to execute. It also prevents a frozen task from potentially running forever.

1.  Click “Settings” in the tab bar
2.  Uncheck “Stop the Task if It Runs Longer Than”
3.  Click “OK”
4.  Restart the computer

![](https://miro.medium.com/max/632/1*JCbhka2vYW3YaRKP6f_sZQ.png)

## Open PowerShell:

_PowerShell_ is a command-line shell and object-oriented scripting language that’s used to automate administrative tasks and configure system settings It can be used to automate practically anything in the operating system. It also replaced Command Prompt as the default system shell for Windows 10.

1.  Press “⊞ Windows”
2.  Enter “PowerShell” into the search bar
3.  Click “Run as Administrator”

![](https://miro.medium.com/max/700/1*xr_-IDQJImC1M_LtuYvjaA.png)

## Get Running Processes:

The _Get-Process_ command is used in PowerShell to access all the processes that are currently running on the computer. It displays every process that’s running by default. It also specifies a process name or process ID to display the specific process that’s running which can be piped to other commands.

1.  Copy the command from below these instructions
2.  Paste the command into PowerShell
3.  Press “Enter”

get-process

![](https://miro.medium.com/max/700/1*hi3pqaYZxTFIftTw1-i2Sg.png)

## Bonus: Run a Program At Startup:

Task Scheduler can’t start WSL2 in the background as expected at startup because background processes are run in session zero. It causes programs that run from the command-line to start without a window. It can also be easily fixed with a visual basic script that controls how the process starts.

1.  Find the Ubuntu version from below these instructions
2.  Replace the “Program” with the program name
3.  Replace the “Arguments” with the arguments
4.  Copy the provided code
5.  Paste the code into Notebook
6.  Click the “File” menu
7.  Click “Save”

**Ubuntu 20.04:**  
set object = createobject("wscript.shell")   
object.run "wsl.exe --distribution Ubuntu-20.04 program arguments", 0**Ubuntu 18.04:**  
set object = createobject("wscript.shell")   
object.run "wsl.exe --distribution Ubuntu-18.04 program arguments", 0

![](https://miro.medium.com/max/700/1*fpjHSwbTl01kUgRN_wLCJg.png)

> _“Hopefully, this article helped you get the 👯‍♀️🏆👯‍♀️, remember to subscribe to get more content 🏅”_

## Next Steps: AI, Machine Learning, and Deep Learning

This article is part of a mini-series that helps readers set up everything they need to start using WSL2 for artificial intelligence, machine learning, deep learning, and or data science. It includes articles that contain instructions with copy and paste code and screenshots that help readers get the outcome as soon as possible. It also includes articles that contain instructions with explanations and screenshots that help readers process what’s happening.

01. [Install Windows Subsystem for Linux 2 (WSL2)](https://medium.com/swlh/how-to-install-the-windows-subsystem-for-linux-2-wsl2-779b9fd2cadc)  
02. [Install the NVIDIA CUDA Driver and Toolkit in WSL2](https://thealtruist.medium.com/how-to-install-the-nvidia-cuda-toolkit-11-in-wsl2-88292cf4ab77)  
03. [Install Software From Source Code in WSL2](https://medium.com/swlh/how-to-install-software-from-the-source-code-in-wsl2-7525fcfb71b2)  
04. [Install the Jupyter Notebook Home and Public Server in WSL2](https://medium.com/swlh/how-to-set-up-a-jupiter-notebook-server-and-access-it-from-a-local-or-remote-network-on-windows-d335c5ba490d)  
05. [Install Virtual Environments in Jupyter Notebook in WSL2](https://thealtruist.medium.com/how-to-use-virtual-environments-with-jupyter-notebook-in-wsl2-32d809531c33)  
06. [Install Programs With a Graphical User Interface in WSL2](https://thealtruist.medium.com/how-to-install-programs-with-a-graphical-user-interface-gui-in-wsl2-33203d7344ab)07. [Install Ubuntu Desktop With a Graphical User Interface in WSL2](https://thealtruist.medium.com/how-to-install-ubuntu-desktop-with-a-graphical-user-interface-in-wsl2-7939aa8cb8db)

## Glossary:

The _Shell_ is an [interpreter](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#a031) that presents the [command-line interface](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#bd35) to users and allows them to interact with the [kernel](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#7bb8). It lets them control the system using commands entered from a keyboard. It also translates the commands from the programming language into the machine language for the kernel.  
[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#c7bb)]

The _Interpreter_ is a program that reads through instructions that are written in human-readable programming languages and executes the instructions from top to bottom. It translates each instruction to a machine language the hardware can understand, executes it, and proceeds to the next instruction.[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#05c0)]

The _Command-Line Interface (CLI)_ is a program that accepts text input from the user to run commands on the operating system. It lets them configure the system, install software, and access features that aren’t available in the graphical user interface. It also gets referred to as the terminal or console.[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#05c0)]

The _Kernel_ is the program at the heart of the operating system that controls everything in the computer. It facilitates the memory management, process management, disk management, and task management. It also facilitates communication between the programs and hardware in machine language.[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#05c0)]

_Visual Basic Script (VBScript)_ is a scripting language that’s based on Visual Basic. It was created for developing web pages but it has become a popular language for writing batch files on Windows. It can also be executed using Windows Scripting Host to interact with the Windows operating system.  
[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#fcdf)]

_Create Object_ is a function in Visual Basic Script that creates an [instance](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#c8af) of a [Component Object Model](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#f49d) object. It specifies the programmatic identifier to access the methods and properties in the component object model. This lets users launch and control external programs that have a program identifier.  
[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#fcdf)]

The _Instance_ refers to the distinct identity of a particular object that exists among zero or more objects that belong to the same class. It has the same attributes as the other objects but the attribute values are usually different. It can also have the same values but it would still be a different instance.[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#7f80)]

The _Component Object Model (COM)_ is an object model and programming standard that allows objects to interact with other objects using different programming languages and operating systems. It defines one or more sets of related functions that can be used to start and control other programs.[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#7f80)]

The _WScript_ object is used in Visual Basic Script to create an instance of Windows Script Host. It runs scripts in multiple programming languages that use object models to perform tasks. It also has access to programs, shortcuts, environment variables, the registry, and the operating system.  
[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#fcdf)]

The WshShell object is used in Visual Basic Script to allow users to interact with the Windows operating system. It has access to environment variables, system folders, and the registry. It can also create shortcuts, display pop-up windows, and run programs with command-line arguments and keystrokes.[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#fcdf)]

The _Run_ method is used in Visual Basic Script to run programs. It can run graphical programs, command-line programs, and scripts using command-line arguments and keystrokes. It can also run programs with or without a window, activate program windows, and pause programs temporarily.[[Return](https://medium.com/swlh/how-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1#fcdf)]

95

2

## Sign up for Top 10 Stories

### By The Startup

Get smarter at building your thing. Subscribe to receive The Startup's top 10 most read stories — delivered straight into your inbox, twice a month. [Take a look.](https://medium.com/swlh/newsletters/top-10-stories?source=newsletter_v3_promo--------------------------newsletter_v3_promo--------------)

Get this newsletter

Emails will be sent to pheintz2@gmail.com.[Not you?](https://medium.com/m/signin?operation=login&redirect=https%3A%2F%2Fmedium.com%2Fswlh%2Fhow-to-run-ubuntu-in-wsl2-at-startup-on-windows-10-c4567d6c48f1&collection=The+Startup&collectionId=f5af2b715248&newsletterV3=Top+10+Stories&newsletterV3Id=13df37cfc4c2&source=newsletter_v3_promo--------------------------newsletter_v3_promo----------13df37cfc4c2----)

## [More from The Startup](https://medium.com/swlh?source=post_page-----c4567d6c48f1-----------------------------------)

Follow

Get smarter at building your thing. Follow to join The Startup’s +8 million monthly readers & +752K followers.

![Samuel Robles](https://miro.medium.com/fit/c/24/24/1*3fkH5DSQVAAiAkufCiTP2A.jpeg)

[

Samuel Robles

](https://medium.com/@sam-robles-199?source=post_page-----c4567d6c48f1----0-------------------------------)

[

·Dec 3, 2020

](https://medium.com/swlh/how-to-create-a-linked-list-in-javascript-1bfef32c7722?source=post_page-----c4567d6c48f1----0-------------------------------)

[

## How to Create a Linked List in JavaScript

One of the first things you are told to learn as you wish to progress your career in the world of programming are data structures. …



](https://medium.com/swlh/how-to-create-a-linked-list-in-javascript-1bfef32c7722?source=post_page-----c4567d6c48f1----0-------------------------------)

[

Data Structures

](https://medium.com/tag/data-structures?source=post_page-----c4567d6c48f1---------------data_structures--------------------)

[

12 min read

](https://medium.com/swlh/how-to-create-a-linked-list-in-javascript-1bfef32c7722?source=post_page-----c4567d6c48f1----0-------------------------------)

---

Share your ideas with millions of readers.

[Write on Medium](https://medium.com/new-story?source=post_page_footer_cta_write----------------------------------------)

---

![Vivek Menon](https://miro.medium.com/fit/c/24/24/1*OzV63pFxRqhmoq4MYLnwuA.jpeg)

[

Vivek Menon

](https://medium.com/@viveksmenon?source=post_page-----c4567d6c48f1----1-------------------------------)

[

·Dec 3, 2020

](https://medium.com/swlh/understanding-probability-distribution-b5c041f5d564?source=post_page-----c4567d6c48f1----1-------------------------------)

[

## Understanding Probability Distribution

What is Probability Distribution? What are different types of Probability distributions? How will it help in framing Data Science Solutions?. Let me try to explain it in very simple words Definition:-A probability distribution is the mathematical function that gives the probabilities of occurrence of different possible outcomes for an experiment. …



](https://medium.com/swlh/understanding-probability-distribution-b5c041f5d564?source=post_page-----c4567d6c48f1----1-------------------------------)

[

Probability Distributions

](https://medium.com/tag/probability-distributions?source=post_page-----c4567d6c48f1---------------probability_distributions--------------------)

[

9 min read

](https://medium.com/swlh/understanding-probability-distribution-b5c041f5d564?source=post_page-----c4567d6c48f1----1-------------------------------)

[

![Understanding Probability Distribution](https://miro.medium.com/fit/c/112/112/1*nOMS0KgevT7YfqtfnhgXUg.png)

](https://medium.com/swlh/understanding-probability-distribution-b5c041f5d564?source=post_page-----c4567d6c48f1----1-------------------------------)

---

![Yegor Shytikov](https://miro.medium.com/fit/c/24/24/1*ibp2_dBO2I9aLgbetUYpqQ@2x.jpeg)

[

Yegor Shytikov

](https://medium.com/@yegorshytikov?source=post_page-----c4567d6c48f1----2-------------------------------)

[

·Dec 3, 2020

](https://medium.com/swlh/magento-2-4-elasticserch-vs-mysql-search-performance-1c6ddd85f5e4?source=post_page-----c4567d6c48f1----2-------------------------------)

[

## Magento 2.4 ElasticSerch vs. MySQL Search Performance

With Magento 2, we could use an open-source solution such as Apache Solr or Elasticsearch. We could use a managed solution such as Klevu or Algolia or use MySQL full-text search, which was the default one before the M2.4 release. However, it is better to use MYSQL for the following…



](https://medium.com/swlh/magento-2-4-elasticserch-vs-mysql-search-performance-1c6ddd85f5e4?source=post_page-----c4567d6c48f1----2-------------------------------)

[

Magento

](https://medium.com/tag/magento?source=post_page-----c4567d6c48f1---------------magento--------------------)

[

12 min read

](https://medium.com/swlh/magento-2-4-elasticserch-vs-mysql-search-performance-1c6ddd85f5e4?source=post_page-----c4567d6c48f1----2-------------------------------)

[

![Magento 2.4 ElasticSerch vs. MySQL Search Performance](https://miro.medium.com/fit/c/112/112/1*3VUEm_FH0UeDHAFMbBtRxg.png)

](https://medium.com/swlh/magento-2-4-elasticserch-vs-mysql-search-performance-1c6ddd85f5e4?source=post_page-----c4567d6c48f1----2-------------------------------)

---

![Hantsy](https://miro.medium.com/fit/c/24/24/1*hvpS8wQrCuh4tXDq2Yy08g.png)

[

Hantsy

](https://medium.com/@hantsy?source=post_page-----c4567d6c48f1----3-------------------------------)

[

·Dec 3, 2020

](https://medium.com/swlh/testing-jakarta-ee-9-components-with-arquillian-and-weld-f9e1add08895?source=post_page-----c4567d6c48f1----3-------------------------------)

[

## Testing Jakarta EE 9 Components With Arquillian and JBoss Weld

Arquillian (JBoss Arquillian) Core 1.7.0 added Jakarta EE 9 and the long-awaited JUnit 5 support. For impatient developers, you can try to run your Jakarta EE 9/JUnit 5 based Arquillian tests against Weld container, Glassfish v6 (both managed and remote) and Apache Tomcat 10 (for Jakarta Servlet 5.0). In this…



](https://medium.com/swlh/testing-jakarta-ee-9-components-with-arquillian-and-weld-f9e1add08895?source=post_page-----c4567d6c48f1----3-------------------------------)

[

Cdi

](https://medium.com/tag/cdi?source=post_page-----c4567d6c48f1---------------cdi--------------------)

[

3 min read

](https://medium.com/swlh/testing-jakarta-ee-9-components-with-arquillian-and-weld-f9e1add08895?source=post_page-----c4567d6c48f1----3-------------------------------)

[

![Testing Jakarta EE 9 Components With Arquillian and JBoss Weld](https://miro.medium.com/fit/c/112/112/1*XZgar-TMaqJ9r-ovXsLz_A.png)

](https://medium.com/swlh/testing-jakarta-ee-9-components-with-arquillian-and-weld-f9e1add08895?source=post_page-----c4567d6c48f1----3-------------------------------)

---

![Jodi Compton](https://miro.medium.com/fit/c/24/24/2*Nn1llm2RKtkZhPRrWlUaXg.jpeg)

[

Jodi Compton

](https://medium.com/@jodicomptoneditor?source=post_page-----c4567d6c48f1----4-------------------------------)

[

·Dec 3, 2020

](https://medium.com/swlh/how-a-history-teacher-taught-me-the-most-important-thing-about-writing-54c45273bae?source=post_page-----c4567d6c48f1----4-------------------------------)

[

## How a History Teacher Taught Me the Most Important Thing About Writing

Aka, ‘How I Wrote My First Novel, Part 1’ “How I Wrote My First Novel” is a short series about, as you’d expect, the process of writing my first crime novel and getting it published. This series is meant to help aspiring novelists, especially those who work in genre, and…



](https://medium.com/swlh/how-a-history-teacher-taught-me-the-most-important-thing-about-writing-54c45273bae?source=post_page-----c4567d6c48f1----4-------------------------------)

[

Writing

](https://medium.com/tag/writing?source=post_page-----c4567d6c48f1---------------writing--------------------)

[

7 min read

](https://medium.com/swlh/how-a-history-teacher-taught-me-the-most-important-thing-about-writing-54c45273bae?source=post_page-----c4567d6c48f1----4-------------------------------)

[

![How a History Teacher Taught Me the Most Important Thing About Writing](https://miro.medium.com/fit/c/112/112/1*vx_ELwkaYddgpyWZEHSCcg.jpeg)

](https://medium.com/swlh/how-a-history-teacher-taught-me-the-most-important-thing-about-writing-54c45273bae?source=post_page-----c4567d6c48f1----4-------------------------------)