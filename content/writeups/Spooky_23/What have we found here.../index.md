---
title: "[SpookyCTF'23] What Have We Found Here..."
date: 2023-10-29T10:09:26+08:00
authors:
- Kairos
tags:
- crypto
ctfs:
- Spooky23
---

## SpookyCTF'23: What Have We Found Here...

This was a simple cryptography challenge. We are given a [text file](found_notes.txt) containing a long string of weird characters.

![Text.png](text.png)

## Identifying the Cipher

I copied the whole string of characters into a [Cipher Identifier](https://www.dcode.fr/cipher-identifier) online tool, and it indicated a high probability of Base64 encoding. 

## Solving for The Flag
Plugging it into a [base64 decrypting](https://www.dcode.fr/base-64-encoding) tool, it seemed that the ascii characters it generated was just complete gibberish. Here's a small snippet:
```
ã�eí,Q¸ia���õ¨]�æbß7q�·v�Ûj;î=hkukr>^SÖ¹ÜY�ÜÅU;s'\ö«3L$P�­CöWV*?�V"³�È7
�¦�ÆdÌÜ«)Îã���Ä6íæ�néZwP���¹jÃº%¤ØªØ_j�É¨�:��:¸äc�ÈûÒH¬Í�=)#m��9��Æ]®Þõb���óUå�¤=ª&óCm�JV¸&Zvó��C®T�Z]«�c±¨Ù²¸§��ÌG�89©*%j]ÕdÉØ�Ó#Îî)Y¸¦£c<UÜ�xÏQP*�Ã5e�8â�´ÀzýÝ´å%s�jÒ«V3Ü�£�ÍMh¡U�}ê�È»ö¨§»�× Òµ�bÔGCÎw/ëK¶�)�­�ËT�_��SQ�1-º��}*±�Æå&�±Ë#mZ�]�ý�1«oÿ�v£sJjä�j6�]?:g��õzßÂºÍÄ�¼�ù}©ÃÃwq�­
```

Next, I tried to toggle it to the `File to Download` function on the same website, which produced an image file!

![flag](flag.png)