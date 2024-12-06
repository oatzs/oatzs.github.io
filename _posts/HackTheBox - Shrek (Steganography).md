---
title: "HackTheBox - Shrek (Steganography)"
date: Fall 2024
categories:
  - blog
tags:
  - Jekyll
  - update
---

Went into uploads, found a php rev shell alluding to an area 51 directory, went there and downloaded an .mp3. Cut the end of the .mp3 which was just static noise, put it into spectrogram (which required max frequency on spectogram scale settings) and we got an ftp login.
`mget *.txt`
```bash
mget *.txt
```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
