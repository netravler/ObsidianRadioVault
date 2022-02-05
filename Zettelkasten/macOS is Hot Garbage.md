[![Sergio](https://miro.medium.com/fit/c/96/96/0*bXh5MBg4QgCzgNUx)](https://medium.com/@sergiointoronto?source=post_page-----6f3909835b0f-----------------------------------)

[Sergio](https://medium.com/@sergiointoronto?source=post_page-----6f3909835b0f-----------------------------------)

# macOS is Hot Garbage

It’s 2022. Apple has been releasing graphical operating systems since 1984 and in 38 years they haven’t learned enough to make a great OS. In this post I’ll show you everything misleading, awkward, or just plain broken about macOS. OK, probably not everything.

# Finder is Dogshit

“Finder” is macOS’s file manager. Let’s take a look.

## **Files don’t Auto-Arrange**

Drag-and-drop files into an empty folder makes them scroll off to the right

By default, file icons will preserve their arrangement, which is never what I want. I’ve learned to two-finger-tap (aka _right-click_) and press Clean Up. This arranges the files as I expect, usually. Sometimes it doesn’t (see video) and I’m not sure why.

## **Informational Redundancy**

I can’t explain this one.

![](https://miro.medium.com/max/1400/1*YT_A3adukXXku33P2htLUw.jpeg)

## **Finder Lacks Cut-and-Paste**

![](https://miro.medium.com/max/1400/1*aupqQ11cI7RAd-yssyZRSA.jpeg)

On most computers, files can be cut, copied, and pasted just like text. In Finder, **cut** is disabled.

![](https://miro.medium.com/max/1400/1*arfnpomR9bYZu6N_dApNbg.jpeg)

Technically **there** **is a way** to cut-and-paste, but it’s not what you‘d expect. It’s not _cut and paste._ macOS has _copy_ and _disregard-copy-actually-move-instead_. Holding down the _option_ key reveals this extra menu item.

This boggled my mind when I learned it. Imagine if text worked this way.

## **When Finder is Closed your Desktop Icons Disappear**

Finder has two responsibilities: showing files/folders and showing desktop icons. Many DWMs work this way — including _explorer.exe_ on most versions of Windows — but unlike other DWMs Finder can be killed completely, taking your desktop icons with it.

Pressing quit on Finder hides the desktop icons. Run Finder to see them again.

Personally I don’t find this much of a hassle, but it leads to another edge-case. What if you want a Finder window? For that, **open Finder twice**_._

Launch Finder twice for the Finder window appears.

This video shows macOS Catalina, but the issue wasn’t fixed in **Big Sur** or **Monterey**. I doubt they’ll ever fix it.

## **Search-as-you-Type Breaks Typing Conventions**

To search I type some characters and then press _enter_. As a habit, _e_nter _o_r _tab_ follows when I type something. But enter doesn’t open a file in macOS, it renames it. Yes, really! Pressing enter edits the file name. I’ll wait while non-macOS readers compose themselves.

The only way to open a file with your keyboard is to move your hand to the arrow keys and press **_command+down_**. Finder’s hotkeys are just awkward.

# The Window Manager is Buggy and Unpredictable

What is a Desktop Window Manager (DWM)? It does the little things around your apps: giving you a bar to click-and-drag, letting you resize at the edges, and handling the menus at the top. MacOS’s DWM — called Quartz Compositor — can be changed completely with apps like [Amethyst](https://github.com/ianyh/Amethyst#amethyst), but most people use the default. But it’s awkward.

## **“Zoom” (aka Maximize) is Inconsistent**

Safari and Google Chrome behave differently when the title bar is double-clicked.

Windows and Ubuntu call it “maximize” while macOS calls it “zoom”. This action behaves differently depending on the app. Safari, Finder, Preview, and other apps exhibit this unexpected behaviour. Most built-in apps do this, but not all of them. For example the Notes app fills the whole screen like I expect. Why does this happen? I couldn’t find an answer that made any sense.

## **Apps Remain Running (and Focused) After Being Closed**

Google Chrome remains running after all windows are closed, just like all macOS apps.

Closing all windows of an app doesn’t exit the app. It’s still running and it still has focus. Anything you type goes to the invisible app which has [confused people for almost 10 years](https://apple.stackexchange.com/questions/51030/how-do-i-give-focus-to-the-correct-window-when-closing-another-applications-win). Using the Switcher (_command+tab_) shows the app even though it has no windows. It’s easy to accidentally have 10+ apps running despite seeing only 1 or 2. For a company making “intuitive” software this is anything but.

## **Splitting Apps is Unnecessarily Annoying**

At first I thought this feature didn’t exist. I eventually found it. Here’s how it works.

![](https://miro.medium.com/max/1400/1*1nEn3pSnwfzgVKKT2yu8MQ.jpeg)

You hover over the tiny green button and a menu appears. It’s the smallest User Interface (UI) element anywhere in macOS.

![](https://miro.medium.com/max/1400/1*07lUsH0hAA2fG7SYG2iXrA.png)

Keep in mind this is really split-full screen. The window moves to a new desktop and if you don’t have a second window **the** **split will cancel**. If you want snapping or hotkeys similar to Windows or Ubuntu you need a third-party app like [Magnet](https://apps.apple.com/us/app/magnet/id441258766) or [Spectacle](https://www.spectacleapp.com/). Annoying to say the least.

![](https://miro.medium.com/max/1400/1*wZ8lQpJXBLC4S87BpQoAtQ.png)

**Update:** It is possible! Holding _option_ lets you resize a window to half your screen similar to Windows and Ubuntu. It’s annoying but at least it’s possible.

## **Window Management Hotkeys**

Observant readers may have noticed “Zoom” had a hotkey in that screenshot. That’s not default. macOS lets you add hotkeys for any action and I’ve added one for zoom. You can do this via System Preferences → Keyboard → Shortcuts → clicking the “+” icon.

![](https://miro.medium.com/max/1356/1*YJgTqwX_zfAkhJjcyxtEZw.jpeg)

However don’t do this because it doesn’t work. Well, it…often doesn’t work. This DWM action is app-dependant. Firefox, Google Chrome, and many of the productivity apps I use everyday don’t even twitch when I press the hotkey. Nothing happens. But apps like Spotify, Slack, and built-in apps like Notes _do_ work. What’s the difference? I’m not sure, but it’s the first time I’ve ever seen a hotkey that **only works sometimes**.

**Clicking Takes Focus, but Not Right-Clicking**

Clicking another window makes it take focus. Right-clicking does not take focus

I’m not sure why Apple decided this was an important feature, but I’ve highlighted the only use-case I could think of. Is this handy or just confusing?

Another feature, if you right-click then left-click quickly, it counts as a **double-click**. weird.

## Restoring a Minimized Window is Insane

Pressing _command+M_ minimizes a window. I often hit it by accident when going for _command+N_. You need the mouse to get it back. Either select Window -> the window title, or _right-click_ (_control_-click) the dock icon and select the window.

But as I recently discovered **there is a way** to recover it using the keyboard. Hit command+tab to open the switcher, highlight the minimized application, and slide your thumb onto _option_ key before releasing _command_. The minimized application pops up!

This is bananas. Why not _always_ bring up the application instead of doing _nothing_? How are people supposed to discover this? It’s like a secret hotkey only known by Apple gurus. Oh, and it doesn’t work if the application has any windows that _aren’t minimized_. Thanks Apple.

# Missing Features and Awful Bugs

## **The Disappearing Mouse Issue**

I thought it was hyperbole until I experienced it. My cursor just vanished without warning. It still “worked” — I could blindly click things. But I had no cursor! What would break such a fundamental feature of a modern OS?

I don’t know, but I wasn’t alone. The top search result gave me [10 things to try if your mouse “keeps disappearing.](https://www.switchingtomac.com/tutorials/osx/mouse-keeps-disappearing-on-mac-10-things-to-try/)” I guess this happens a lot. The last suggestion blew me away, **“Learn To Use Your Mac Without a Mouse.”** Here is a better suggestion. Learn to use another OS.

## **Missing Hotkeys**

On the topic of life without a mouse, let me speak to the keyboard-centric folks out there. macOS isn’t for you.

Many common tasks lack hotkeys such as splitting windows and “zoom” (both discussed above), launching applications that appear on the dock, and moving windows between desktops are more common examples that lack hotkeys. These actions require extra software or are [straight-up impossible](https://superuser.com/questions/184763/is-there-a-way-to-move-the-current-window-to-another-desktop-without-using-a-mou).

![](https://miro.medium.com/max/1400/1*haAIowcua91U_QmcrFqkIA.jpeg)

Apple’s iPhone keyboard was arguably the most well-thought-out mobile keyboard on earth. Why then is macOS’s keyboard so poorly designed? For example, _command+W_ closes a window or browser tab while command+Q closes the whole application. Because they’re right next to each other it’s easy to accidentally hit the wrong one. Logging out is command+control+Q — just 1 extra modifier. But we’re human and mistakes happen.

Sometimes I miss a modifier and accidentally quit my browser instead of locking my computer. It’s frustrating. I’m not the only one - my coworkers admit to flubbing macOS hotkeys just as often as I do. By contrast, the Windows and Ubuntu hotkey (Super/“Windows key”+L) isn’t near any other hotkeys. Mistakes are no big deal.

## **Missing Keyboard Keys**

A standard keyboard includes a key which brings up the context menu. It’s equivalent to right-clicking.

![](https://miro.medium.com/max/1400/1*GsiK_YL4hC8br757NgbhyA.png)

![](https://miro.medium.com/max/1400/1*TmpYPatP_3oAzWvv89goAA.png)

Once I got used to fixing typos this way I could do it without thinking. It didn’t break my train of thought. Until I used macOS. Apple keyboards don’t include this key and it’s **impossible** to rebind another key for this purpose. The operating system simply [does not support this keyboard key](https://apple.stackexchange.com/questions/32715/how-do-i-open-the-context-menu-from-a-mac-keyboard#answers).

That’s not the only missing hotkey, though luckily the others have workarounds. Actions like Printscreen (use _command+shift+3_ instead), both backspace & delete (use _fn+delete_ instead), home/end (use _fn+left/right_ instead), any function keys (hold _fn+_use the touchbar instead), or a power/sleep button are all missing from my Macbook keyboard.

## **The Power Button Does Nothing**

The power button doesn’t put my Macbook Pro 2019 to sleep. And that’s on purpose — Apple’s [documentation](https://support.apple.com/en-us/HT201236#sleep) says the “Touch ID Sensor” (which is a button) does nothing. Oh, except clicking it 5 times opens macOS’s accessibility options. Clicking it once has no effect.

There is a way to have a sleep button. You can [add a sleep button to the touchbar](https://ioshacker.com/how-to/how-to-add-a-quick-sleep-button-to-macbooks-touch-bar). But you can’t make the “Touch ID Sensor” do anything when clicked, at least in macOS **Catalina**. After updating to **Monterey** the button now locks the macbook. _command+control+Q_ already does this, but hey it’s better than a useless button.

## **Can’t Disable the Built-in Display**

![](https://miro.medium.com/max/1400/1*j4lpOQ-P1_Ceo-144Ldrcw.png)

If you want to do this you’re out of luck. You can’t disable the screen while keeping the Macbook open. This is easy on Windows and Ubuntu but impossible on macOS. [The top suggestion](https://apple.stackexchange.com/questions/20591/how-do-i-use-only-the-external-display-with-my-macbook-pro-lid-open-on-lion?answertab=votes#tab-top) was a laughable workaround: Arrange the displays such that they didn’t overlap and reduce the Macbook brightness to zero.

![](https://miro.medium.com/max/1400/1*lx_RjgqR4rsBfZZT6IPVlQ.png)

macOS still can’t handle this despite the problem being 10 years old. Apple just isn’t interested in fixing some problems.

## **macOS Will Drain your Smartphone’s Battery**

Admittedly this one was my fault. Here’s what happened:

-   My partner unplugged my Macbook to charge a Nintendo Switch.
-   My phone and external display were connected, but the Macbook had been closed for hours. I assumed it was asleep.
-   It wasn’t asleep. It stayed awake and used my phone as a power **source**. In the morning they were both completely dead.

Why did this happen?

-   Even at 100% battery and _while already charging_, my Macbook defaults to stealing power from my phone - I always have to [toggle the setting](https://i.imgur.com/rFFVMke.png). With the same phone and cable other laptops charge my phone.
-   Having an external display attached prevents macOS from sleeping, even when idle or closed.

If I was on-call or had to leave urgently this would have been a catastrophe. Maybe this issue is why Apple avoids USB-C on their phones?

## Sometimes Things just Break

Today the preferences screen didn’t work at all. It was fine after a reboot. I thought macOS didn’t have problems like this, but it does.

I waited, watching “the spinning beach ball of death”. It happens when macOS is busy and freezes your application or system. You can’t see it in the video because macOS cleverly hides it in screen captures. It was awkward.

# Conclusion

Maybe macOS had a hey day. Maybe many years ago when the demands were lesser and the CEO had a vision and things were good. Now in 2022 macOS is an embarrassment with loads of edge-cases and flaws. Is the grass greener elsewhere or should we accept macOS as it is?

