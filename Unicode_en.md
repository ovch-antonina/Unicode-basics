### Unicode and its encodings UTF-8 and UTF-16
Letters, punctuation marks, and other characters need to be converted into digital data to be stored in computer memory (which allows to do things like transmit them over the Internet). That is the purpose of character encoding systems, which assign a unique numerical code to each encoded character (it should be noted that the word “character” in this context does not always correspond to characters in their human understanding and could also refer to, for example, control characters such as tab or newline).

**Unicode** is a universal character encoding system created and maintained by the non-profit organization “The Unicode Consortium”. Unicode is officially named the Unicode Worldwide Character Standard and is not a single character encoding, but a standard that includes a set of characters and a family of encodings for that set.

The Unicode character set is called **UCS** (Universal Character Set). It assigns each character a numeric code that is written as U+[hexadecimal code] and is called a _code point_. The Unicode code space (that is, the code points allowed for use within the standard) consists of code points 0 to 10FFFF. The Unicode character set includes characters from all major writing systems, as well as technical symbol systems, and some other characters, for example Emoji.  At the moment the Unicode standard covers 150 writing systems, and the total number of characters is close to 150,000. That is Unicode’s advantage over other character encoding systems that all cover a much smaller number of characters (usually only the Latin script plus a national alphabet or a specialized symbol system). Thanks to that, by now Unicode has been adopted in all modern software.

UCS codes are only a convention about which code corresponds to which character, they do not determine how these codes are represented in memory or in a file. Machine level representation of UCS codes is defined by UTF (Unicode Transformation Format) encodings.

Discussions about UTF encodings usually relate to the three Unicode **character encoding forms: UTF-8, UTF-16, UTF-32**. However, it should be noted that the abbreviation UTF can also refer to character encoding schemes, which are a character encoding form plus an indication of its endianness (byte order) or a lack of such indication: for example, the character encoding form UTF-16 corresponds to three character encoding schemes – UTF-16 (endianness isn’t explicitly stated), UTF-16LE (little-endian), and UTF-16BE (big-endian).

Character encoding forms operate with _code units_ – bit sets of a fixed length, combinations of which are used to digitally represent UCS codes. Character encoding forms assign unique sequences of code units to code points U+0000..U+D7FF and U+E000..U+10FFFF (the U+D800...U+DFFF range is reserved for UTF-16 surrogate pairs, explained further).

All three character encoding forms encode the same set of characters (UCS), but they represent these characters as different byte sequences and distribute the **number of bytes needed to represent a character** in different ways.

First of all, different character encoding forms use code units of different sizes, corresponding to the titular number: in UTF-**_8_**, the size of the code unit is 1 byte (**_8_** bits), in UTF-**_16_** – 2 bytes (**_16_** bits).

Table 1. Code unit size

| UTF-8  | UTF-16  |
|:------:|:------:|
|1 byte|2 bytes|

Keep in mind that both UTF-8 and UTF-16 are variable-length encodings, so these code unit sizes don’t necessarily mean that a character takes up that number of bytes.

* **UTF-8** uses one code unit (=one byte) for code points U+0000…U+007F – the first Unicode block, “Basic Latin”, which contains upper- and lowercase Latin-script letters, as well as numbers, punctuation, and control characters. For that reason UTF-8 is the most effective encoding for texts in English and other Latin-script languages, since almost all characters of such texts take up 1 byte each in UTF-8. However, code points after U+007F take up 2, 3, or 4 code units (=bytes).

* In **UTF-16**, code points U+0000..U+D7FF and U+E000..U+FFFF take up one code unit (=2 bytes) each. These code points correspond to the Basic Multilingual Plane (BMP), which contains the most frequently used characters. The other, less frequently used code points U+10000..U+10FFFF are represented with the use of so called “surrogate pairs” of two code units and correspondingly take up 2 bytes each. Surrogate pairs use the aforementioned reserved code points U+D800…U+DFFF: each surrogate pair consists of a high surrogate from the range U+D800…U+DBFF, followed by a low surrogate from the range U+DC00…U+DFFF. Code points from these ranges cannot be used as stand-alone code points.

Table 2. Number of bytes needed to represent a character

| UTF-8  | UTF-16  |
|:------:|:------:|
|1-4|2 or 4|

For example, the character with the UCS code _U+0061_ (lowercase Latin “a”) is encoded in UTF-8 as _61_ (_0110000_)and uses one byte, and in UTF-16 it’s encoded as _0061_ and uses 2 bytes. . The character with a more complicated code _U+1D11E_ (G-clef "&#119070;") is encoded in UTF-8 as _F0 9D 84 9E_ (_11110000:10011101:10000100:10011110_), using as many as 4 bytes, and in UTF-16 as the surrogate pair _D834 DD1E_ (high surrogate _U+D834_ plus low surrogate _U+DD1E_), also 4 bytes.

UTF-8 obviously is more efficient, since it uses less memory in most cases thanks to the smaller code unit size. However, it depends on the writing system: in some cases, for example for Chinese text or musical notation, UTF-8 starts using 3-4 bytes for more complex characters and thus uses more memory than UTF-16, which basically uses 2 bytes for most of the commonly used characters.

Another advantage of UTF-8 is that the Basic Latin characters encoded in it with one byte match up to the corresponding ASCII characters 1:1, so UTF-8 is **backward compatible with ASCII**. If a file in UTF-8 consists of only ASCII characters, it is encoded in exactly the same way as the same file in ASCII. One particular practical consequence of this compatibility is that UTF-8 is more convenient for updating legacy applications.

Table 3. Backward compatible with ASCII

| UTF-8  | UTF-16  |
|:------:|:------:|
|yes|no|

**UTF-8** is predominately used **for web pages** and in general is the most popular and the most commonly used Unicode encoding (thanks to the advantages described above); but **UTF-16** is used **for internal Unicode representation** in many programming languages (Java, C#, JavaScript), as well as in operating systems, in particular in Windows (but Linux, for example, mostly uses UTF-8). This is the case for a number of reasons, in part just because UTF-16 was easier to convert to from earlier Unicode encodings that also had 2-byte code units.

Table 4. Predominantly used

| UTF-8  | UTF-16  |
|:------:|:------:|
|In web pages|In programming languages and operating systems|

In practice that sometimes makes UTF-16 more convenient just because that’s what’s adopted within the platform or programming language.