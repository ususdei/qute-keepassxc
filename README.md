

# Introduction

This is a [qutebrowser][2] [userscript][5] to fill website credentials from a [KeepassXC][1] password database.


# Installation

First, you need to enable [KeepassXC-Browser][6] extensions in your KeepassXC config.


Second, you must make sure to have a working private-public-key-pair in your [GPG keyring][3].


Then, simply check out this repository and, if you do not want to configure an explicit path, symlink the
`qute-keepassxc` to your `~/.local/share/qutebrowser/userscripts/qute-keepassxc`.
Install the python module `pynacl`.
Make sure `qute-keepassxc` is executable.


Finally, adapt your qutebrowser config.
You can e.g. add the following lines to your `~/.config/qutebrowser/config.py`
Remember to replace `ABC1234` with your actual GPG key.

```python
config.bind('<Alt-Shift-u>', 'spawn --userscript qute-keepassxc --key ABC1234', mode='insert')
config.bind('pw', 'spawn --userscript qute-keepassxc --key ABC1234', mode='normal')
```

If you did not symlink `qute-keepassxc` you need to provide the full path here.

N.B. To manage multiple accounts you need the [rofi](https://github.com/davatorium/rofi) program.


# Usage

If you are on a webpage with a login form, simply activate one of the configured key-bindings.

The first time you run this script, KeepassXC will ask you for authentication like with any other browser extension.
Just provide a name of your choice and accept the request if nothing looks fishy.


# How it works

This script will talk to KeepassXC using the native [KeepassXC-Browser protocol][4].


This script needs to store the key used to associate with your KeepassXC instance somewhere.
Unlike most browser extensions which only use plain local storage, this one attempts to do so in a safe way
by storing the key in encrypted form using GPG.
Therefore you need to have a public-key-pair readily set up.

GPG might then ask for your private-key password whenever you query the database for login credentials.


# TOTP

This script recently received experimental TOTP support.
To use it, you need to have working TOTP authentication within KeepassXC.
Then call `qute-keepassxc` with the `--totp` flags.

For example, I have the following line in my `config.py`:

```python
config.bind('pt', 'spawn --userscript qute-keepassxc --key ABC1234 --totp', mode='normal')
```

For now this script will simply insert the TOTP-token into the currently selected
input field, since I have not yet found a reliable way to identify the correct field
within all existing login forms.
Thus you need to manually select the TOTP input field, press escape to leave input
mode and then enter `pt` to fill in the token (or configure another key-binding for
insert mode if you prefer that).

# Autotype

This script theoretically supports requesting a Keepass Autotype sequence for the current URL.
Unfortunately this does not seem to work under wayland yet so it is still completely untested.

Example config:

```python
config.bind('<Alt-Shift-a>', 'spawn --userscript qute-keepassxc --key ABC1234 --autotype', mode='insert')
```

# Compatiblity

Tested with:

 - KeepassXC 2.7.9
 - qutebrowser v3.3.1
 - python 3.12.7


[1]: https://keepassxc.org/
[2]: https://qutebrowser.org/
[3]: https://gnupg.org/
[4]: https://github.com/keepassxreboot/keepassxc-browser/blob/develop/keepassxc-protocol.md
[5]: https://github.com/qutebrowser/qutebrowser/blob/master/doc/userscripts.asciidoc
[6]: https://keepassxc.org/docs/KeePassXC_GettingStarted.html#_configure_keepassxc_browser


# License

Copyright (c) 2018-2024, Markus Blöchl. Released under the MIT License.

