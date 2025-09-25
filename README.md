# elogind-slackware
For testing: modified official rc.elogind and elogind.SlackBuild. <br>
Now Slackware-Current  can support new elogind functions: varlinkctl, userdbctl etc<br>
Keep in mind that all these new functions are not released yet we build from master<br>
New functions appears in this commit: https://github.com/elogind/elogind/commit/75c45d63c8a08a1512c9145e38c040e14900378b <br>


We all have to say elogind devs <br>
## THANK_YOU

This improvments will help a lot Gnome builds and future Plasma builds in no-systemd disrtos.

## examples

```
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 2:52:12pm)-(CPU 1.7%:Net 5)-(omen:~)-(12G:206)
└─ >> userdbctl user omen
   User name: omen
 Disposition: regular
 Last Passw.: Mon 2024-11-11 02:00:00 EET
    Login OK: yes
 Password OK: yes
         UID: 11000
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 2:52:24pm)-(CPU 1.7%:Net 5)-(omen:~)-(12G:206)
└─ >> userdbctl user testuser
   User name: testuser
 Disposition: regular
 Last Passw.: Wed 2025-09-24 03:00:00 EEST
    Login OK: yes
         UID: 11003
         GID: 11003 (testuser)
   Directory: /home/testuser
     Storage: classic
       Shell: /bin/bash
 Passwd Chg.: max 273y 9month 1w 4d 19h 30min/warn 1w
Pas. Ch. Now: no
   Passwords: none
     Service: io.systemd.NameServiceSwitch
 Self Modify: realName
              emailAddress
              iconName
              location
              shell
              umask
              environment
              timeZone
              preferredLanguage
              additionalLanguages
              preferredSessionLauncher
              preferredSessionType
              pkcs11TokenUri
              fido2HmacCredential
              recoveryKeyType
              lastChangeUSec
              lastPasswordChangeUSec
      (Blobs) avatar
              login-background
 (Privileged) passwordHint
              hashedPassword
              pkcs11EncryptedKey
              fido2HmacSalt
              recoveryKey
              sshAuthorizedKeys
Warning: lacking rights to acquire privileged fields of user record of 'testuser', output incomplete.
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 3:03:23pm)-(CPU 2.2%:Net 9)-(omen:~)-(12G:206)
└─ >> userdbctl user sddm
   User name: sddm
 Disposition: system
 Last Passw.: Mon 1996-10-28 02:00:00 EET
    Login OK: no (nologin shell)
 Password OK: no (none set)
         UID: 64
         GID: 64 (sddm)
 Aux. Groups: video
   Real Name: User for SDDM
   Directory: /var/lib/sddm
     Storage: classic
       Shell: /bin/false
Pas. Ch. Now: no
   Passwords: none
     Service: io.systemd.NameServiceSwitch
 Self Modify: none
      (Blobs) none
 (Privileged) none
```

```
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 3:04:38pm)-(CPU 2.1%:Net 9)-(omen:~)-(12G:206)
└─ >> varlinkctl call unix:/run/systemd/userdb/io.systemd.NameServiceSwitch \
  io.systemd.UserDatabase.GetUserRecord \
  '{"userName":"testuser","service":"io.systemd.NameServiceSwitch"}'
{
        "record" : {
                "userName" : "testuser",
                "uid" : 11003,
                "gid" : 11003,
                "homeDirectory" : "/home/testuser",
                "shell" : "/bin/bash",
                "passwordChangeNow" : false,
                "lastPasswordChangeUSec" : 1758672000000000,
                "passwordChangeMaxUSec" : 8639913600000000,
                "passwordChangeWarnUSec" : 604800000000,
                "status" : {
                        "670e4da96548c6680462a1d367326f46" : {
                                "service" : "io.systemd.NameServiceSwitch"
                        }
                }
        },
        "incomplete" : true
}
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 3:05:07pm)-(CPU 2.1%:Net 8)-(omen:~)-(12G:206)
└─ >> varlinkctl call unix:/run/systemd/userdb/io.systemd.NameServiceSwitch   io.systemd.UserDatabase.GetUserRecord   '{"userName":"sddm","service":"io.systemd.NameServiceSwitch"}'
{
        "record" : {
                "userName" : "sddm",
                "uid" : 64,
                "gid" : 64,
                "realName" : "User for SDDM",
                "homeDirectory" : "/var/lib/sddm",
                "shell" : "/bin/false",
                "passwordChangeNow" : false,
                "lastPasswordChangeUSec" : 846460800000000,
                "status" : {
                        "670e4da96548c6680462a1d367326f46" : {
                                "service" : "io.systemd.NameServiceSwitch"
                        }
                }
        },
        "incomplete" : false
}
```
```
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 3:06:25pm)-(CPU 2.0%:Net 8)-(omen:~)-(12G:206)
└─ >> userdbctl groups-of-user manos
USER  GROUP  
manos audio
manos cdrom
manos floppy
manos input
manos lp
manos netdev
manos plugdev
manos power
manos scanner
manos video

10 memberships listed.
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 3:07:32pm)-(CPU 1.9%:Net 8)-(omen:~)-(12G:206)
└─ >> userdbctl groups-of-user sddm
USER GROUP
sddm video

1 memberships listed.
```

