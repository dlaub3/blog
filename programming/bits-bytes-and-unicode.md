---
title: "Bits, Bytes, and Unicode"
date: 2018-07-29T19:13:07-04:00
draft: false
---



A **bit** is the smallest unit in computing. It is either a `1` or a `0` . Next up is a **byte**,  a byte is 8 bits. 256 (2^8) patterns can be represented by a byte. One byte can store the numbers 0 through 255 in decimal notation. The number 255 is `11111111` and is 8 bits, one byte in length. 



**ASCII** (American Standard Code for Information Interchange) is the first encoding standard. It is able to represent 128 characters. This is because ASCII uses a 7-bit encoding. It contains the English letters A-Za-z, numbers 0-9, and some special characters as well. So a single byte is able to store an ASCII character. HTML 4.0 defaulted to ASCII. 



**UTF-8** (Unicode Transformation Format )  is backwards compatible with ASCII. It essentially takes the 7-bit ASCII  encoding and expands it with more bits, and is thus able to represent more characters. UTF-8 characters can be from 1 to 4 bytes long. So UTF-8 uses a 16-bit encoding.  It is capable of representing all of Unicode with the ability to represent 2^21 characters. HTML5 defaults to using UTF-8. 



**Unicode** is a character set that represents almost all the characters in the world. 



**Encoding** is how the characters are translated into bits.  It's the 1 and 0 representation. 

> **Encoding** translates numbers into binary. **Character sets** translates characters to numbers.
>
> <cite>https://www.w3schools.com/charsets/ref_html_utf8.asp</cite>



The decimal representation of the character 'A' in ASCII is `&#65;` . When the character 'A' is written to disk it is stored in bits 0s and 1s in a single byte that is the binary number 65, `01000001`.



It is important to understand the relationship between encoding and character sets.



I'm not sure how UTF-8 got it's name. Perhaps because it can be written in octal notation? 



References 

- https://web.stanford.edu/class/cs101/bits-bytes.html
- https://www.w3schools.com/charsets/ref_html_ascii.asp
- https://en.wikipedia.org/wiki/ASCII#7-bit_codes
- https://stackoverflow.com/questions/10229156/how-many-characters-can-utf-8-encode
- https://www.w3schools.com/charsets/ref_html_utf8.asp
- https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/