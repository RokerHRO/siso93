# Siso93 – A human-readable binary-to-text encoding

As with Base85 we encode 4 octets into 5 characters, so the "encoding overhead" is 25% + overhead due to line breaks.
The encoded data has this format:
```
    ,-----+------+------+------+------.
    | tag | enc0 | enc1 | enc2 | enc3 |
    `-----+------+------+------+------'
```
Every input data octet is represented by one output character. The 256 possible values are divided into 3 "planes" with 85…86 characters per plane. The "tag" character encodes the planes for the four following bytes. So there are 3x3x3x3 = 81 different tags possible.

We chose the encoding alphabet so ASCII text is as human-readable as possible: Nearly all printable ASCII bytes are encoded 1:1. The encoding alphabet for the "tag" character is chosen that ASCII-dominating input text will have "light" tag characters that disturbs the human reader as little as possible.

The encoding of the data octets shows this table:

![octet encoding table](siso93.png | width=982)

The encoding of the "tag" byte shows this table:

![tag encoding table](siso93-tag.png | width=600)
