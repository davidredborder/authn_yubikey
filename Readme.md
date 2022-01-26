Auth yubikey
------------

this apache module will authenticate agains a yubikey server. It will check the correct keyboard layout, if the yubikey is setup correct


the passwd file contains:

<yubikey_id>:<sha1(username)> 

or if userpassword is set: 
<yubikey_id>:<sha1(username:password)> 


-- bugs --
if the yubikey sends the keymap, but we use us/de keymap, the magic for detecting the keymap does not work properly. therefore the key used in those environments does not need to send the keymap, and the yubikey server needs to also have the with itself translated key
