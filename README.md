# Devanagari Script Tokenizer

A specialized tokenizer for Devanagari script (used in Hindi and other Indian languages) based on Byte-Pair Encoding (BPE). This tokenizer is designed to efficiently handle Devanagari text while preserving the linguistic structure of the script.

## What is a Tokenizer?

A tokenizer is a tool that breaks down text into smaller units called tokens. These tokens can be words, subwords, or characters. The main purposes of tokenization are:
1. To convert text into a format that machine learning models can process
2. To reduce vocabulary size while maintaining meaning
3. To achieve efficient text compression

## How This Tokenizer Works

The tokenizer uses Byte-Pair Encoding (BPE), which:
1. Initially represents text as individual bytes
2. Iteratively merges the most frequent pairs of adjacent tokens
3. Creates a vocabulary of subword units
4. Provides efficient compression while maintaining meaningful units

## Devanagari Script Pattern

The tokenizer uses a specialized regex pattern (`DEVANAGARI_SPLIT_PATTERN`) to handle Devanagari script:

```python
DEVANAGARI_SPLIT_PATTERN = r"""'s|'t|'re|'ve|'m|'ll|'d| ?\p{N}+| ?(?:[\u0904-\u0939\u093d-\u093d\u0950-\u0950\u0958-\u0961\u0970-\u097f\ua8f2-\ua8fe\U00011b00-\U00011b09\u1cd3-\u1cd3\u1ce9-\u1cec\u1cee-\u1cf3\u1cf5-\u1cf6\u1cfa-\u1cfa][\u0900-\u0903\u093a-\u093c\u093e-\u094f\u0951-\u0957\u0962-\u0963\ua8e0-\ua8f1\ua8ff-\ua8ff\u1cd0-\u1cd2\u1cd4-\u1ce8\u1ced-\u1ced\u1cf4-\u1cf4\u1cf7-\u1cf9]*)+| ?\p{L}+| ?[^\s\p{L}\p{N}]+|\s+(?!\S)|\s+"""
```

This pattern is broken down into several components:

1. Common English contractions: `'s|'t|'re|'ve|'m|'ll|'d`

2. Numbers: `?\p{N}+`

3. Devanagari characters:
   - Base characters (consonants): `\u0904-\u0939`
     - Examples: क ख ग घ ङ च छ ज झ ञ ट ठ ड ढ ण त थ द ध न प फ ब भ म य र ल व श ष स ह
   - Special characters: `\u093d-\u093d`, `\u0950-\u0950`
     - Examples: ऽ ॐ
   - Additional consonants: `\u0958-\u0961`
     - Examples: क़ ख़ ग़ ज़ ड़ ढ़ फ़ य़
   - Digits and special symbols: `\u0970-\u097f`
     - Examples: ० १ २ ३ ४ ५ ६ ७ ८ ९ ॰ ॱ ॲ ॳ ॴ ॵ
   - Extended range characters: `\ua8f2-\ua8fe`, `\U00011b00-\U00011b09`
     - Examples: ꣲ ꣳ ꣴ ꣵ (rare combinations used in Sanskrit texts)

4. Devanagari modifiers (matras):
   - Vowel marks: `\u0900-\u0903`, `\u093a-\u093c`, `\u093e-\u094f`
     - Examples: ा ि ी ु ू ृ ॄ ॅ े ै ो ौ ं ः ँ
   - Stress marks and accents: `\u0951-\u0957`
     - Examples: ॑ ॒ ॓ ॔
   - Additional vowel marks: `\u0962-\u0963`
     - Examples: ॢ ॣ

5. Other patterns:
   - Latin script: `?\p{L}+`
   - Non-space characters: `?[^\s\p{L}\p{N}]+`
   - Whitespace: `\s+(?!\S)|\s+`

## Features

- Preserves Devanagari character combinations (consonants with vowel marks)
- Handles mixed script text (Devanagari with English)
- Provides compression ratio metrics
- Supports both encoding (text to tokens) and decoding (tokens to text)
- Interactive Gradio interface for demonstration

## Usage

1. Encoding text to tokens:
```python
tokenizer = DevanagariTokenizer()
tokenizer.load("hindi.model")
tokens = tokenizer.encode("your_text_here")
```

2. Decoding tokens back to text:
```python
decoded_text = tokenizer.decode(tokens)
```


## Training

The tokenizer can be trained on new text data:
```python
tokenizer = DevanagariTokenizer()
tokenizer.train(text, vocab_size=32000)
tokenizer.save("model_name.model")
```

The training process:
1. Splits text using the Devanagari pattern
2. Converts text chunks to UTF-8 bytes
3. Iteratively merges most frequent byte pairs
4. Creates a vocabulary of subword units
5. Saves the model for later use 

## Demo

Hugging face app: [Tokenizer_Hindi](https://huggingface.co/spaces/walnash/Tokenizer_Hindi)