```
userdbctl groups-of-user sddm
USER GROUP
sddm video

1 memberships listed.
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 3:07:39pm)-(CPU 1.9%:Net 8)-(omen:~)-(12G:206)
└─ >> userdbctl group avahi
  Group name: avahi
 Disposition: system
         GID: 214
     Service: io.systemd.NameServiceSwitch
```

### More info for testers:
```
userdbctl --help
userdbctl [OPTIONS...] COMMAND ...

Show user and group information.

Commands:
  user [USER…]               Inspect user
  group [GROUP…]             Inspect group
  users-in-group [GROUP…]    Show users that are members of specified groups
  groups-of-user [USER…]     Show groups the specified users are members of
  services                   Show enabled database services
  ssh-authorized-keys USER   Show SSH authorized keys for user

Options:
  -h --help                  Show this help
     --version               Show package version
     --no-pager              Do not pipe output into a pager
     --no-legend             Do not show the headers and footers
     --output=MODE           Select output mode (classic, friendly, table, json)
  -j                         Equivalent to --output=json
  -s --service=SERVICE[:SERVICE…]
                             Query the specified service
     --with-nss=BOOL         Control whether to include glibc NSS data
  -N                         Do not synthesize or include glibc NSS data
                             (Same as --synthesize=no --with-nss=no)
     --synthesize=BOOL       Synthesize root/nobody user
     --with-dropin=BOOL      Control whether to include drop-in records
     --with-varlink=BOOL     Control whether to talk to services at all
     --multiplexer=BOOL      Control whether to use the multiplexer
     --json=pretty|short     JSON output mode
     --chain                 Chain another command
     --uid-min=ID            Filter by minimum UID/GID (default 0)
     --uid-max=ID            Filter by maximum UID/GID (default 4294967294)
  -z --fuzzy                 Do a fuzzy name search
     --disposition=VALUE     Filter by disposition
  -I                         Equivalent to --disposition=intrinsic
  -S                         Equivalent to --disposition=system
  -R                         Equivalent to --disposition=regular
     --boundaries=BOOL       Show/hide UID/GID range boundaries in output
  -B                         Equivalent to --boundaries=no

See the userdbctl(1) man page for details.
```

```
6.15.9-zen+ ☘P.A.O ☘(Thu Sep-9 3:09:53pm)-(CPU 1.8%:Net 8)-(omen:~)-(12G:206)
└─ >> varlinkctl --help
varlinkctl [OPTIONS...] COMMAND ...

Introspect Varlink Services.

Commands:
  info ADDRESS           Show service information
  list-interfaces ADDRESS
                         List interfaces implemented by service
  list-methods ADDRESS [INTERFACE…]
                         List methods implemented by services or specific
                         interfaces
  introspect ADDRESS [INTERFACE…]
                         Show interface definition
  call ADDRESS METHOD [PARAMS]
                         Invoke method
  validate-idl [FILE]    Validate interface description
  help                   Show this help

Options:
  -h --help              Show this help
     --version           Show package version
     --no-pager          Do not pipe output into a pager
     --more              Request multiple responses
     --collect           Collect multiple responses in a JSON array
     --oneway            Do not request response
     --json=MODE         Output as JSON
  -j                     Same as --json=pretty on tty, --json=short otherwise
  -q --quiet             Do not output method reply
     --graceful=ERROR    Treat specified Varlink error as success
     --timeout=SECS      Maximum time to wait for method call completion
  -E                     Short for --more --timeout=infinity

See the varlinkctl(1) man page for details.
```

#### NOTE 
Everything here is unofficial and only for personal use.
If you want to test things clone repo and run SLackBuild, slackpkg new-config and reboot... <br>
**But you are on your own if system breaks.** 

I dont think man pages are 100% ok but i dont care so keep that in mind. 
