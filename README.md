# GetAbsolutePath
macOS AppleScript for Automator that gets the absolute path of a file or directory and copies it to the clipboard.

## Purpose
There are lots of times I find that I'm deep into a folder hierarchy in the Finder and come across a log file or something that I want to open in Vim in the Terminal or somesuch. Looking around for easy solutions, it turns out there's no simple way to just get the path to the item in question, whether it's a folder or file. Yes, you can do a ⌘I to get the info, but nothing in the HUD window is something that can be easily copied and pasted into a terminal, and besides, that's extra steps.

So, as a way to _finally_ make something with [Automator](https://support.apple.com/guide/automator/welcome/mac) that I would actually use, I decided to figure out how to get the absolute file path via [AppleScript](https://en.wikipedia.org/wiki/AppleScript) and copy the path to the clipboard that I can then use elsewhere.

## Installation
Simply place **Get Absolute Path of Item.workflow** in the `~/Library/Services` directory. 

## Usage
Right-click on the file or folder in question, then select "Services", then choose "Get Absolute Path of Item". The full path to the file or folder will now be on the clipboard.

![Services Menu Image](https://github.com/tachoknight/GetAbsolutePath/blob/master/services-menu.png) 