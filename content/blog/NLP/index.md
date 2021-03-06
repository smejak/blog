---
title: Learn NLP, it'll be fun, they said. It was fun, we should do this again, said the transformer.
date: "2021-05-20 19:00:00"
categories: "tech"
tags: [nlp, online course, deep learning, ai] 
---

After exploring different fields of machine learning for a while now, I decided to give Natural Language Processing (NLP) a shot. As with any ML field, pretty much everything in NLP looks like magic before you start to get into it, and then you're left to wonder how on Earth is it possible that something like this works so well. I'll hopefully get to my philosophical summary of AI in a different post, today, I'd like to talk a bit about my experience with NLP, which I primarily obtained from the [Natural Language Processing Nanodegree course from Udacity](https://www.udacity.com/course/natural-language-processing-nanodegree--nd892).

## Part 1: Get data, process it, build a statistical model

I recently attended a [conference from ScaleAI](https://scale.com/events/transform) and happened to catch the talk by Andrew Ng, where he covered the rather ironic reality that the vast majority of AI research papers cover the model-side of machine learning, even though in practice, you're going to spend way more time on cleaning and generally processing your data so that your model can learn something from it. I suppose this is true for the tools we use when building AI powered applications as well, libraries like TensorFlow and PyTorch make it easier than ever to build state-of-the-art AI models, but I have not yet found a library that would automatically preprocess all your data. Compared to importing pretrained models and adding a couple of custom layers to fit the specific task, preprocessing data feels like an extremely labor-intensive task. Specifically in NLP, the procedure you learn to implement for a part-of-speech tagging process can be divided into the following parts:

#### Getting the data

You can probably get all the data you'll ever need from the internet. Of course, the data won't be very clean and if you're going to build a web-scraper that will download gigabytes of text data, you need to account for bias, but for a simple part-of-speech tagger, you don't really need to worry about bias.
In python, we can get data from the internet via the [requests library](https://docs.python-requests.org/en/master/). Here's a simple of example of how we might use it:
```
import requests

r = requests.get("https://en.wikipedia.org/wiki/Natural_language_processing")
text = r.text
```
#### Cleaning the data

It would be nice if getting data from the internet was as simple as running a `requests.get()`... Unfortunately, we have some more work to do. If we print out the text from above, it looks something like this:
```
<ul id="footer-info" >
	<li id="footer-info-lastmod"> This page was last edited on 7 April 2021, at 03:07<span class="anonymous-show">&#160;(UTC)</span>.</li>
	<li id="footer-info-copyright">Text is available under the <a rel="license" href="//en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License">Creative Commons Attribution-ShareAlike License</a><a rel="license" href="//creativecommons.org/licenses/by-sa/3.0/" style="display:none;"></a>;
additional terms may apply.  By using this site, you agree to the <a href="//foundation.wikimedia.org/wiki/Terms_of_Use">Terms of Use</a> and <a href="//foundation.wikimedia.org/wiki/Privacy_policy">Privacy Policy</a>. Wikipedia?? is a registered trademark of the <a href="//www.wikimediafoundation.org/">Wikimedia Foundation, Inc.</a>, a non-profit organization.</li>
```
...? Doesn't look like something you'd want to use as training data for a language model. Luckily, there is an easy solution called [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/).
```
from bs4 import BeautifulSoup

soup = BeautifulSoup(text, "html5lib")
summaries = soup.find_all("div", class_="mw-parser-output")
summaries[0].select_one("p").get_text().strip()
```
These few lines of code managed to turn the thing above into something way nicer:
```
'Natural language processing (NLP) is a subfield of linguistics, computer science, and artificial intelligence concerned with the interactions between computers and human language, in particular how to program computers to process and analyze large amounts of natural language data.  The result is a computer capable of "understanding" the contents of documents, including the contextual nuances of the language within them. The technology can then accurately extract information and insights contained in the documents as well as categorize and organize the documents themselves.'
```
Just beautiful... but it's not without its flaws. Before getting the plain text, I needed to go to the Wikipedia page in my browser and inspect the html to find a div which contained this text and then input the div's class to the find_all() method. For scraping a single website like Wikipedia, where the html and css will likely be consistent on most pages, this one manual search is fine, but if you'd like to build an app that scrapes tens or even hundreds of webpages, the task of manually getting all html classes would suddenly become equivalent to finding a hundred needles in a hundred haystacks (maybe not equivalent, but close enough).

#### Normalization, tokenization, and other -izations

After the quite laborous task of searching through the html code on the website you want to scrape, the last step is probably easier. Depending on what task you want to solve, you might implement a different combination of preprocessing methods, but generally, you'll probably want to get rid of everything which might confuse your model at any point during training. This may or may not include capitalization, characters which don't add meaning to the sentence, words which are basically just filler, perhaps even the modifications of individual words. Some of these tasks can be accomplished with simple string manipulation, for others, you might need to import libraries such as [regular expression](https://docs.python.org/3/library/re.html) or the [natural language toolkit](https://www.nltk.org/).

#### Building the HMM Tagger

Now that we've covered preprocessing, we can get into the really fun stuff. 

Before I get into the specifics, I have to quickly go over what HMMs are (and no, I don't mean the shipping company). Hidden Markov Models (HMMs) are, simply put, statistical models which map and label objects based on some probabilities calculated from training examples. Ok, that didn't do it at all, so let's try the practical approach. Since I was building a HMM for a part-of-speech tagger, let's consider it in that context. Say I have a bag of words and I want to build a sentence out of them. Now, since I use language every day, I may not realize it, but somehow I have learned that sentences usually follow a certain structure. A noun might be followed by a verb, then maybe an adjective followed by another noun: "Dog ate pretty cat" (I know, not the best sentence in the world). Each word in a sentence has a probability of being followed by another word and each individual word also has a probability of belonging to a certain part of speech. A Hidden Markov Model maps all these probabilities.

## Part 2: We need to go deeper

When you get into AI, you hear all these big sounding phrases like "computer vision", "natural language processing", or "deep reinforcement learning". It makes an impression that these different fields are very separated, but the truth is, they're not all that different once you scratch the surface. I think the biggest differences are arguably at the data processing level, since once you get to coding the specific models with PyTorch or TensorFlow, natural language processing isn't all that different from computer vision (just to elaborate, the models themselves are quite different since their architecture is built with completely different tasks in mind, but the process is very intuitive compared to data preprocessing).

My second big project in NLP was an English-French translator. This time, all the data was available and the preprocessing was fairly straightforard, all that really needed to be done is tokenize sentences into words and apply padding where necessary (because English and French sentences with the same meaning don't always have the same length). In my final model, I implemented an encoder-decoder model with embedding layers and bidirectional GRU's, which proved to be quite effective at learning the translation task in previous experiments. The full code of the model can be found [here](https://github.com/smejak/Udacity_Natural_Language_Processing_Nanodegree/blob/main/%232%20Machine%20Translation/machine_translation.ipynb) (under "Model 5: Custom (IMPLEMENTATION)").

## Part 3: Final project and conclusion

The final part of the Nanodegree was focused on speech recognition, which, after reading a couple of blog posts and looking more into what's happening in NLP, is probably a little different to what is usually considered NLP, nevertheless, speech is still a crucial part of language (and who doesn't want to know how to make your own version of Siri).

My last project was a speech recognizer using deep neural networks as its primary means of learning to decode sound signals into text. An ASR (Automatic Speech Recognition) pipeline can be boiled down to the following: 

CNN model for feature extraction -> RNN model for learning to identify words from the features -> [CTC](https://towardsdatascience.com/intuitively-understanding-connectionist-temporal-classification-3797e43a86c)(Connectionist Temporal Classification) loss function for converting the RNN outputs into words -> [NLM](https://arxiv.org/abs/1906.03591) (Neural Language Model) making sure the text actually makes sense

The implementation of my ASR project is available [here](https://github.com/smejak/Udacity_Natural_Language_Processing_Nanodegree/tree/main/%233%20Speech%20Recognition) ("sample_models.py" show the individual models, "vui_notebook.ipynb" contains the main code). Truth be told, most of the implementation on my side was once again in the models themselves, the vast majority of the ASR pipeline was provided by Udacity, nevertheless, model building is fun (except maybe when you're waiting 2 hours for a model to train only to find it has worsened since the last hyperparameter change, but there are a numer of automatic hyperparameter tuning solutions that I did not implement due to the relatively small size of the project).

## Final thoughts

I find NLP absolutely fascinating. Looking at an interview with GPT-3, it is clear that we have entered a world where you cannot know for sure what is human-made and what is AI-made, which might be a little bit scary, but I like to think of it as extremely exciting, just think of the possibilities for automation and increased efficiency. Nevertheless, it's still important to understand the current limitations of AI and realize that a lot of the hype around it is artificial (no pun intended). At the end of the day, it is still a bunch of column vectors changing their values based on some error function (which our minds might be too, however; don't want to be undermining the incredible complexity that is behind all the PyTorch/TensorFlow imports). I don't want to get ahead of myself, there's still so much to learn and explore.