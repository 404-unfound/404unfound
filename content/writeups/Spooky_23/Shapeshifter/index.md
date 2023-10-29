---
title: "[SpookyCTF'23] Shapeshifter"
date: 2023-10-29T10:09:26+08:00
authors:
- Kairos
tags:
- misc
ctfs:
- Spooky23
---

## SpookyCTF'23: Shapeshifter
```
There's this figure in front of me, but I can't even figure out what it is! What is that thing??
```
[Challenge File](distant-figure.png)

This was an interesting misc challenge, mixed with a little bit of steganography in it.

## The image
The image was a corrupted png file. I tried to fix the header but it didn't work :O

I went on to check the strings of the file:
```
kairos@pop-os:~/Downloads$ strings distant-figure.png 
nBWT\
distant-figure/true-form.exe
tK4%dw
mn(a
m3L5sS
r9'1
{p87
yKO&
xV '(s
yZ>#
q~]t
Xg&h/
Gcg f
YvFW
jxXT
vXhE
jnRK
2_1Bx
GHaW
U,E/
g>LC#5
%86rs7jcD
PnmO
/n4R
PpJm
<'J:s
?~9s
w><K/
m5QRs
p.g&
p6<z
I:p%S
:e#}
*x";i
!Fa]
)<L"
8E!d
,j3x
A0#z
##Qy	
L8,r
(%\`K
x6	;
F@38
IQBP
U,u)6
#:BH
 D#*JdN
a+_u%
^,2 
$W*e
Gyf:
oQn,0 
;RoQ 
Hqi@,
qL~L(
kRb]
@r4S
:C4V
FQ)h
VhzKz
U-8H A
D/n(
5-T@a
eM42&
$%js
gJb+
4$B(
J?O:
i+]g
EJp )
H#ni
aiF7
LRyC
bf%eG
?:P94
26@	
rV;wRk^
Wu=w[Z
Rf0Wp
OfRTfA/
m2)M4`
`bS$
+289
Fxw+
]e/7#MJ
B']e
n;>3@
433@
Z'*w/
/Utz_
k,=i
WWb6
dOx1
IAkD5&(&
m}1)
c*sw>
h\v8h<.
ckBi
.uhn
ZIm)
`Sc	
7q86
9]4M'
I2)hS
O'mJ
}#=[Co
M(L;
Oh:8{
$yHS
_)L#k
nBWT\
distant-figure/true-form.exePK
```

From this, it seems like there is a hidden executable file within the image. To retrieve it, I used `binwalk` to extract the hidden files.

## Analysing the Hidden Files
The extracted files contained a folder `distant-figure`, and within the folder was the `true-form.exe`.

Again, I used `strings` to take a look at the exe file.
```bash
kairos@pop-os:~/Downloads$ strings binwalk/distant-figure/true-form.exe > output.txt
```

Looking for the flag format `NICC`, I got a lead :D
```
\hich\af31506\dbch\af31505\loch\f31506 NICC}{\rtlch\fcs1 \af0\afs16 \ltrch\fcs0 \fs16\cf20\insrsid9635513\charrsid14358698 \{\hich\af31506\dbch\af31505\loch\f31506 Y0U_F0UND_M
}{\rtlch\fcs1 \af0\afs16 \ltrch\fcs0 \fs16\cf20\insrsid1073285\charrsid14358698 \hich\af31506\dbch\af31505\loch\f31506 Y_\hich\af31506\dbch\af31505\loch\f31506 TRU\hich\af31506\dbch\af31505\loch\f31506 3_F0RM}{\rtlch\fcs1 \af0\afs16 \ltrch\fcs0 
```

There are fragments of the flag scattered all over:
- NICC
- Y0U_F0UND_M
- Y_
- TRU
- 3_F0RM

## Getting the Flag

Piecing together all the fragments, we get the flag!
`NICC{Y0U_F0UND_MY_TRU3_F0RM}`