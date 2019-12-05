## What is Visual Studio Code?
Visual Studio Code is a code editor redefined and optimized for building and debugging modern web and cloud applications.

#

## These shortcuts will work in Visual Studio Code files with the Sublime Text Keybindings installed.

#

## What I've learned so far:

**`F1`** wil bring up the command control case

The search bar in the addons popup has its own query language, so if an **`@`** sign is placed at the beginning of a command, **it will start displaying possible matches to the query**.

Pressing **`Ctrl`** + **`/`** will automatically comment the line where the cursor is, regardless of the set language.

**`Ctrl`** + **`b`** will toggle the addon bar.

**`Ctrl`** + **`,`** will bring up the settings menu

#

## Using the settings

<pre>
"editor.cursorSmoothCaretAnimation": true
</pre>

Will make the cursor navigation smooth and nice to watch.

#

<pre>
"editor.renderWhitespace": "all"
</pre>

Will mark all the whitespaces in the code file.

#

<pre>
"editor.minimap.enabled": false
</pre>

Will disable the minimap.

#

<pre>
"editor.copyWithSyntaxHighlighting": true
</pre>

Will copy the text with formatting included and also color highlight.

#

<pre>
"files.autoSave": "onFocusChange",
"files.autoSaveDelay": 1000
</pre>

Toggles autosave and will toggle the autosave on and trigger it each time when i change the focus of the cursor.

#

Holding down the **`Alt`** or **`Shift`** key when splitting the workspace will **toggle between vertical and horizontal splitting**.

#

## Search terms and commands in the command bar

**`*.js`** will search for all the files with the **.js** extension

**`server`** will search in the folder structure of the project directly

**`?`** will display all the available things to be done from the command pallet

**`@`** will display an overview of the entire file (methods, variables etc.)

**`@:`** will display an overview of the entire file, with the elements grouped in methods, variables etc.

**`"Toggle breadcrumbs"`** will display the current file path at the top of the file, in the editor UI.

**`Ctrl`** + **`Shift`** + **`:`** will allow access to the breadcrumbs path for the current file

**`Shift`** + **`Ctrl`** + **`L`** will set a cursor on every instance of a found word in a file

**When creating a new file, the new file’s path can be specified in its name.**

**`terminal: rename`** in the command pallette will allow us to rename the currently selected terminal.

#

## Language specific settings
Visual Studio Code has support for language specific settings. 

This makes the editor highly customizable, and these settings can be easily moved to another computer with a thumb drive, or cloud sync.

### **Markdown**

<pre>
"[markdown]":{
	"editor.enableminimap":true }
</pre>

#

## Shortcuts

**`Ctrl`** + **`w`** for editing the word inside which the cursor is.

**`Ctrl`** + **`F4`** for closing the current document.

**`Ctrl`** + **`k`**, **`Ctrl`** + **`f`** for “beautifying” code.

**`Ctrl`** + **`r`**, **`Ctrl`** + **`g`** for removing unused references.

**`Alt`** + **`Shift`** + **`w`** for wrapping front-end code in div tags.

**`Ctrl`** + **`Alt`** + **`Enter`** for refreshing the page that you’re working on.


#

## Compound debugging and tasks in Visual Studio Code

