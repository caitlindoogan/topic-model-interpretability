> This repository contains information about the datasets and pre-processing methods accompanying the [paper](https://www.aclweb.org/anthology/2021.naacl-main.300.pdf).

## Table of Contents
* [General Information](#general-information)
* [Datasets](#datasets)
* [About the data](#about-data)
* [Pre-processing overview](#pre-processing)
* [Setting up](#setting-up)
* [Project Status](#project-status)
* [Acknowledgements](#acknowledgements)
* [Contact](#contact)
* [Citations](#citation)

## General Information
The Auspol1819 dataset and its subset, the Auspol18 dataset, were created to examine the reliability of coherence scores to measure topic model interpretability. The data has been published here to enable reproducibility and for use in future research. 
We encourage using these datasets for those developing new short-text topic models intended for the analysis of social media data.

### Rational
Researchers in various fields, including public health, media and communications, and political sciences, have found social media to be an appealing source of data. Topic modelling is now a standard tool for assisting researchers working with large amounts of social media data. The method's popularity stems from the in-depth insights which would be unable to be achieved using manual alternatives. As a result, there is an increased appetite for computational methods topic models capable of producing highly informative topics from challenging social media datasets. 
We aimed to evaluate topic interpretability in a way that was representative of the methods used in applied research. That is, how researchers would interpret topics in real-world settings. 

Tweets containing #Auspol were chosen to form the dataset for the analysis of coherence scores and broader understanding of topic model interpretability for the following reasons:
1. To enable evaluation of the influence of small language changes on coherence score evaluation. This was because Australian English is similar to British and American English but with specific characteristics that introduce mismatches with common benchmark corpora. Specifically, we wanted to see how calculating coherence scores (NPMI)  on a similar language (in our case, a large corpus of news stories from the Australian Broadcasting Corporation) affected these results.
2. The population creating tweets using the hashtag #Auspol is fairly homogeneous. Based on the literature, we anticipated that the great majority of tweets were written in Australian English.
3. The discussion of Australian politics on #Auspol is highly topical, event-driven, and closely linked to the news cycle. As a result, confirming the legitimacy of topics was assisted by news media, which was used as a point of external validation. 

## Datasets
- `datasets`: Containing the Auspol18, Auspol1819 datasets, and their length controlled counterparts.
  - `datasets/auspol1819_ids`: Length controlled Auspol1819 dataset.
  - `datasets/auspol18_ids`: The Auspol18 dataset.
  - `datasets/auspol1819_ids_min-4-tokens`: Length controlled Auspol1819 dataset, minimum of 4 tokens.
  - `datasets/auspol18_ids_min-4-tokensjson`: Length controlled Auspol18 dataset, minimum of 4 tokens.
  - `datasets/auspol1819_ids_min-10-tokens`: Length controlled Auspol1819 dataset, minimum of 10 tokens.
  - `datasets/auspol18_ids_min-10-tokensjson`: Length controlled Auspol18 dataset, minimum of 10 tokens.
  - `datasets/top-30-mentions.txt`: A list of the top 30 mentions in tweets from Auspol1819. 
  - `datasets/top-30-hashtags.txt`: A list of the top 30 hashtags in tweets from Auspol1819.
  - `datasets/top-50-mentions.txt`: A list of the top 50 mentions in tweets from Auspol1819. 
  - `datasets/top-50-hashtags.txt`: A list of the top 50 hashtags in tweets from Auspol1819. 

## About the data
The datasets are derived from tweets with the hashtag #Auspol, a longstanding hashtag used to discuss Australian politics. 
 
| Detail | Auspol1819 | Auspol18 |
| ------ | ------- | -------- |
| No. of Tweets            | 1,554,570 | 304,923 |
| Date range (UTC)    | Jan 13 2018 - Jan 25 2019 | Jan 12 3018 - Aug 9 2018 |
| Date range (AEST) | Jan 13 2018 - Jan 26 2019  | Jan 12 3018 - Aug 10 2018 |
| Unique users                | 65,178 | 18,650 |

### Controlled length datasets
As the length of documents in both Auspol1819 and Auspol18 were incredibly short, we opted to control the length of the aupol18 dataset prior to topic modelling.

Documents included in the analysis were restricted to those with four or more tokens. 

Additionally, we provide versions of the auspol19 and auspol18 datasets containing documents with ten or more tokens. The document lengths are shown below.

| Any Length   |   Auspol1819   |   Auspol18   | Min 4 tokens  | Auspol1819   |   Auspol18      | Min 10 tokens  | Auspol1819   |   Auspol18   |
| ------------ | -------------  |  ----------- | -------------  | ------------ | -------------  | -------------  | ------------ | -------------|
| No. Tweets   | 1,554,570      | 304,923      | No. Tweets     | 1,392,544    | 260,628        | No. Tweets     | 862,502      | 36,535       |
| Minimum      | 1              | 1            | Minimum        | 4            | 4              | Minimum        | 10           | 10           |
| Maximum      | 67             | 42           | Maximum        | 67           | 42             | Maximum        | 67           | 42           |
| Average      | 11.67          | 6.37         | Average        | 12           | 7.02           | Average        | 16.50        | 11.21        |
| Median       | 11             | 6            | Median         | 12           | 17             | Median         | 16           | 11           |        

## Pre-processing overview
Exhaustive pre-processing was undertaken before topic modelling. These pre-processing steps are briefly:

> **Removed from text:**	
- Emojis (and emoticons) 
- URLs, including links for pictures, gifs and other embedded media. 
- Mentions and hashtags 
- Punctuation, whitespaces, numbers and special characters
- One letter tokens
- 'Auspol'
- Non-ASCII characters
- Stopwords
- Infrequent tokens (n>10)

> **Exclusion of tweets that were:**
- Not written in English 
- Empty after cleaning
- Duplicates
- Retweets
- Had less than four tokens

> **Normalisation:**
- Contraction expansion
- Conversion of American English to Australian English
- Collapsed elongated words
- Replacing accented characters
- POS-tagging
- Tokenisation
- Lemmatisation
- Conversion (slang, abbreviations, common spelling errors)
- Manual Bigram selection (Bi-grams with a PMI > 5 were considered)

 Note: We did not remove high-frequency terms above a certain threshold, e.g. top 5% of tokens, as we had normalised terms.

### Varient construction
> Four versions of the auspol18 dataset were constructed:
- **AWH:** contains the 30 most frequent hashtags (excluding #Auspol).
- **AWM:** contains the 30 most frequent mentions of verified accounts. 
- **AWMH:** contains the 30 most frequent hashtags and 30 most frequent mentions of verified accounts.
- **AP:** contains neither hashtags nor mentions.

>The top 30 hashtags and mentions are available in `auspol18-top-30-hashtags.txt` and `auspol18-top-30-mentions.txt`. 
> Additionally, we have included the top 50 hashtags and mentions as separate files (`auspol18-top-50-hashtags.txt` and `auspol18-top-50-mentions.txt`) should you wish to experiment with the effects of additional metadata. 

## Setting up 
As per the Twitter developers policy, the content of tweets cannot be shared freely. Instead, tweet ids can be provided for re-hydration.
Please extract the tweets for the ids in `dataset/auspol1819_ids[_min-x-tokens].json` or `dataset/auspol18_ids[_min-x-tokens].json`. 

### 1. Obtaining Twitter developer API access
>You will need to register your application for a Twitter developer API to obtain the necessary access passes. 
>You can apply for access [here](https://developer.twitter.com/en/apply-for-access).

> You will need to obtain the following:
- 1. consumer_key
- 2. consumer_secret
- 3. access_token
- 4. access_token_secret

### 2. Setting up the codebase and the dependencies for re-hydration
- Clone this repository.
- We recommend [Twarc](https://github.com/DocNow/twarc) for re-hydration.
- Install the following dependencies for Twarc.
> 
| Dependency                  | Version | Installation Command                                                |
| ----------                  | ------- | ------------------------------------------------------------------- |
| Python                      | 3.8.5   | `conda create --name stance python=3.8` and `conda activate stance` |
| Twarc                       | 2.0.13  | `pip install twarc==2.0.13`     |
| Tweepy                      | 3.13.0  | `pip install tweepy==3.13.0`    |
| argparse                    | 1.4.0   | `pip install argparse==1.4.0`   |
| python-magic                | 0.4.22  | `pip install pythonmagic==0.4.22`   |
| xtract                      | 0.1a3   | `pip install xtract==0.1a.3`    |
| wget                        | 3.2.0   | `pip install wget==3.2.0`       |
| wandb                       | 0.9.4   | `pip install wandb==0.9.4`      |

### 3. Re-hydrating tweets
- Re-hydrate the tweets.
- Save all the tweets in a single folder, <dataset>.json. 
>Juan Banda @jmbanda has provided an informative [_tutorial_](https://github.com/thepanacealab/covid19_twitter/blob/master/COVID_19_dataset_Tutorial.ipynb) on this from the Pancealab COVID-19 twitter project. 

## Project Status
Project is: _in progress_ 

## Acknowledgements
>Many thanks to Callum Waugh, who contributed to the conceptualisation of this project and participated in topic labelling. Thank you to Elizabeth Daniels and Elliot Freeman, who also contributed to topic labelling.

## Contact
**Caitlin Dooogan** 
- **Email:** caitlin.doogan@monash.edu
- **Twitter:** [@CaitDoogan](https://twitter.com/CaitDoogan?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor)
- **Github:** [@caitlindoogan](https://github.com/caitlindoogan) 

**Wray Buntine** 
- **Email:** wray.buntine@monash.edu
- **Twitter:** [@wraylb](https://twitter.com/wraylb?lang=en)
- **Github:** [@wbuntine](https://github.com/wbuntine)

>Feel free to contact us!

## Citation
>Authors: Caitlin Doogan and Wray Buntine
>Please Cite our paper if you find the datasets useful:

```
@inproceedings{doogan-buntine-2021,
    title={Topic Model or Topic Twaddle? Re-evaluating Semantic Interpretability Measures},
    author={Doogan, Caitlin  and Buntine, Wray},
    booktitle={Proceedings of the 2021 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies",
    month={jun},
    year={2021},
    address={Online},
    publisher = "Association for Computational Linguistics},
    url={https://www.aclweb.org/anthology/2021.naacl-main.300},
    pages={3824--3848}
}
        
```

