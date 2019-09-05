# Siso93 – A human-readable binary-to-text encoding

## Example

| Input | Output |
|-------|--------|
| This is an example of Siso93 encoding. |`` `This`_is_`an_e`xamp`le_o`f_Si`so93`_enc`odin`g. ``|
| What does the word "Apfel" mean? |`` `What`_doe`s_th`e_wo.rd_*`Apfe,l*_m`ean? ``|
| What does the word "Любовь" mean? |`` `What`_doe`s_th`e_wo.rd_*CP{Qn$P1P3$P2Ql'*_me`an? ``|
| Die Größenänderung kostet 10€ pro Meter. |`` `Die_HGrC69C.enFC$nd`erun`g_ko`stet^_10brb,_p`ro_M`eter`. ``|
| 👩 + 🎄 = ❤️ | `Ap.q)^_+_pl.nd_G=_b}$$o8o` |
 
## How does it work
As with Base85 we encode 4 octets into 5 characters, so the "encoding overhead" is 25% + overhead due to line breaks.
The encoded data has this format:
```
    ,-----+------+------+------+------.
    | tag | enc0 | enc1 | enc2 | enc3 |
    `-----+------+------+------+------'
```
Every input data octet is represented by one output character. The 256 possible values are divided into 3 "planes" with 85…86 characters per plane. The "tag" character encodes the planes for the four following bytes. So there are 3x3x3x3 = 81 different tags possible.

We chose the encoding alphabet so ASCII text is as human-readable as possible: Nearly all printable ASCII bytes are encoded 1:1, except characters that are not "HTML-safe" or that are used for quoting and escaping: `&`, `<`, `>`, `"` and `\`. The encoding alphabet for the "tag" character is chosen that ASCII-dominating input text will have "light" tag characters that disturbs the human reader as little as possible.

The encoding of the data octets shows this table:

![data octet encoding](siso93.png)

White boxes denotes characters in "plane 0", yellow in "plane 1" and blue in "plane 2".
For some characters there are alternative encodings defined if their occurrence in the encoded text is unwanted. More alternative encodings can be defined in the future (the yellow plane 1 has a lot of spare space, yet).

The encoding of the "tag" byte shows this table:

![tag byte encoding](siso93-tag.png)

The first two data octets determine the table row, the last two data octets determine the table column.

Example: If the 4 octets are in the planes 0, 2, 2, 1, the tag byte is encoded as `Y`.

## See also
https://github.com/sveljko/base41 – another unusual binary-to-text encoding. :-)
