# Auth yubikey

This Apache Module will authenticate against a yubikey server. It will use Basic Auth. Every user can have an individual
Password set, which is needed before the OTP. (2FA)

It will translate if the yubikey sends a Keymap.

## Requirements

Make sure following packages are installed to compile : libykclient-dev apache2-dev libykclient-dev libcurl

## Apache Config

```apacheconf
<VirtualHost *:443>
  ...
  <Directory "/var/www/html/secure_folder">
    # We use Basic Auth, since we need username/password
    AuthType Basic
    
    # provider is this Authenticator
    AuthBasicProvider yubikey
    
    # Same as Basic auth
    AuthName "Please Log In using your YubiKey"
    Require valid-user
    
    # timeout how long user stays logged in until new token is required
    AuthYubiKeyTimeout 3600
    
    # Where to store the Cache
    AuthYubiKeyTmpFile /etc/apache2/passwds/dnsadm.ykTmpDb
    
    # Where to read the User database from (see next chapter)
    AuthYubiKeyUserFile /etc/apache2/passwds/dnsadm.passwd
    
    # is a HTTPs Site is required for this to work (will show error and not redirect)
    AuthYubiKeyRequireSecure <On|Off>
    
    # Show External Error Page
    AuthYubiKeyExternalErrorPage <On|Off>
    
    # Server Authertication
    AuthYubiKeyServerKeyId 23
    AuthYubiKeyServerKey "<server key>"
    AuthYubiKeyValidationUrl "https://yubico.example.com/yubi-tcl/cgi-verify-2.0.tcl"
    
    
  </Directory>
  ...
</VirtualHost>
```

## KeyUserFile

the file contains either

```ini
<yubikey_id>:<sha1(username)> 
```

or if userpassword is set:

```ini
<yubikey_id>:<sha1(username:password)> 
```

