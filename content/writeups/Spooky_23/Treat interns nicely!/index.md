---
title: "[SpookyCTF'23] Treat interns nicely!"
date: 2023-10-29T11:04:26+08:00
authors:
- Kairos
tags:
- misc
ctfs:
- Spooky23
---

## SpookyCTF'23: Treat interns nicely!
```
Look, I know that they sometimes work for free.

But you really gotta treat the interns nicely. They could run this whole thing, you know! Even if they're not actually human.
```

## Explanation

This was a really fun(ny) misc challenge. In SpookyCTF, the 'interns' were the bots in the Discord channel. There are two main commands, `/compliment` and `/complain`. 

Using `/compliment` to "treat" the interns nicely, we would get random sarcastic replies like:
```
At NICC, we're over the moon (or should I say, over Zorglax?) with your positive feedback!

You're spared from abduction, but you're still on the menu for our intergalactic humor.

I'm not sure if I should be flattered or bored by all this positive feedback.

Consider this your lucky day. We won't be abducting you... for now.

I'm glad you think so.

Oh joy, more positive feedback. What a surprise.
```

After bombarding the intern with around 25+ `/compliment`s, it gave us the flag!
```
Being nice pays off. Here's a flag! NICC{S0m3tim3s_b31ng_n1c3_h3lps}
```

Pretty interesting challenge lol.