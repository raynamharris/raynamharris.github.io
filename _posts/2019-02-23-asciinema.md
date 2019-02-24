---
layout: post
title:  "asciinema for recording command-line terminal session"
date:   2019-02-23
excerpt: "Sharing screen casts of live coding is way more awesome when the text can be copies. asciinema makes this possible."
image: "/images/asciinema.png"
---

Today I attended [IndieWebCamp Austin 2019](https://indieweb.org/2019/Austin). IndieWebCamps are two days events for web creators to meet up to share ideas, improve their personal websites, and build upon each other's creations. After a morning keynote, all the attendees (there were about 20 of us) gave a short demo of some of our web creations. 

[asciinema](https://asciinema.org/) was something that struck me as immediately useful and something that I would like to use. It is a lightweight, text-based approach to terminal recording. Because it's text-based, people watching the screencast can *copy the text from the terminal!* I had to try it out. 

[Getting started](https://asciinema.org/docs/getting-started) was pretty straightforward. I used `brew install asciinema` to install it. Unfortunately, the record comment, `asciinema rec`, didn't work immediately; however, there was a [closed GitHub issue](https://github.com/asciinema/asciinema/issues/260) with a solution! I ran `brew postinstall python3` and it worked! I used `asciinema rec` to start a screen recording. I chose to follow a few of the commands in the Software Carpentry Unix Shell Lesson on navigating files and directories, which only goes into `pwd`, `ls`, and `man`. Once I typed `exit` to end the recording, instructions for how to post to the web were printed directly on my screen. Then I used `asciinema auth` to authentic myself as a user so that my videos could be stored on the web. 

Check it out!

[![asciicast](https://asciinema.org/a/3uHCusbqQd6KAYNZ15NSv21bQ.svg)](https://asciinema.org/a/3uHCusbqQd6KAYNZ15NSv21bQ)