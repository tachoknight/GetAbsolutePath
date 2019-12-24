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

## How It Works
The workflow is two steps:

1. Workflow receives current files or folders in Finder
2. Run AppleScript

### The AppleScript
This is the script in total:

```
on replace_chars(this_text, search_string, replacement_string)
	set AppleScript's text item delimiters to the search_string
 	set the item_list to every text item of this_text
 	set AppleScript's text item delimiters to the replacement_string
 	set this_text to the item_list as string
 	set AppleScript's text item delimiters to ""
 	return this_text
end replace_chars

on trim_up_to(this_text, trim_chars)
	set x to the length of the trim_chars
	repeat until this_text begins with the trim_chars
		try
			set this_text to characters (x + 1) thru -1 of this_text as string
		on error
			return ""
		end try
	end repeat
	
	return this_text
end trim_up_to

on run {input, parameters}
	# input is already an alias
	set theFile to input as string
	#display dialog theFile
	set the changedFile to replace_chars(theFile, ":", "/")
  set the changedFile to replace_chars(changedFile, " ", "\\ ")
	set the fixedFile to trim_up_to(changedFile, "/")
	set the clipboard to fixedFile
	return input
end run
```

The workflow calls `on run {input, parameters}`. The thing I didn't realize is that `input` is an _alias_, which is likely documented somewhere but I just didn't find it. From there the alias is converted to a string, which is then washed through two functions, `replace_chars`, which changes the path delimeter from ":" to "/" (because, I'm guessing, AppleScript's origin on Classic MacOS where the ":" was used to separate folders), and the result of that is passed to `trim_up_to` which is described below.

### `trim_up_to`
This function was necessary because the _real_ absolute path includes the name of the machine, which is utterly unnecessary when trying to open a file in Vim. 
This function will remove all the characters up to the first slash, so
`My Mac/Users/me/blah blah` becomes `/Users/me/blah blah`. 
