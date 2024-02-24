[video](https://youtu.be/zduSFxRajkE)

## Unicode
- utf-8, utf-16, utf-32
- used to define characters
- ord("h") to get the unicode codepoint of "h" (chr(any number) is the reverse)
- cannot use it as tokenization as is because the unicode encoding keep on changing
- utf-8 is more common
- utf-32 is fixed length instead of variable length
- [blog post 1](https://www.reedbeta.com/blog/programmers-intro-to-unicode/) 
- utf-8 is the only one backward compatible to ascii encodings
- string class in python has `.encode('utf-8)`.
- utf-16 and utf-32 has many "wasteful" encodings.
- MegaByte - [paper showcasing feeding raw bytes into model](https://arxiv.org/abs/2305.07185)

## Byte-Pair Encoding Algorithm
- [wiki](https://en.wikipedia.org/wiki/Byte_pair_encoding)
- we identify the most repeated pair of chars in a sequence and then replace it with another single char, repeat till every pair of chars is unique.
- 