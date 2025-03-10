# ENCODING SCHEME 
IN THE ORDER THEY APPEAR IN THE FINAL BINARY STRING

First bit: 

* “1” indicates that the simplified text (after removing commas, upper-case letters and multiple spaces and periods) ends in a period; 
* “0” indicates it does not end in a period

A set of phrases; each phrase is encoded as follows:

* the number of words groups in this phrase (with an extra 1 after the initial 0s)
each word group is encoded as follows:
* an indicator of the type of encoding used; “0” indicates  Global Dictionary Encoding,  “1” indicates Local Dictionary Encoding or Standard Encoding; if the next two bits after the “1” are “10” then it is the Standard Encoding (and these two bits are garbage), otherwise it is the Local Dictionary Encoding (and this two bits are part of the actual encoding)
* the corresponding encoding for this word group (see below):

## Global Dictionary Encoding

* number of revealed characters (with an extra 1 after the initial 0s)
* list of \<position difference, char\> representing revealed characters (position difference is the difference between the index of the current revealed char and the previous one); both are encoded without an extra 1 after the initial 0s

* the position difference to the end of the word (without an extra 1 after the initial 0s)
* the rank in the suffix trie search + 1 (without an extra 1 after the initial 0s)
* Note: this rank is got by searching for this word with the last letter of the previous word (if there is a previous word in this phrase); if this is the start of the phrase, the rank is got by searching for this word alone
## Local Dictionary Encoding

* the rank in the local dictionary search + 1 (with an extra 1 after the initial 0s)

## Standard Encoding

* the length of the word (without an extra 1 after the initial 0s)
* the set of individual characters (without an extra 1 after the initial 0s)

“10” to indicate that the set of phrases is done

A set of the number of spaces: reading the decoded string so far left-to-right, char-by-char 

* the number of spaces at the beginning of the text + 1 is stored (without an extra 1 after the initial 0s)
* for each space encountered, the number of spaces at that point + 1 is stored in the binary string (without an extra 1 after the initial 0s)
* for each period encountered, the number of spaces immediately before it + 1 and after it + 1 are stored (without an extra 1 after the initial 0s)
* the number of spaces at the end of the text + 1 is stored (without an extra 1 after the initial 0s)

(note: for all of these, once the number of spaces is stored, all multiple spaces are deleted)

A bit vector for commas, periods and upper-case letters: 

* a “0” indicates this is character is as left as it is
* a “1” with a letter as the corresponding char in the text indicates this letter should be upper case
* a “1” with a space as the corresponding char in the text indicates this char should be a comma or a period; the next bit indicates which one: “10” for comma and “11” for period

(note: for all of these, once the indicator bit(s) is stored, commas + periods are made into spaces and upper-case letters into lower case letters, in the encoding phase)

For all individual chars encoded, they are encoded as follows:
Normal:
```
‘a’ – 1
‘b’ – 2
…
‘z’ – 26
‘ ‘ - 27
```
OR
Based on frequencies (see wordclass.cc)
