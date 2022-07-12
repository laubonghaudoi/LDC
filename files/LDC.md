# rime-cantonese: A Standardized Cantonese Lexicon in Jyutping

**Authors:** Mingfei Lau, Muhan Zhong, Jian Su, Henry Chan, Chaak-ming Lau

**Language:** This lexicon is solely in contemporary Cantonese. The ISO 693-3 code is `yue`

**Recommended/Expected use of corpus:** Input Method (IME) lexicon, Transliteration tool, Automatic Speech Recognition (ASR) lexicon, Text-to-Speech (TTS) lexicon, dictionary.

## Collection Procedure – Format, Method, and Timespan

The data of rime-cantonese consists of two parts: single-character entries (`char` entries) and multi-character entries (`word` entries). The `char` entries are stored in `char.csv`, while the `word` entries are stored `word.csv` and `phrase_fragment.csv`. Below are the descriptions of the two parts.

### Single-character Entries (`char` Entries)

#### Data Source

The primary source of char entries is the [Cantonese Pronunciation List of the Characters for Computers](https://github.com/lshk-org/jyutping-table) by LSHK. However, in addition to those characters that comply with the OpenCC standard, the original list also contains simplified Chinese characters and variant Chinese characters, which were removed when we created our lexicon.

To ensure the correctness of pronunciation and the comprehensiveness of character collection, we supplemented this list with the following sources:

1. Unihan kCantonese database from Unicode 13.0. [Source](https://unicode.org/reports/tr38/)
1. Chinese Character Database: With Word-formations, Phonologically Disambiguated According to the Cantonese Dialect. [Source](https://humanum.arts.cuhk.edu.hk/Lexis/lexi-can/)
1. jyut.net (_Cantonese pronunciation materials summa_, this site is a collection of multiple dictionaries) [Source](https://jyut.net/)

For characters that are missing from the primary source yet present in the supplementary sources, we normalize them before including them in our lexicon, based on the guidelines described in the following sub-sections.

#### Regarding Simplified Chinese

Chinese texts can be written in either Traditional or Simplified Chinese. To avoid data redundancy, the lexicon should contain only one variant. Specifically, the lexicon contains Traditional Chinese and does not contain any simplified Chinese characters for the following reasons.

Technically speaking, the conversion from traditional Chinese to simplified Chinese is more accurate than that in the opposite direction, as traditional Chinese characters are less ambiguous. For example, the traditional Chinese characters 發 (“distribute; prosper”) and 髮 (“hair”) are both written as 发 in simplified Chinese. Therefore, when converting the character 发 from simplified Chinese to traditional Chinese, one must determine the correct character based on the context, which may result in a loss of accuracy. In contrast, one can convert traditional Chinese to simplified Chinese using the Open Chinese Convert (OpenCC) program with an accuracy of nearly 100%.

Simplified Chinese characters are more tailored to Mandarin Chinese than to Cantonese because some Chinese characters are simplified by replacing their phonetic components with simpler components that are homophonous in Mandarin, whereas they are not necessarily homophonous in Cantonese. For example, the traditional Chinese character 劇 is simplified as 剧 because the new phonetic component 居 (pronunced jū in Mandarin) sounds similar to the original character 劇 (pronunced jù in Mandarin). However, in Cantonese, the two characters are pronunced kek6 and geoi1, which are completely different. Therefore, some simplified characters may cause confusion and problems for Cantonese users.

#### Regarding Regional Variants of Traditional Chinese

In addition to the difference between traditional and simplified Chinese, traditional Chinese itself differs in different eras, regions and standards (note that this is different from the choice of orthographic characters, which will be discussed in the `word` entries section). For example, the character leoi5 (“inside”) is written as 裏 in Hong Kong and 裡 in Taiwan. Storing all variants in the lexicon would result in data redundancy.

To address this, we have adopted the OpenCC Traditional Chinese standard in our lexicon. The standard is designed for conversion between Chinese variants, which means that when Chinese text is written in the OpenCC standard, the conversion to Hong Kong and Taiwan variants will achieve the highest accuracy.

We recommend accompanying the OpenCC library when using rime-cantonese in applications. For instance, when building a Cantonese transliteration module, one can generate the missing simplified Chinese variants first with OpenCC in order to get a comprehensive character-to-pronunciation mapping list.

#### Regarding Variants Chinese Characters

Variant Chinese characters are Chinese characters that are homophones and synonyms. For example, 亓 is a variant character of 其 (“its”). These Chinese characters are not actively used in modern times. Thus, variant characters are included only in char entries and not in word entries. Particularly, some Japanese Shinjitai characters and Vietnamese Chữ Nôm are also included because they are more commonly found in daily life. For example, 円 is the Japanese variant for 圓, while it is commonly used to describe the currency unit of Japan.

### Multi-character Entries (`word` Entries)

#### Data Source

There are multiple data sources of word entries. The majority of the entries are imported from these online/physical resources:

1. Words entries from [words.hk](https://words.hk/)
1. Curated word list from [hambaanglaang.hk](https://hambaanglaang.hk/)
1. Selected entries from the four dictionaries:
   1. 實用廣州話分類詞典
   1. _A Dictionary of Cantonese Slang_
   1. 廣州話詞典
   1. 地道廣州話用語
1. Selected entries from rime-cantonese users’ donated user dictionaries

“Selected entries” means that we removed incorrect or uncommon words based on subjective judgement and retained only the common and sensible words in daily usage. This helped to reduce the size the redundancy of the lexicon.

#### Regarding Orthographic Choices of Characters

As no official-level orthography exists for Cantonese, we aim to promote the standardization of written Cantonese through our lexicon, in which the orthography of `word` entries are standardized. Due to the lack of standardization, the Cantonese community uses various orthographic representations for the same linguistic unit. For example, bei2 meaning “to give; passive marker” has three common written forms: 畀, 俾 and 比. This creates redundancies and ambiguities and also increases the cost of data preprocessing in NLP tasks. To tackle this problem, we prescribe a list of recommended writing forms and convert all collected words according to this standard. The full list can be found in [Common typos in Cantonese](https://jyutping.org/en/blog/typo/). In principle, we try to map one character to one morpheme, to reduce redundancy or ambiguities.

Cantonese is known for its abundant sentence-final particles, and the Cantonese community currently doesn’t have a consistent way of writing them. Luke (2007) compiled a list of recommended written form for Cantonese sentence particles. We made some adjustments to it to keep necessary contrasts. The full list can be found in [Cantonese Sentence Particles](https://jyutping.org/en/blog/particles/).

## Directory Structure & File Format Specific Details

The corpus consists of three CSV files: `char.csv`, `word.csv` and `phrase_fragment.csv`. All files contain two columns:

- `char`: The Chinese character(s) of this lexical entry
- `jyutping`: The pronunciation in Jyutping of this lexical entry

Note that we separate the multi-character word entries into two files, `word.csv` and `phrase_fragement.csv`. This is for the convenience of different applications. `phrase_fragement.csv` contains multi-character entries that are phrases, sentences, or sentence fragments, which are usually longer in length than entries in `word.csv`.

All data is stored in the UTF-8 encoding. There are currently ~34k entries in `char.csv`, ~95k entries in `word.csv` and ~8k entries in `phrase_fragment.csv`.

rime-cantonese is also open sourced on GitHub at <https://github.com/CanCLID/rime-cantonese-upstream>. There is also a downstream version that is reformatted to serve as an Input Method (IME) database at <https://github.com/rime/rime-cantonese>.
