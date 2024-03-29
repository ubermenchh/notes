[video](https://youtu.be/zduSFxRajkE) | [colab](https://colab.research.google.com/drive/1y0KnCFZvGVf_odSfcNAws6kcDD7HsI0L?usp=sharing) | [repo](https://github.com/karpathy/minbpe)

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

### Tokenizer
- it is a encoder-decoder 
- it is trained just like a model
- training set for tokenizer is seperate than that of the model.

[gpt2 paper](https://d4mucfpksywv.cloudfront.net/better-language-models/   language_models_are_unsupervised_multitask_learners.pdf)

### regex pattern
- `\p{L}` -> matches any letter (\p{Letter})
- `\p{N}` -> matches any number (\p{Number})
- `\s`    -> matches whitespace

## Tiktoken lib
- `pip install tiktoken`
- check different gpt tokenizers [here](https://github.com/openai/tiktoken/blob/main/tiktoken_ext/openai_public.py)

## GPT2 code
- [encoder.py](https://github.com/openai/gpt-2/blob/master/src/encoder.py)
- `im` in `<|im_start|>` is imaginary monologue

## sentencepiece ([repo](https://github.com/google/sentencepiece))
- tiktoken encodes to utf-8 and then BPEs bytes
- sentencepiece BPEs the code points and optionally falls back to utf-8 bytes for rare code point, which are then translated to byte tokens.

#### vocab_size
- too big vocab_size will lead to undertraining as a large number of tokens will show up rarely.
- too small vocab_size, the tokens will get squished into one many large chunks which will not be able to provide sufficient info to the model

[paper introducing new tokens](https://arxiv.org/abs/2304.08467) 