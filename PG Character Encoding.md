---
tags:
- Other
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]
# Unicode :
- Unicode provides a unique way to define every character in every spoken language of the world by assigning it a unique number. The Unicode standard is maintained by the Unicode Consortium and defines more than 1,40,000 characters from more than 150 modern and historic scripts along with emoji.
- Unicode can be defined with different character encoding like UTF-8, UTF-16, UTF-32, etc. Among these UTF-8 is the most popular as it used in over 90% of websites on the World Wide Web as well as on most modern Operating systems like Windows.
# ASCII :
- It is a character encoding standard for electronic communication. American Standard Code for Information Interchange(ASCII) and was first launched in 1963. ASCII codes are used to represent text in computers and telecom devices.
- ASCII is used for representing 128 English characters in the form of numbers, with each letter being assigned to a specific number in the range 0 to 127. For e.g., the ASCII code for uppercase A is 65, uppercase B is 66, and so on. Check out the following table for some more examples.
- Most computers are using ASCII encoding for text representation, which makes transferring data from one device to another a lot easier.
# The Difference Between Unicode and UTF-8
## Unicode
- Unicode is a character set.
- It is a list where all characters have a unique decimal number:
```
A	=	65
B	=	66
C	=	67
D	=	69
```
- The decimal numbers that represent the string "hello"is 104 101 108 108 111
## Encoding
- UTF-8 is encoding.
- It is how unicode numbers are translated into binary numbers to be stored in the computer:
- UTF-8 encoding will store "hello" like this (binary): 01101000 01100101 01101100 01101100  01101111
- Unicode is a **character set**. It translates characters to numbers.
- UTf-8 is an **encoding standard**. It translates numbers into binary.
# UTF-8 Code Groups by Values
| Utf-8 Code Group                                                                               | Dec           | Hex         |
| ---------------------------------------------------------------------------------------------- | ------------- | ----------- |
| [Basic Latin](https://www.w3schools.com/charsets/ref_utf_basic_latin.asp)                      | 0-127         | 0000-007F   |
| [Latin-1 Supplement](https://www.w3schools.com/charsets/ref_utf_basic_latin.asp#mark_supp)     | 128-255       | 0080-00FF   |
| [Latin Extended-A](https://www.w3schools.com/charsets/ref_utf_latin_extended_a.asp)            | 256-383       | 0100-017F   |
| [Latin Extended-B](https://www.w3schools.com/charsets/ref_utf_latin_extended_a.asp#mark_ex_b)  | 384-591       | 0180-024F   |
| [Latin IPA Extensions](https://www.w3schools.com/charsets/ref_utf_latin_ipa.asp)               | 592-685       | 0250-02AD   |
| [Spacing Modifiers](https://www.w3schools.com/charsets/ref_utf_modifiers.asp)                  | 688-767       | 02B0-02FF   |
| [Diacritical Marks](https://www.w3schools.com/charsets/ref_utf_diacritical.asp)                | 768-879       | 0300-036F   |
| [Greek and Coptic](https://www.w3schools.com/charsets/ref_utf_greek.asp)                       | 880-1023      | 0370-03FF   |
| [Cyrillic Basic](https://www.w3schools.com/charsets/ref_utf_cyrillic.asp)                      | 1024-1279     | 0400-04FF   |
| [Cyrillic Supplement](https://www.w3schools.com/charsets/ref_utf_cyrillic.asp)                 | 1280-1327     | 0500-052F   |
| [Armenian](https://www.w3schools.com/charsets/ref_utf_armenian.asp)                            | 1328-1423     | 0530-058F   |
| [Hebrew](https://www.w3schools.com/charsets/ref_utf_hebrew.asp)                                | 1424–1535     | 0590-05FF   |
| [Arabic](https://www.w3schools.com/charsets/ref_utf_arabic.asp)                                | 1536–1791     | 0600-06FF   |
| [Syriac](https://www.w3schools.com/charsets/ref_utf_syriac.asp)                                | 1792–1871     | 0700-074F   |
| [Arabic Supplement](https://www.w3schools.com/charsets/ref_utf_arabic.asp)                     | 1872-1919     | 0750-0/FF   |
| [Syriac Supplement](https://www.w3schools.com/charsets/ref_utf_syriac.asp)                     | 2144–2159     | 0860-086F   |
| [Arabic Extended-A](https://www.w3schools.com/charsets/ref_utf_arabic.asp)                     | 2208-2303     | 08A0-08FF   |
| [Hindi / Devanagari](https://www.w3schools.com/charsets/ref_utf_hindi.asp)                     | 2304–2431     | 0900-097F   |
| [Thai](https://www.w3schools.com/charsets/ref_utf_thai.asp)                                    | 3584–3711     | 0E00-0E7F   |
| [Georgian](https://www.w3schools.com/charsets/ref_utf_georgian.asp)                            | 4256–54351    | 10A0-10FF   |
| [Ethiopic](https://www.w3schools.com/charsets/ref_utf_ethiopic.asp)                            | 4608–4991     | 1200-137F   |
| [Ethiopic Supplement](https://www.w3schools.com/charsets/ref_utf_ethiopic.asp)                 | 4992-5023     | 1380-139F   |
| [Cherokee](https://www.w3schools.com/charsets/ref_utf_cherokee.asp)                            | 5024–5119     | 13A0-13FF   |
| [Canadian Aboriginal](https://www.w3schools.com/charsets/ref_utf_aboriginal.asp)               | 5120–5759     | 1400-167F   |
| [Ogham](https://www.w3schools.com/charsets/ref_utf_ogham.asp)                                  | 5760-5791     | 1680-169C   |
| [Runic](https://www.w3schools.com/charsets/ref_utf_runic.asp)                                  | 5792-5887     | 16A0-16FF   |
| [Tagalog](https://www.w3schools.com/charsets/ref_utf_pilipino.asp)                             | 5888-5819     | 1700-171F   |
| [Hanunoo](https://www.w3schools.com/charsets/ref_utf_pilipino.asp)                             | 5920-5951     | 1720-173F   |
| [Buhid](https://www.w3schools.com/charsets/ref_utf_pilipino.asp)                               | 5952-5983     | 1740-175F   |
| [Tagbanwa](https://www.w3schools.com/charsets/ref_utf_pilipino.asp)                            | 5984-6015     | 1760-177F   |
| [Canadian Aboriginal Extended](https://www.w3schools.com/charsets/ref_utf_aboriginal.asp)      | 6320–6399     | 18B0-18FF   |
| [Cyrillic Extended C](https://www.w3schools.com/charsets/ref_utf_cyrillic.asp)                 | 7296-7304     | 1C80-1C89   |
| [Georgian Extended](https://www.w3schools.com/charsets/ref_utf_georgian.asp)                   | 7312-7359     | 1C90-1CBF   |
| [Phonetic Extensions](https://www.w3schools.com/charsets/ref_utf_latin_ipa.asp)                | 7424-7551     | 1D00-1D7F   |
| [Phonetic Extensions Supplement](https://www.w3schools.com/charsets/ref_utf_latin_ipa.asp)     | 7552-7615     | 1D80-1DBF   |
| [Diacritical Marks Supplement](https://www.w3schools.com/charsets/ref_utf_diacritical.asp)     | 7616-7679     | 1DC0-1DFF   |
| [Latin Extended Additional](https://www.w3schools.com/charsets/ref_utf_latin_extended_a.asp)   | 7680-7935     | 1E00-1EFF   |
| [Greek Extended](https://www.w3schools.com/charsets/ref_utf_greek_extended.asp)                | 7680-7935     | 1F00-1FFF   |
| [General Punctuation](https://www.w3schools.com/charsets/ref_utf_punctuation.asp)              | 8192-8303     | 2000-206F   |
| [Superscripts and Subscripts](https://www.w3schools.com/charsets/ref_utf_supsub.asp)           | 8304-8351     | 2070-209F   |
| [Currency Symbols](https://www.w3schools.com/charsets/ref_utf_currency.asp)                    | 8352-8399     | 20A0-20CF   |
| [Letterlike Symbols](https://www.w3schools.com/charsets/ref_utf_letterlike.asp)                | 8448-8527     | 2100-214F   |
| [Number Forms](https://www.w3schools.com/charsets/ref_utf_number_forms.asp)                    | 8528-8591     | 2150-218F   |
| [Arrows](https://www.w3schools.com/charsets/ref_utf_arrows.asp)                                | 8592-8703     | 2190-21FF   |
| [Mathematical Operators](https://www.w3schools.com/charsets/ref_utf_math.asp)                  | 8704-8959     | 2200-22FF   |
| [Misc Technical](https://www.w3schools.com/charsets/ref_utf_technical.asp)                     | 8960-9215     | 2300-23FF   |
| [Enclosed Alphanumeric](https://www.w3schools.com/charsets/ref_utf_enclosed_alpha.asp)         | 9312-9471     | 2460-24FF   |
| [Box Drawings](https://www.w3schools.com/charsets/ref_utf_box.asp)                             | 9472-9599     | 2500-257F   |
| [Block Elements](https://www.w3schools.com/charsets/ref_utf_block.asp)                         | 9600-9631     | 2580-259F   |
| [Geometric Shapes](https://www.w3schools.com/charsets/ref_utf_geometric.asp)                   | 9632-9727     | 25A0-25FF   |
| [Miscellaneous Symbols](https://www.w3schools.com/charsets/ref_utf_symbols.asp)                | 9728-9983     | 2600-26FF   |
| [Dingbats](https://www.w3schools.com/charsets/ref_utf_dingbats.asp)                            | 9984-10175    | 2700-27BF   |
| [Misc Mathematical Symbols A](https://www.w3schools.com/charsets/ref_utf_math_a.asp)           | 10176-10223   | 27C0-27EF   |
| [Supplemental Arrows A](https://www.w3schools.com/charsets/ref_utf_arrows_supp_a.asp)          | 10224-10239   | 27F0-27FF   |
| [Braille](https://www.w3schools.com/charsets/ref_utf_braille.asp)                              | 10240-10495   | 2800-28FF   |
| [Supplemental Arrows B](https://www.w3schools.com/charsets/ref_utf_arrows_supp_b.asp)          | 10496-10623   | 2900-297F   |
| [Misc Mathematical Symbols B](https://www.w3schools.com/charsets/ref_utf_math_b.asp)           | 10624-10751   | 2980-29FF   |
| [Supplemental Math Operators](https://www.w3schools.com/charsets/ref_utf_math.asp)             | 10752-11007   | 2A00-2AFF   |
| [Misc Symbols and Arrows](https://www.w3schools.com/charsets/ref_utf_misc_symb_arr.asp)        | 11008-11263   | 2B00-2BFF   |
| [Glagolitic](https://www.w3schools.com/charsets/ref_utf_glagolitic.asp)                        | 11264-11359   | 2C00-2C5F   |
| [Latin Extended C](https://www.w3schools.com/charsets/ref_utf_latin_extended_a.asp#mark_ex_c)  | 11360-11391   | 2C60-2C7F   |
| [Coptic](https://www.w3schools.com/charsets/ref_utf_greek.asp)                                 | 11392-11519   | 2C80-2CFF   |
| [Georgian Supplement](https://www.w3schools.com/charsets/ref_utf_georgian.asp)                 | 11520-11567   | 2D00-2D2F   |
| [Ethiopic Extended](https://www.w3schools.com/charsets/ref_utf_ethiopic.asp)                   | 11648-11743   | 2D80-2DDF   |
| [Cyrillic Extended A](https://www.w3schools.com/charsets/ref_utf_cyrillic.asp)                 | 11744-11755   | 2DE0-2DFF   |
| [Supplemental Punctuation](https://www.w3schools.com/charsets/ref_utf_punctuation.asp)         | 11776-11903   | 2E00-2E7F   |
| [CJK Radicals Supplement](https://www.w3schools.com/charsets/ref_utf_cjk_radicals.asp)         | 11904-12031   | 2E80-2EFF   |
| [CJK KangXi Radicals](https://www.w3schools.com/charsets/ref_utf_cjk_radicals.asp)             | 12032-12255   | 2F00-2FDF   |
| [CJK Unified Ideographs](https://www.w3schools.com/charsets/ref_utf_cjk_ideographs.asp)        | 19968-40959   | 4E00-9FFF   |
| [Cyrillic Extended B](https://www.w3schools.com/charsets/ref_utf_cyrillic.asp)                 | 42460-42655   | A640-A69F   |
| [Latin Extended D](https://www.w3schools.com/charsets/ref_utf_latin_extended_a.asp#mark_ex_d)  | 42784-43007   | A720-A7FF   |
| [Latin Extended E](https://www.w3schools.com/charsets/ref_utf_latin_extended_a.asp#mark_ex_e)  | 42784-43007   | A720-A7FF   |
| [Gothic](https://www.w3schools.com/charsets/ref_utf_gothic.asp)                                | 66352-66383   | 10330-1034F |
| [Aegean Numbers](https://www.w3schools.com/charsets/ref_utf_aegean.asp)                        | 65792-65855   | 10100-1013F |
| [Phoenican](https://www.w3schools.com/charsets/ref_utf_lydian.asp)                             | 67840-67871   | 10900-1091F |
| [Lydian](https://www.w3schools.com/charsets/ref_utf_lydian.asp)                                | 67872-67903   | 10920-1093F |
| [Meroitic Hieroglyphs](https://www.w3schools.com/charsets/ref_utf_meroitic_hieroglyphs.asp)    | 67968-67999   | 10980-1099F |
| [Egyptian Hieroglyphs](https://www.w3schools.com/charsets/ref_utf_egyptian_a.asp)              | 77824–78895   | 13000–1342F |
| [Musical Symbols](https://www.w3schools.com/charsets/ref_utf_music.asp)                        | 119040-119295 | 1D100-1D1FF |
| [Math Alphanumeric](https://www.w3schools.com/charsets/ref_utf_math_alpha.asp)                 | 119808-120831 | 1D400-1D7FF |
| [Mahjong Tiles](https://www.w3schools.com/charsets/ref_utf_tiles.asp)                          | 126976-127023 | 1F000-1F02F |
| [Domino Tiles](https://www.w3schools.com/charsets/ref_utf_tiles.asp)                           | 127024-127135 | 1F030-1F09F |
| [Playing Cards](https://www.w3schools.com/charsets/ref_utf_cards.asp)                          | 127136-127231 | 1F0A0-1F0FF |
| [Enclosed Alpha Supplement](https://www.w3schools.com/charsets/ref_utf_enclosed_alpha.asp)     | 127232-127487 | 1F100-1F1FF |
| [Enclosed Ideographic](https://www.w3schools.com/charsets/ref_utf_enclosed_ideographic.asp)    | 127488-127743 | 1F200-1F2FF |
| [Miscellaneous Symbols](https://www.w3schools.com/charsets/ref_utf_misc_symbols.asp)           | 127744-128511 | 1F300-1F5FF |
| [Emoticons](https://www.w3schools.com/charsets/ref_utf_emoticons.asp)                          | 128512-128591 | 1F600-1F64F |
| [Ornamental Dingbats](https://www.w3schools.com/charsets/ref_utf_ornamental.asp)               | 128592-128639 | 1F650-1F67F |
| [Transport and Maps](https://www.w3schools.com/charsets/ref_utf_transport.asp)                 | 128640-128767 | 1F680-1F6FF |
| [Alchemical Symbols](https://www.w3schools.com/charsets/ref_utf_alchemical.asp)                | 128768-128895 | 1F700-1F77F |
| [Geometric Shapes Extended](https://www.w3schools.com/charsets/ref_utf_geometric.asp#mark_ext) | 128896-129023 | 1F780-1F7FF |
| [Supplemental Arrows C](https://www.w3schools.com/charsets/ref_utf_arrows_supp_c.asp)          | 129024-129279 | 1F800-1F8FF |
| [Supplemental Symbols](https://www.w3schools.com/charsets/ref_utf_symbols_supp.asp)            | 129280-129535 | 1F900-1F9FF |
