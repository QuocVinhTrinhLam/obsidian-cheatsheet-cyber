## Notetaking Tools

| [CherryTree](https://www.giuspen.com/cherrytree/)     | [Visual Studio Code](https://code.visualstudio.com/) | [Evernote](https://evernote.com/)            |
| ----------------------------------------------------- | ---------------------------------------------------- | -------------------------------------------- |
| [Notion](https://www.notion.so/)                      | [GitBook](https://www.gitbook.com/)                  | [Sublime Text](https://www.sublimetext.com/) |
| [Notepad++](https://notepad-plus-plus.org/downloads/) | [OneNote](https://www.onenote.com/?public=1)         | [Outline](https://www.getoutline.com/)       |
| [Obsidian](https://obsidian.md/)                      | [Cryptpad](https://cryptpad.fr/)                     | [Standard Notes](https://standardnotes.com/) |
## Logging
#### Exploitation Attempts

First, clone the [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm) repo to our home directory (in our case `/home/htb-student` or just `~`).

```shell
3kjS@htb[/htb]$ git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

Next, create a `.tmux.conf` file in the home directory.

```shell
3kjS@htb[/htb]$ touch .tmux.conf
```

The config file should have the following contents:

```shell
3kjS@htb[/htb]$ cat .tmux.conf 

# List of plugins

set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-logging'

# Initialize TMUX plugin manager (keep at bottom)
run '~/.tmux/plugins/tpm/tpm'
```

After creating this config file, we need to execute it in our current session, so the settings in the `.tmux.conf` file take effect. We can do this with the [source](https://www.geeksforgeeks.org/source-command-in-linux-with-examples/) command.

```shell
3kjS@htb[/htb]$ tmux source ~/.tmux.conf
```
#### Tmux.conf

```shell
set -g history-limit 50000
```
## Formatting and Redaction
#### Blurring Password Data

![](Notetaking%20&%20Organization-20260828-151635.png)
#### Blanking Out Password with Solid Shape

![](Notetaking%20&%20Organization-20260828-151646.png)
