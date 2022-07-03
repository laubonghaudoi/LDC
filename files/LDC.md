# rime-cantonese: A Standardized Cantonese Lexicon in Jyutping

Authors: Mingfei Lau, Muhan Zhong, Jian Su, Henry Chan, Chaak-ming Lau

Language: This lexicon is solely in Contemporary Cantonese, the ISO 693-3 code is `yue`

Recommended/Expected use of corpus: Input Method (IME) lexicon, Transliteration tool, Automatic Speech Recognition (ASR) phone lexicon, Text-to-Speech (TTS) phone lexicon, Dictionary.

# Collection Procedure - format, method, and timespan

The data of rime-cantonese is divided into two parts: single-character entries (char entries) and multi-character entries (word entries). Char entries are stored in `char.csv`, while word entries are stored `word.csv` and `phrase_fragment.csv`. Below are their introductions, respectively.

## Single-character entries (char entries)

### Data source

The primary source of char entries is the [Cantonese Pronunciation List of the Characters for Computers](https://github.com/lshk-org/jyutping-table) by LSHK. However, in addition to those characters that comply with the OpenCC standard, the original list also contains simplified Chinese characters and variant Chinese characters, which were removed when we created our lexicon.

To ensure the correctness of pronunciation and the comprehensiveness of character collection, we supplemented this list with the following sources:

1. Unihan kCantonese database from Unicode 13.0. [Source](https://unicode.org/reports/tr38/)
1. Chinese Character Database: With Word-formations, Phonologically Disambiguated According to the Cantonese Dialect. [Source](https://humanum.arts.cuhk.edu.hk/Lexis/lexi-can/)
1. jyut.net (_Cantonese pronunciation materials summa_, this site is a collection of multiple dictionaries) [Source](https://jyut.net/)

For characters that are missing in the primary source yet collected in the supplementary sources, we normalize them before including them in our lexicon, based on the guidelines described in the following sub-sections.

### About simplified Chinese characters

The lexicon **does not include any simplified characters** introduced in the Character Simplification Scheme for the following reasons.

The conversion from traditional Chinese to simplified Chinese is technically more accurate than that from simplified Chinese to traditional Chinese because traditional Chinese characters are less ambiguous. For example, the traditional Chinese characters 發 (“distribute; prosper”) and 髮 (“hair”) are both written as 发 in simplified Chinese. Therefore, when converting the character 发 from simplified Chinese to traditional Chinese, one must determine the correct character based on the context, which may result in a loss of accuracy.

Besides, simplified Chinese characters are more tailored to Mandarin Chinese than to Cantonese because some Chinese characters are simplified by replacing their phonetic components with simpler components that are homophonous in Mandarin, whereas they are not necessarily homophonous in Cantonese. For example, the traditional Chinese character 劇 is simplified as 剧 because the new phonetic component 居 (pronunced jū in Mandarin) sounds similar to the original character 劇 (pronunced jù in Mandarin). However, the two characters are pronunced kek6 and geoi1 in Cantonese, respectively, which are totally different. Therefore, some simplified characters may cause confusion and trouble for Cantonese users.

However, the lexicon does collect other character variants, such as Japanese Shinjitai characters and Vietnamese Chữ Nôm for ease of retrieval and input.

### About variants Chinese characters

Besides simplified characters, the issue of character variants also needs to be addressed (note that this is different from orthographic character choice, which we will discuss in the word entries section). To ensure the consistency while keeping complexities as low as possible, rime-cantonese takes advantage of Open Chinese Convert (OpenCC), an open-source library which supports multi-way conversions between different regional variants of Chinese characters. OpenCC maintains a standard for all Chinese character variants (we will call it “the OpenCC standard” or “OpenCC variants” from now on). The OpenCC standard character list keeps the most semantic contrasts between code points and servers as the baseline character variants. And with this baseline, the library can convert characters between Mainland Simplified, Hong Kong traditional, Taiwan traditional variants with this standard. We recommend accompanying the OpenCC library when using rime-cantonese in applications. For instance, when building a Cantonese transliteration module, one can generate the missing Simplified Chinese variants first with OpenCC in order to get a comprehensive character-to-pronunciation mapping list.

## Multi-character entries (word entries)

## Data source

There are multiple data sources of word entries. The majority of the entries are imported from these online/physical resources:

1. Words entries from words.hk
1. Curated word list from hambaanglaang.hk
1. Selected entries from the four dictionaries:
   1. 實用廣州話分類詞典
   1. _A Dictionary of Cantonese Slang_
   1. 廣州話詞典
   1. 地道廣州話用語
1. Selected entries from rime-cantonese users' donated user dictionaries

“Selected entries” means that we removed ill-formed or uncommon words based on subjective judgement and retained only the common and sensible words in daily usage. This helped to reduce the size the redundancy of the lexicon.

### About character variant choice

Different from char entries, which include all character variants except Simplified Chinese variants, all word entries characters are store in the form of OpenCC standard variants. One can convert the characters into Hong Kong, Taiwan or Mainland China variants with the OpenCC program based on the use case.

### About orthographic choices of characters

Since Cantonese doesn't have an official-level orthography, rime-cantonese is aimed at promoting the standardization of written Cantonese, where the orthography of word entries are all standardized. The Cantonese community has various orthographic representations for a same linguistic unit, and they are widely used in. For example, bei2 meaning “to give; passive marker” has three common written forms: 畀, 俾 and 比. This creates unnecessary redundancies and ambiguities which may hinder the standardization process, and also increases the cost of data preprocessing in NLP tasks. To tackle this problem, we prescribe a list of recommended writing forms and convert all collected words according to this standard. The full list can be found in [Common typos in Cantonese](https://jyutping.org/en/blog/typo/). In principle, we try to map one character to one morpheme, to reduce redundancy or ambiguities.

Cantonese is known for its abundant sentence-final particles, and the Cantonese community currently doesn’t have a consistent way of writing them. Luke (2007) compiled a list of recommended written form for Cantonese sentence particles. We made some adjustments to it to keep necessary contrasts. The full list can be found in [Cantonese Sentence Particles](https://jyutping.org/en/blog/particles/).

## Directory Structure & File Format Specific Details

The corpus has three csv files: `char.csv`, `word.csv` and `phrase_fragment.csv`. All files contain two columns:

- `char`: The Chinese character(s) of this lexical entry
- `jyutping`: The Jyutping pronunciation of this lexical entry

Note that we separate the multi-character word entries into two files, `word.csv` and `phrase_fragementcsv`. This is for the convenience of different applications. `phrase_fragementcsv.csv` contains multi-character entries that are phrases, sentences, or sentence fragments, which are usually longer in length than entries in `word.csv`.

All data is stored in UTF-8 encodings. There are currently ~34k entries in `char.csv`, ~95k entries in `word.csv` and ~8k entries in `phrase_fragment.csv`

rime-cantonese is also open sourced on GitHub at https://github.com/CanCLID/rime-cantonese-upstream. It has a downstream version which is reformatted as to serve as an Input Method (IME) database, link: https://github.com/rime/rime-cantonese.

## Checksums

- MD5 (.//phrase_fragment.csv) = 64f1e64402f717de62990e47d2aa8ce7
- MD5 (.//char.csv) = fd22025d6789f60a0e473124f0bdc21b
- MD5 (.//word.csv) = 3daf6a554e0595ced71d8555a47007d6
