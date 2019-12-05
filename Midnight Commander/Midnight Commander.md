## What is Midnight Commander?
GNU Midnight Commander is a feature rich full-screen text mode application that allows you to copy, move and delete files and whole directory trees, search for files and run commands in the included subshell.

## Hints
* **mouse support can be enabled** if the gpm package is installed
* **text blocks are symbolized by** the symbol "*" for an entire block and "?" for using character precision
* **it can also do case conversion** (lowercase to uppercase and the other way around)
* it **can treat some archive types and remote hosts as if they were local filesystems**


## Commands
* **`F9`** for **going to the top menu**
* **`Esc+1`** for **displaying the help menu**
* **`Ctrl+x i`** for **displaying detailed information about a file**
* **`Ctrl+\`** for **displaying the file hotlist**
* **`Ctrl+x h`** for **adding a file to the hotlist**
* **`F3`** for **viewing a file with the text editor** or **closing the file opened with the text editor**
* **`Ctrl+x q`** for **displaying the contents of the file in the other half of the screen** (need to press Esc or Alt-i when in the editing/viewing tab to exit editing mode)
* **`Insert`** for **tagging/untagging files**
* **`+`** for **using pattern detection with the files**
* **`F7`** for **creating a new directory**
* **`Alt+o`** for **listing the selected directory in the other panel**
* **`Alt+i`** for **displaying the same content in the other panel as in the selected panel**
* **`Alt Shift+h`** for **displaying the list of directories that has already been previosly displayed in the selected panel**
* **`F4`** for **starting the text editor with the selected file in it**
* **`Ctrl and +`** or **`Ctrl and -`** for **waving through the used selection criteria**
* **`F5`** for **copying a file/directory**

* To copy a file and manipulate the name of that file by using text blocks, we symbolize the text blocks in the field source mask, as they are symbolized in the origin directory, then we  mention in the "to: " field the new order of the text blocks, or their position

* To manipulate the text blocks with character precision, the text blocks are represented with the **`"?"`** symbol

* Options for character manipulation during copying operations:
    1. **`\u`** for **uppercase conversion**
    1. **`\U`** for **converting the entire block sequence(a word) to uppercase**
    1. **`\I`** for **lowercase conversion**
    1. **`\L`** for **converting the entire block sequence to lowercase**

* To create links, we can use the following commands:
    1. **`Ctrl-x`** l for **creating a hard link in the current panel**
    1. **`Ctrl-x`** s for **creating a symbolic link in the current panel**
    1. **`Ctrl-x`** v for **creating a symbolic link for the selected file in the directory viewed in the other panel**

* To **change file ownership**, we can use the shortcut **`Ctrl-x o`**, and toggle the options with Space.

* To **use chmod on a file**, we can use **`Ctrl-x c`** ( needs to run with sudo in order to change priviledges )

* In order to access an ftp location and treat it as a local file system, the following command must be used: cd ftp_address ( this will display the remote location in the selected panel as if it were a local filesystem )-it uses FISH or SFTP

* To **search for a directory**, use the command **`Alt-?`**

* **`F2`** for **displaying the user menu**, which is, as the name suggests, a list of commands defined by the user. In the command tab of the upper menu, there is the option for modifying the file with commands for the user menu
