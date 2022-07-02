# rime-cantonese: A Standardized Cantonese Lexicon in Jyutping

Authors: Mingfei Lau, Muhan Zhong, Jian Su, Henry Chan, Chaak-ming Lau

Language: This lexicon is solely in Standard Cantonese, the ISO 693-3 code is `yue`

Recommended/Expected use of corpus: Input Method (IME) lexicon, Transliteration tool, Automatic Speech Recognition (ASR) phone lexicon, Text-to-Speech (TTS) phone lexicon, Dictionary.

# Collection Procedure - format, method, and timespan

The data of rime-cantonese is divided into two parts: single character entries (char entries) and multi-character word/phrase entries (word entries). Char entries are stored in `char.csv` and word entries are stored `word.csv` and `phrase_fragment.csv`. Below are their introductions, respectively.

## Single character entries (char entries)

### Data source

The main source of char entries is the [Cantonese Pronunciation List of the Characters for Computers](https://github.com/lshk-org/jyutping-table) by LSHK. However, this table collects all character variants, including Simplified Chinese variants. We remove these variants, and only keep those OpenCC standard variants. To ensure the correctness of pronunciation and the comprehensiveness of character collection, we supplemented this list with the following sources:

1. Unihan kCantonese database from Unicode 13.0. [Source](https://unicode.org/reports/tr38/)
1. Chinese Character Database: With Word-formations, Phonologically Disambiguated According to the Cantonese Dialect. [Source](https://humanum.arts.cuhk.edu.hk/Lexis/lexi-can/)
1. jyut.net (_Cantonese pronunciation materials summa_, this site is a collection of multiple dictionaries) [Source](https://jyut.net/)

If there are characters that are missing in the main source yet collected in the supplementary sources, we include them in our lexicon, and normalize them based on our guidelines, which are detailed in the following sub-sections.

### About simplified Chinese characters

While simplified characters work well with Mandarin Chinese, they may lead to ambiguities and errors in Cantonese. In the Simplified Character Scheme, some homophonous characters in Mandarin are all represented with one single character, while they are non-homophonous in Cantonese. This results in a significant amount of homographs, i.e., characters with ambiguous meaning and pronunciations when they are used to write non-Mandarin languages. For example, 松 is pronounced cung4 in Cantonese, meaning “pine woods”, while 鬆 is pronounced sung1, meaning “loose” or “untie”. The Character Simplification Scheme merges these two characters and creates a one-to-many mappings between the code point 松 and two Cantonese morphemes, i.e. a homograph. To avoid the trouble of homograph disambiguation in future applications, rime-cantonese is designed to reduce such mergers as much as possible. Therefore, the lexicon does **not include any simplified characters** introduced in the Character Simplification Scheme. However, the lexicon does collect other character variants, such as Japanese Shinjitai characters and Vietnamese ChuNom.

### About Chinese character variants

Besides simplified characters, the issue of character variants also needs to be addressed (note that this is different from orthographic character choice, which we will discuss in the the word entries section). To ensure the consistency while keeping complexities as low as possible, rime-cantonese takes advantage of an open-source library called Open Chinese Convert (OpenCC), which supports two-way conversions between different regional variants of Chinese characters. OpenCC maintains a standard for all Chinese character variants (we will call it “the OpenCC standard” or “OpenCC variants” from now on). The OpenCC standard character list keeps the most semantic contrasts between code points and servers as the baseline character variants. And with this baseline, the library can convert characters between Mainland Simplified, Hong Kong traditional, Taiwan traditional variants with this standard. We recommend accompanying the OpenCC library when using rime-cantonese in applications. For instance, when building a Cantonese transliteration module, one can generate the missing Simplified Chinese variants first with OpenCC in order to get a comprehensive character-to-pronunciation mapping list.

## Multi-character word and phrase entries (word entries)

## Data source

There are multiple data sources of word entries. The majority of the entries are imported from these online/physical resources:

1. Words entries from words.hk
1. Curated word list from hambaanglaang.hk
1. Selected entries from the four dictionaries:
   1. _實用廣州話分類詞典_
   1. _A Dictionary of Cantonese Slang_
   1. _廣州話詞典_
   1. _地道廣州話用語_
1. Selected entries from rime-cantonese users' donated user dictionaries

"Selected entries" means that we manually curated the sources entries by subjective judgements, removing the uncommon or junk words and retain only the common and sensible words in daily usage. This help reduce the size the redundancy of the lexicon.

### About character variant choice

Different from char entries, which include all character variants except Simplified Chinese variants, all word entries characters are store in the form of OpenCC standard variants. One can convert the characters into Hong Kong, Taiwan or Mainland Simplified variants with OpenCC based on the use case.

### About orthographic choices of characters

Since Cantonese doesn't have an official-level orthography, rime-cantonese is aimed at promoting the standardization of written Cantonese, where the orthography of word entries are all standardized. The Cantonese community has various orthographic representations for a same linguistic unit, and they are widely used in. For example, bei2 meaning “to give; passive marker” has three common written forms: 畀, 俾 and 比. This creates unnecessary redundancies and ambiguities which may hinder the standardization process, and also increases the cost of data preprocessing in NLP tasks. To tackle this problem, we prescribe a list of recommended writing forms and convert all collected words according to this standard. The full list can be found in [Common typos in Cantonese](https://jyutping.org/en/blog/typo/). In principle, we try to map one character to one morpheme, to reduce redundancy or ambiguities.

Cantonese is known for its abundant sentence-final particles, and the Cantonese community currently doesn’t have a consistent way of writing them. Luke (2007) compiled a list of recommended written form for Cantonese sentence particles. We made some adjustments to it to keep necessary contrasts. The full list can be found in [Cantonese Sentence Particles](https://jyutping.org/en/blog/particles/)

## Directory Structure & File Format Specific Details

The corpus has three csv files: `char.csv`, `word.csv` and `phrase_fragment.csv`. All files contain two columns:

- `char`: The Chinese character(s) of this lexical entry
- `jyutping`: The Jyutping pronunciation of this lexical entry

Note that we separate the multi-character word entries into two files, `word.csv` and `phrase_fragementcsv`. This is for the convenience of different applications. `phrase_fragementcsv.csv` contains multi-character entries that are phrases, sentences, or sentence fragments, which are usually longer in length than entries in `word.csv`.

All data is stored in UTF-8 encodings. There are currently ~34k entries in `char.csv`, ~95k entries in `word.csv` and ~8k entries in `phrase_fragment.csv`

rime-cantonese is also open sourced on GitHub atuhttps://github.com/CanCLID/rime-cantonese-upstream. It has a downstream version which is reformatted as to serve as an Input Method (IME) database, link: https://github.com/rime/rime-cantonese.

## Checksums

- MD5 (.//phrase_fragment.csv) = 64f1e64402f717de62990e47d2aa8ce7
- MD5 (.//char.csv) = fd22025d6789f60a0e473124f0bdc21b
- MD5 (.//word.csv) = 3daf6a554e0595ced71d8555a47007d6
- MD5 (.//LDC.md) = e1fe4b09526317e06d80dbf7934d4e15
- MD5 (.//md5s.txt) = d41d8cd98f00b204e9800998ecf8427e
