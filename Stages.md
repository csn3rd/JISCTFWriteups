# Stages Writeup

This problem is worth 200 points in the "Crypto and Stego" category. This was one of the tougher challenges in the whole cmpetition with only 8% of teams being able to solve it. I was the 14th to solve this challenge.

![](https://github.com/csn3rd/JISCTFWriteups/blob/master/Screen%20Shot%202020-11-21%20at%202.49.15%20PM.png)

## Problem Statement:
Try to recover this file!!

### crypto.dat
```
5sdk'6:+"*5sdk'5sdn)5sdn)5sdk'5sdk'5sdk(5sdn(5s[h(5sdn(6:!q)5sdn)5s[h(5s[h(6:!n'5sdk'6:!q)5sdk'5sdk'5sdn)5s[h(5s[h(5s[e&5sdk'6:*t(5s[h(5s[e&5sdk(6:!q(5sdn(5sdn(5sdn(5s[h'5sdk'5s[h(5s[h(6:!n'5s[h(5s[h'5sdk(5s[h'5sdk(5s[h(5sdn)5sdn)5s[h(5sdk(5sdk'6:!q)5sdk(5sdn)5s[h(5s[e&5s[h(5s[h(5sdk'6:+"*5s[h(5s[h'5sdk(5s[e'5sdn(6:!q)5sdk(5sdk(5s[h(5s[e&5sdk'6:!q(5sdn)5s[h(5sdk'6:+")5s[h(5s[e'5sdk'5sdn(5sdn)5s[e'5sdk(5s[h'5sdk'5sdn)5sdk(5sdk(5s[h(6:!n'5sdk(5s[e&5s[h(5s[h'5sdk(5sdn(5sdk'5s[e'5sdk(5s[e&5sdk'5sdk'5s[h(5sdk(5sdn)5sdn)5sdk(6:!n(5sdn(6:!q(5sdn(6:!n'5sdn)5s[h'5sdk(5s[h'5sdk'6:!n'5sdk'6:!q(5sdk'6:*t(5sdk(5s[e&5sdk'5s[h(5sdn(5sdn)5s[h(5sdn)5sdk'6:+"*5sdk(5s[e'5s[h(6:*t)5s[h(6:*t)
```

## Solution

It seems like the ciphertext in crypto.dat is formed into blocks of five bits. This is a tell that we are looking at something that was encoded by ASCII85. Let's decode it and see what we get.

```
ABAABBBBABAAABBBABBBABAAABAAABABABBAAABBABBABABBABBBAABBAABBBAAAABAABABBABAAABAAABBBAABBAABBAAAAABAABBAAAABBAAAAABABBABAABBAABBAABBAAABAABAAAABBAABBBAAAAABBAABAABABAABAABABAABBABBBABBBAABBABABABAABABBABABABBBAABBAAAAAABBAABBABAABBBBAABBAABAABABAAABABBABABBABABABABAABBAAAAABAABABAABBBAABBABAABBBAAABBAAABABAAABBAABBBAAABABABAABAABAAABBBABABABABAABBBAAAABABAAAAAABBAABAABABABBAABAAAAABABABAAAAABAAABAAAABBABABABBBABBBABABBAABABBABABAABBABAAAABBBAABAABABAABAABAABAAAABAABABAABAABBAAABABAAAAABAAAABBABBAABBBAABBABBBABAABBBBABABAAABAABBBBABAABBBBAB
```

We recognize that this resulting ciphertext has 2 characters, A and B. So, let's convert to binary and see if we get any notable results. As a reference, here is the conversion chart for the standard printable characters to their binary, octal, hex, and decimal representations.

![](https://github.com/csn3rd/JISCTFWriteups/blob/master/ascii_table.png)

We notice that each character in binary representation must have a 0 in the first bit position. So, let's convert A to 0 and B to 1.

```
0100111101000111011101000100010101100011011010110111001100111000010010110100010001110011001100000100110000110000010110100110011001100010010000110011100000110010010100100101001101110111001101010100101101010111001100000011001101001111001100100101000101101011010101010011000001001010011100110100111000110001010001100111000101010010010001110101010100111000010100000011001001010110010000010101000001000100001101010111011101011001011010100110100001110010010100100100100001001010010011000101000001000011011001110011011101001111010100010011110100111101
```

This is a valid binary encoded string and decoding it gets us the following.

```
OGtEcks8KDs0L0ZfbC82RSw5KW03O2QkU0JsN1FqRGU8P2VAPD5wYjhrRHJLPCg7OQ==
```

This resulting string is encoded by some base, most probably base 64 or base 32 by the two equal signs at the end. Let's try base64.

```
8kDrK<(;4/F_l/6E,9)m7;d$SBl7QjDe<?e@<>pb8kDrK<(;9
```

Once again, our ciphertext seems to be encoded by ASCII 85.

When we decode it, we get `JISCTF{Multiple_Enoding_of_data_JISCTF}`, which is our flag.

## Flag

`JISCTF{Multiple_Enoding_of_data_JISCTF}`
