---
title: "[CTF.SG'22] Wildest Dreams Pt 2"
date: 2022-03-13T00:00:00+08:00
authors:
- Bowen
tags:
- web
- php
ctfs:
- CTF.SG'22
---

# Wildest Dreams Pt. 2 (CTF.SG 2022)

![image](https://user-images.githubusercontent.com/85286288/158060498-00273d94-e72e-4a6d-a853-256b5b58555e.png)

## 1989.php

Once we open this file, a simple glance through the source code shows that the
flag will be revealed once `md5(i1) == md5(i2)`.

![image](https://user-images.githubusercontent.com/85286288/158060585-a64d1573-dbf9-497c-a4b1-62dfb10e3104.png)

The only requirements for `i1` and `i2` is that they shouldn't be the same, and
they must have a length of more than 15 characters.

![image](https://user-images.githubusercontent.com/85286288/158060645-d40bb341-f9eb-4537-9c7f-319f704846c4.png)

In php, two strings matching the regular expression `0+e[0-9]+` compared with
`==` returns `true`. Thus, strings such as `0e69`, `0e12` etc will return `true`
when compared with each other.

Some `md5` hashes will output hashes starting with `0e`, and these are called
`magic hashes`, you can read up on them [here](https://www.whitehatsec.com/blog/magic-hashes/).

Thus, all we need to do is to pass in 2 strings which will evaluate to `0e...`,
and viola, we get the flag! These strings are easily found by searching
`md5 magic hashes` on google. The ones I used were `hashcatsB4SOwuGVuoe` and
`hashcatfaXyv0NCydC2`, which I took from https://github.com/spaze/hashes/blob/master/md5.md.

You can see from here that both strings evaluate to `0e...` when converted to an
md5 hash.

![image](https://user-images.githubusercontent.com/85286288/158060995-56ad840b-d4d7-4268-a241-122920fe7748.png)
![image](https://user-images.githubusercontent.com/85286288/158061006-7ebcf25c-a290-460e-87da-97bcd236b7ee.png)

Query to get flag: http://chals.ctf.sg:40401/1989.php?i1=hashcatsB4SOwuGVuoe&i2=hashcatfaXyv0NCydC2

## Flag

The flag is: `CTFSG{you_see_me_in_h1nds1ght_tangled_up_with_you_all_night}`
![image](https://user-images.githubusercontent.com/85286288/158060933-25a35803-88dd-4297-9de6-eedcfc42cede.png)
