
# scli
`scli` is a simple terminal user interface for [Signal](https://signal.org). It uses [signald](https://gitlab.com/thefinn93/signald/) and [urwid](http://urwid.org/).

# Installation
## Dependencies
### signald
Firstly, you need to install [signald](https://gitlab.com/thefinn93/signald/).
You can follow the guide provided in the README, but if you are using `ubuntu`/`debian`, it's basically the following:

Add the following to your `sources.list`:
```
deb https://updates.signald.org master main
```
And trust the signing key:
```
curl https://updates.signald.org/apt-signing-key.asc | sudo apt-key add -
```
Now you can install signald:
```
sudo apt install signald
```
### urwid
You can install it trough your distributions package manager.
On `ubuntu` / `debian`:
```
sudo apt install python3-urwid
```
From `pip` to install: 
```
pip3 install urwid
```
Optionally Install `urwid_readline`.
Unfortunately it's not available as package in `ubuntu`/`debian`.
So install it from pip:
```
pip3 install urwid_readline
```

# First start
## Linking your device
`scli` does not provide anything for registering/linking, you need to do this using `signald`.

See here: [Linking signald with your phone](https://gitlab.com/thefinn93/signald/-/blob/master/docs/linking-qrencode-howto.md)

## Launching
Now you can start using:
```
scli -u PHONE_NUMBER
```

**Note**: `PHONE_NUMBER` starts with `+` followed by the country code.

# Usage
A simple two-paned interface is provided.
Left pane contains the contact list and the right pane contains the conversation.

You can switch focus between panes by hitting `Tab` (or `Shift + Tab`).
Hitting tab for the first time focuses the conversation, hitting it again focuses to input line.
So the tab order is `Contacts -> Conversation -> Input`, you can use `Shift + Tab` for cycling backwards.

## Navigation
- Use `down` or `j`/`up` or `k` to go down/up in contacts list or in messages.
- Hitting `enter` on a contact starts conversation and focuses to input line.
- Hitting `l` on a contact only starts conversation.
- Hitting `o` on a message opens the URL if there is one, if not it opens the attachment if there is one.
- Hitting `enter` on a message opens the attachment if there is one, if not it opens the URL if there is one.
- Hitting `y` on a message puts it into system clipboard. (needs `xclip`)
- `g` focuses first contact/message.
- `G` focuses last contact/message.
- `d` deletes the message from your screen (and from your history, if history is enabled).
- `i` show a popup that contains detailed information about the message.

## Commands
There are some basic commands that you can use. Hit `:` to enter command mode (like in `vim`)

- `:quit` or `:q` simply quits the program.
- `:openUrl` or `:u` opens last URL in messages, if there is one.
- `:openAttach` or `:o` opens last attachment in messages, if there is one.
- `:attach FILE_PATH` or `:a FILE_PATH` attaches given file to message.
- `:attachClip` or `:c` attaches clipboard content to message. This command tries to detect clipboard content. If clipboard contains something with the mime-type `image/png` or `image/jpg`, simply attaches the image to message. If clipboard contains `text/uri-list` it attaches all the files in that URI list to your message. This command needs `xclip` installed.
- `:toggleNotifications` or `:n` toggles desktop notifications.
- `:edit` or `:e` lets you edit your message in your `$EDITOR`.
- `:toggleAutohide` or `:h` toggles autohide property of the contacts pane.
- `:sync` or `:s` trigger a Contacts synchronisation from your phone. This should not be needed as `signald` is supposed to do that for you.

Examples:
```
:attach ~/cute_dog.png check out this cute dog!
:attachclip here is another picture.
```
**Note**: Commands are case insensitive, `:quit` and `:qUiT` does the same thing.

## Searching
There is a built-in search feature.
Simply hit `/` while you are on the chat window (or focus the input line then type `/`) and start typing, the chat will be filtered out based on your input. You can focus any of the search results and hit `enter` (or `l`) to open that result in full conversation.

For searching through contacts, you need to hit `/` while you are on the contacts window and start typing, contacts will be filtered out while you are typing. Hit `enter` to focus the results. Hitting `Esc` will clear the search.

## Configuration
There are some simple configuration options.
### From command line
You can pass configuration as command line arguments.
See
```
scli --help
```
to see options.

### From configuration file
Default configuration file is `~/.config/scli.conf`. You can start scli with another configuration file by using the `--config` or `-c` command line option.

Configuration file syntax is pretty easy:
 - A configuration line consist in a `key=value` pairs
 - Keys are the same as the one from command line without the `--` (only long options are valid. See example below)
 - Lines starting with `#` and empty lines are ignored


### Example
```sh
scli -u +3312345678 --enable-notifications=false
```
Configuration file equivalent of this command is:
```ini
# Long option forms are used in config file. (u=+123... is not valid.)
username=+3312345678
enable-notifications=false
```

# Screenshots
![scli](screenshots/1.png?raw=true)
![scli](screenshots/2.png?raw=true)
![scli](screenshots/3.png?raw=true)

