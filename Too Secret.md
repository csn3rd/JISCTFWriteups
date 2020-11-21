# Too Secret Writeup

This problem is worth 250 points in the Misc category. This was one of the tougher challenges in the whole competition with only 5% of teams being able to solve it. I was the 7th to solve this challenge.

![](https://github.com/csn3rd/JISCTFWriteups/blob/master/Screen%20Shot%202020-11-21%20at%203.23.03%20PM.png)

## Problem Statement:
The flag is in this format: JISCTF{data-data}

### voice_message.wav
```
76 83 52 117 76 105 65 116 76 83 65 116 76 83 48 103 76 105 48 117 76 83 65 117 73 67 48 116 76 105 48 103 76 108 57 102 88 49 56 117 73 67 52 116 73 67 52 117 76 105 52 117 73 67 52 103 76 108 57 102 88 49 56 117 73 67 52 103 76 83 48 116 76 83 65 116 76 83 48 116 76 105 65 117 73 67 53 102 88 49 57 102 76 105 65 116 73 67 48 116 76 83 48 117 73 67 48 116 76 83 65 117 76 83 52 117 73 67 53 102 76 108 56 117 76 105 65 117 88 121 53 102 76 105 52 75
```

## Solution

We are given an audio file with a voice saying out a bunch of numbers. We can use some online transcription or speech recognition software to get those numbers so we can work with them. Once we have the numbers, we recognize that the numbers are the decimal representations of some ASCII characters. As a reference, here is the conversion chart for the standard printable characters to their binary, octal, hex, and decimal representations.

![](https://github.com/csn3rd/JISCTFWriteups/blob/master/ascii_table.png)

We can convert from decimal to ASCII to get `LS4uLiAtLSAtLS0gLi0uLSAuIC0tLi0gLl9fX18uIC4tIC4uLi4uIC4gLl9fX18uIC4gLS0tLSAtLS0tLiAuIC5fX19fLiAtIC0tLS0uIC0tLSAuLS4uIC5fLl8uLiAuXy5fLi4K`. It seems like this ciphertext is formed into blocks of 4 which seems to show that we are looking at some Base 64 encoded text.

Decode from Base 64 and we get:
```
-... -- --- .-.- . --.- .____. .- ..... . .____. . ---- ----. . .____. - ----. --- .-.. ._._.. ._._..
```

This seems like morse code but not quite since we have '-', '.', '\_', and ' '. We can convert all the '\_' to '-' because both represent a long tap and may be interchangeable. As a reference, Here is the conversion chart for morse code.

![](https://github.com/csn3rd/JISCTFWriteups/blob/master/morse_table.jpg)

We see that the string `.----.` appears multiple times but is not valid morse code for any characters. However, the morse code for hyphen matches the pattern with `-....-`. So, let's flip all the long taps to short taps and all the short taps to long taps.

```
.--- .. ... -.-. - ..-. -....- -. ----- - -....- - .... ....- - -....- . ....- ... -.-- -.-.-- -.-.--
```

We compare this to the table and determine that it is a valid morse code sequence. We decode it to get `JISCTF-N0T-TH4T-E4SY!!`, which is our flag.

## Flag

`JISCTF{JISCTF-N0T-TH4T-E4SY!!}`

