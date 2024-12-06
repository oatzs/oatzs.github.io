---
title: "HackTheBox - Shrek (Steganography)"
categories:
  - blog
---

Went into uploads, found a php rev shell alluding to an area 51 directory, went there and downloaded an .mp3. Cut the end of the .mp3 which was just static noise, put it into spectrogram (which required max frequency on spectogram scale settings) and we got an ftp login.
`mget *.txt`
```bash
mget *.txt
```
