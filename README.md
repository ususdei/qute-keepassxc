

Introduction
===========

This is a [qutebrowser][2] [userscript][5] to fill website credentials from a [KeepassXC][1] password database.


Installation
============

First, you need to enable [KeepassXC-Browser][6] extensions in your KeepassXC config.


Second, you must make sure to have a working private-public-key-pair in your [GPG keyring][3].


Then, simply check out this repository and, if you do not want to configure an explicit path, symlink the
`qute-keepassxc` to your `~/.local/share/qutebrowser/userscripts/qute-keepassxc`.
Install the python modules `simplejson` and `pynacl`.
Make sure `qute-keepassxc` is executable.


Finally, adapt your qutebrowser config.
You can e.g. add the following lines to your `~/.config/qutebrowser/config.py`
Remember to replace `ABC1234` with your actual GPG key.

```python
config.bind('<Alt-Shift-u>', 'spawn --userscript qute-keepassxc --key ABC1234', mode='insert')
config.bind('pw', 'spawn --userscript qute-keepassxc --key ABC1234', mode='normal')
```

If you did not symlink `qute-keepassxc` you need to provide the full path here.


Usage
=====

If you are on a webpage with a login form, simply activate one of the configured key-bindings.

The first time you run this script, KeepassXC will ask you for authentication like with any other browser extension.
Just provide a name of your choice and accept the request if nothing looks fishy.


How it works
============

This script will talk to KeepassXC using the native [KeepassXC-Browser protocol][4].


This script needs to store the key used to associate with your KeepassXC instance somewhere.
Unlike most browser extensions which only use plain local storage, this one attempts to do so in a safe way
by storing the key in encrypted form using GPG.
Therefore you need to have a public-key-pair readily set up.

GPG might then ask for your private-key passwort whenever you query the database for login credentials.


Compatiblity
============

Tested with:

 - KeepassXC 2.3.4
 - qutebrowser v1.5.2
 - python 3.7.1


Links
=====

 - https://keepassxc.org
 - https://qutebrowser.org
 - https://gnupg.org
 - https://github.com/keepassxreboot/keepassxc-browser/blob/develop/keepassxc-protocol.md
 - https://github.com/qutebrowser/qutebrowser/blob/master/doc/userscripts.asciidoc
 - https://keepassxc.org/docs/keepassxc-browser-migration

[1]: https://keepassxc.org/
[2]: https://qutebrowser.org/
[3]: https://gnupg.org/
[4]: https://github.com/keepassxreboot/keepassxc-browser/blob/develop/keepassxc-protocol.md
[5]: https://github.com/qutebrowser/qutebrowser/blob/master/doc/userscripts.asciidoc
[6]: https://keepassxc.org/docs/keepassxc-browser-migration/


License
=======

Copyright (c) 2018, Markus Blöchl. Released under the MIT License.

