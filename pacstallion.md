Title: PacStallion - Ubuntu's AUR Given a GUI.
Date: 06/02/26

# PacStallion: The GUI for Rapscallions.

So... I made a thing. 
A GUI for Pacstall.

But not just for me, this one's for the rapscallions *and* the Linux newbies out there.
The kind of people who might actually run `:(){ :|:& };:` because they saw it on reddit.
(Or the ones who just like clicking buttons.)

---

## Why did I make PacStallion?

**Debian** is love, **Rhino Linux** is my **#1** defense mechanism against Arch glazers, and **Pacstall** is a glitch that gets you latest packages on Debian.
This project is my “thanks” letter to all three. I wanted to contribute something actually *usable* for once, not just another telegram bot or game prototype that sits on my GitHub gathering dust.

Plus:

- **For the rapscallions**: So you can flex on Arch users and tell them you installed all the latest packages with _clicks_.
- **For the newbies**: So you don’t have to ask “why does nothing happen when I double-click .deb files?” on Reddit again.

---

## Why Microsoft Java? (C# rant incoming.)

Let me start with the *cool* take:  
I don't want to use Python. I've written GUIs in Python. The performance was so horrendous I could achieve world peace before the program launched.

But do I want to write C++? Absolutely not. I still wanna keep my feet intact.
C? Sure, if I want memory leaks as a feature. But surprise, I don’t!
JavaScript? Seriously? Everyone has enough Chromium instances running on their devices hindering them.

So, **C#** it is.  
Faster than Python, less masochistic than C++, won’t hog your resources like Electron.

---

## What does it actually do?

- **Search, install, remove, update**. All with clicks. The classic pacstall features, given shape of a beautiful UI.
- **See lists of packages**. Available, installed, needs updating.

---

## Why you might care

- You mistype the package name inside the terminal, again and again, in rapid succession, each failed attempt somehow faster than the last.
- Double-clicking is your love language.
- You want to be able to say “I install stuff the easy way” just to annoy peeps who think using only `apt-get` is cool because it's "stable" without even knowing what "stable" means in that context!

---

## For fellow FOSS nerdies

Project’s fully open source ([repo here](https://github.com/IMYdev/pacstallion)), and I won’t yell at you if you send a PR.

- Got a feature idea?
- Found a bug?
- Want to port it to TempleOS?

Shoot it my way (On GitHub).

(Please don’t open an issue asking for Vim keybindings... I'm serious.)

---

## Technical Difficulties (and How I Wrangled the Stallion)

Building a GUI for a CLI-first tool like Pacstall wasn't just about slapping buttons on top of commands. I ran into a few skill issues of mine that kept me up later than I’d like to admit:

- The Terminal Hunter: Since Pacstall requires sudo for installs and removals, I had to decide: do I write a custom (and probably insecure) password prompt, or do I stay lazy? I chose the latter... But! It was harder than it looks! I had to implement a "terminal hunter" that checks for everything from gnome-terminal to kitty and alacritty just to launch the process where the user can actually see the output and type their password.

![](https://iili.io/fmVnzge.png)

- Version Comparison Hell: Pacstall’s package versioning gave me a bit of a ride. Between `-deb`, `-bin` packages and `~git` versions with hashes, a simple string comparison to check for updates was impossible, so I ended up writing a normalization engine that strips out suffixes like `-pacstall` and parses git hashes just so the "Update Available" light doesn't lie to you.
![](https://iili.io/fmVWPZQ.png)

- The Data Merge: Sometimes you may have a package installed that isn't in the repology API, for it has been removed or that you might have installed it through a custom `.pacscript`. I had to build a merging system that takes the live API data, crosses it with the output of `pacstall -L`, and handles the discrepancies without crashing even when internet is shaky.
![](https://iili.io/fmW9Qcl.png)

- Keeping it Snappy: Running CLI commands in the background is slow. To keep the UI from freezing while the app "thinks" about your package list, I had to be quite careful about Avalonia’s Dispatcher. It’s a delicate dance of fetching data in the background and updating the UI only when it’s safe.

---

PacStallion was the result of more than half a year of accumulative work...
This is not the first attempt I did at a GUI for Pacstall, the first was [PGM](https://github.com/IMYdev/PGM), it was slow, didn't provide complete functionality, and even broke Pacstall when you use it.

[![](https://iili.io/fmhuOl4.png)](https://github.com/IMYdev/pacstallion)

*PacStallion: For the rapscallions, the newbies, and everyone with terminalophobia.*

![](https://iili.io/fmhhsHP.png)
![](https://iili.io/fmhwkCb.png)
![](https://iili.io/fmhNQO7.png)