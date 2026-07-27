---
layout: default
title: Analyzing Action Verbs
---
<link rel="stylesheet" href="/blog42/assets/css/blog42.css">

* TOC
{:toc}

# 42 Books / 42 Years Blog Post 2: Analyzing Action Verbs; or, a Stylometry of Embodiment
This is the second post in a series of blogs that use digital humanities methodologies to examine the books comprising the [History of Black Writing (HBW)](https://hbw.iu.edu)'s 2025 exhibit [*42 Books / 42 Years*](https://hbw.iu.edu/news-events/events/42Books-42Years/index.html). The second and third posts in the series present a continuation of the first post's stylometric analysis.

## Introduction
In [the previous post](./blog-1.html), I engaged in a stylometric analysis of HBW's *42 Books / 42 Years* corpus by looking at the 100 to 2,000 most frequent words (MFW) occurring among the 42 novels. While I noted that I preprocessed the corpus twice, once including stopwords, and once excluding them, I only focused on the version of the corpus that contained stopwords. My reason was that any list of stopwords—such as [the one found in spaCy](https://github.com/explosion/spaCy/blob/master/spacy/lang/en/stop_words.py), the Python natural language processing (NLP) library I used to tokenize the corpus—consists of high-frequency, low-semantic words such as pronouns and prepositions. Therefore, excluding stopwords would entail the loss of unconscious stylistic fingerprints latent in texts.

However, in this post, I want to engage in a different kind of stylometric analysis. Namely, I want to examine embodiment in stylometric dimensions. In the previous post's analyses, Carolyn Tillman's *Life on Wheels* (W_Tillman_Wheels_1975) appeared as a stylometric anomaly across MFW strata. *Life on Wheels*, to recapitulate, is a semi-autobiographical narrative about a woman confined to a wheelchair. Accordingly, the stylometric uniqueness of Tillman's novel may stem from a difference in embodiment—or, a difference in interacting with the world about oneself with one's body.

## Methodology

But, how to go about analyzing embodiment in terms of stylometry? One hypothesis I have is that since we do things with our bodies, looking specifically at verbs that denote action, such as do, go, and stand, can help me view different modes of embodiment in our corpus of 42 novels.

This is exactly where the exclusion of stopwords may help. If you have clicked the link to spaCy's stopword list, you will perhaps have noticed that it contains a lot of a lot of verbs that don't have much to do with action, like "can," "have," and "be."

At the same time, while stopword exclusion is a good place to start, it is not the only step I should take in preparation for an analysis of action verbs. This is so, especially as spaCy's stopword list includes several action verbs (e.g., "move," "say," and "see").

In view of this, the steps I took to preprocess the corpus of 42 novels for action verb analysis were:

1. In the first post, I had simply tokenized the corpus—that is, I had broke each text down to lists of individual words. In this post, I lemmatized and part-of-speech tagged the corpus. Lemmatization refers to the conversion of each word to its lemma, or dictionary form. In the process of lemmatization, therefore, "went" turns into "go," and "said," into "say." Part-of-speech tagging involves the association of each word with the part of speech category it belongs to. "Apple," for example, will be tagged as a noun (unless it refers to the company, in which case it will be tagged as a proper noun), and "the" will be tagged as a determiner.

2. Owing to the part-of-speech tagging process, I excised any words that were not verbs, including auxiliary verbs like "should" or "may."[^1]

3. I modified the spaCy stopword detection process, such that the action verbs "go," "move," "put," "make," "take," "get," "give," "keep," "say," "see," "show," "call," "name," "part," "use," and "do" are no longer considered stopwords.

4. Finally, I excised any verb lemmas which were detected as stopwords from the corpus.

What remains is a corpus of 42 texts that have been effectively transformed into bags of action verbs. Having thus processed the 42 novels, I can now analyze each novel's respective ways of using action verbs.

## Analysis

### Principal Component Analysis (PCA): Overview of Top Action Verbs

I'll start my analysis with a PCA plot. This will aid in showing the general trends of action verb preference followed by the 42 novels. In the interactive plot below, I take the top 100 action verbs occurring across the campus, calculate the z-score values of the frequencies of each of the verbs in each novel using the normalization process described in the previous blog post, and placed each novel on a two-dimensional plot using these z-scores.

![Figure 1. An interactive PCA plot of the 42 novels, displaying how the novels differ with regard to the frequency with which they use the top 100 action verbs occurring among them. Four novels are highlighted, and named on the legend.](../assets/figs/blog2/fig1.html){: #Figure1}
>Figure 1. An interactive PCA plot of the 42 novels, displaying how the novels differ with regard to the frequency with which they use the top 100 action verbs occurring among them. Four novels are highlighted, and named on the legend.

Although I was expecting *Life on Wheels* to remain the most extreme stylometric outlier in this analysis, it is in fact Ben Okri's *Astonishing the Gods* which assumes this role. In the previous post, I had observed that Okri's novel strayed far from the other novels in the PCA plots on higher MFW strata. The most probable explanation for this is that the thematic contours of *Astonishing the Gods* is substantially different from the other 41 novels. Okri's novel depicts a society of invisible beings living in a world where "Every experience is repeated or suffered till you experience it properly and fully the first time."[^2] This suggests that *Astonishing the Gods*'s stylometric uniqueness may have its roots in a fundamentally novel representation of embodiment. Accordingly, examining the action verbs that are prominent in *Astonishing the Gods* may help us understand what exactly makes this novel so unique.

In addition to *Astonishing the Gods*, I highlighted four other novels which stood on the edges of the PCA plot. *Life on Wheels*, expectedly, is one of them; Jasmine Guillory's *By the Book*, which is not too far away from Tillman's novel on the plot, is also included. A deeper look at the verbs influencing these two novels' position on the plot will reveal any common threads between them.

For other novels, this task is easier to accomplish. William Wells Brown's *Clotel*, the fourth highlighted novel, is in proximity to such early Black novels as Joel Augustus Rogers's *From "Superman" to Man* (Rogers_Superman_1917), Sutton Elbert Griggs's *Imperium in Imperio* (Griggs_Imperium_1899), and Frances Ellen Watkins Harper's *Iola Leroy* (Harper_Iola_1892). This suggests a similarity in how these authors depicted Black embodiment.[^3]

Thus far, I made a lot of claims based on a two-dimensional plot, which, at the end of the day, fails to reveal much about the specifics of what verbs influence which novel's position. In order to gain insight into the modes of embodiment that dominate these novels, let's look into the top action verbs in the corpus, as well as in each of the four highlighted novels.

## The Novel vs. the Corpus: Slopegraphs

I chose to use slopegraphs to visualize the different modes of embodiment found in the four novels I highlighted in [Figure 1](#Figure1). A slopegraph is a chart of lines where each line plots the difference between two data points. [Figure 2](#Figure2) presents two slopegraphs. The left slopegraph tracks the relative frequencies[^4] of the top 25 most frequent action verbs in the corpus versus the relative frequencies of these verbs in the selected novel. The right slopegraph, on the other hand, tracks the relative frequencies of the top 25 most frequent action verbs in the selected novel versus the relative frequencies of these verbs in the corpus. By default, the two slopegraphs display the data for *Astonishing the Gods*, the other four novels may be selected using the dropdown menu below the title.

![Figure 2. Two slopegraphs respectively showing: (1) the relative frequencies of the top 25 most frequent action verbs in the corpus versus the relative frequencies of these verbs in the selected novel, and (2) the top 25 most frequent action verbs in the selected novel versus the relative frequencies of these verbs in the corpus. Using the dropdown menu, one of the four highlighted novels can be selected.](../assets/figs/blog2/fig2.html){: #Figure2}
>Figure 2. Two slopegraphs respectively showing: (1) the relative frequencies of the top 25 most frequent action verbs in the corpus versus the relative frequencies of these verbs in the selected novel, and (2) the top 25 most frequent action verbs in the selected novel versus the relative frequencies of these verbs in the corpus. Using the dropdown menu, one of the four highlighted novels can be selected.

Looking at the left slopegraph for *Astonishing the Gods*, we see that verbs that denote transaction (get, take, give), active interaction (tell, ask, let, call), and the action verb "do" are drastically underutilized—you have to use the "Pan" tool on the top right of the screen to see where they rank on the novel. In contrast, verbs of passive perception (see, feel, hear) are used way more frequently in Okri's novel relative to the corpus.

Looking at the right slopegraph gives this second observation a more defined shape, wherein Okri's frequent use of verbs of passive perception of environment (notice, understand, learn) stands in stark discrepancy with the corpus at large. The same applies to Okri's use of verbs that denote interaction with objects or one's environment rather than others, namely, "pass," "fall," and "lose." It appears that the central conceit of *Astonishing the Gods*—i.e., its portrayal of invisible beings (re-)experiencing things in full—brings about a mode of embodiment that is unique to it. This mode of embodiment focuses on being in touch with and understanding one's surroundings without being influenced by or confined to others' viewpoints. This explains why Okri's novel overutilizes the verb "speak" while underutilizing "say" and "tell": one can speak without any listeners around, but one says or tells something to another.

Compared to *Astonishing the Gods*, *Life on Wheels* presents a more clear-cut case of a difference in embodiment. On the left slopegraph, we see that Tillman's novel makes far less use of two verbs of physical activity, "turn" and "stand," compared to the corpus average. "Find" may be grouped together with these two verbs; with limited physical mobility, one perhaps can't find things on their own. This could be why Tillman's novel makes more use of "tell," "ask," and "call" compared to the corpus average: one has to ask others to do things for them.

It is interesting that, on the right slopegraph, *Life on Wheels* is seen to utilize "start," "love," and "cry" far more than the corpus average. The latter two verbs share an emotional charge, albeit with opposite valences, but what about "start"—is it supposed to evoke a sense of hope and optimism? If it is, this would explain the proximity between Tillman's novel and *By the Book*. While "start" is not in the top 25 verbs of Guillory's book, it actually is the 27th verb. Accordingly, while it may not feature in the right slopegraph for Guillory's novel, it may account for its closeness to *Life on Wheels* on [Figure 1](#Figure1). To add, the right slopegraph for *By the Book* demonstrates that Guillory uses such verbs as "smile" and "laugh" far more than the corpus average, continuing this thread of action verbs with positive valence.

On the other hand, despite being a romance, *By the Book* does not make as much use of the emotionally-charged verbs "love" and "cry"—these two verbs rank as the 52nd and the 78th most frequent action verbs in Guillory's novel, respectively. Rather, *By the Book* is defined by its uniquely frequent use of "write" and "work." Taking into account that the two main characters of *By the Book* are an editorial assistant and a writer, respectively, it can be said that by way of these verbs, the characters of Guillory's novel positively embody a contemporary vision of a working life.

Other novels may present other conditions of life that disallow such a positive valence in embodiment as that present in *By the Book*. One such novel is William Wells Brown's *Clotel*. Looking at the right slopegraph for Brown's novel, we see such verbs as "sell," "bring," "pass," and "return" being overutilized in *Clotel* compared to the corpus average.[^5] *Clotel*'s world is one of constant motion (pass, return), and transaction (sell, bring). In context of *Clotel*'s antebellum narrative, Black bodies are also objects of transaction. In this context, "pass" can also have the connotation of passing as a white person in order to avoid slavery and other forms of racial injustice. In such a world, feeling and wanting are relegated to the background, as evidenced by the left slopegraph.

Accordingly, Brown's novel presents a mode of the embodiment that is in stark contrast to that observed in Okri's novel. Where the action verbs foregrounded in *Astonishing the Gods* depicted a world free from the confines of society, *Clotel* finds Black people shackled, literally, by the society in which they exist.

## Conclusion

All in all, while high-frequency words are generally not excluded in stylometric analyses, excluding them and focusing only on a grouping of words can reveal interesting things about the ways in which novelists write their narratives.

Action verbs, as we have seen, are revelatory of the modes of embodiment that characterize the modes of embodiment that dominate a narrative based on the context portrayed by that narrative. Accordingly, the analysis of action verbs were particularly illuminating with regard to *Astonishing the Gods* and *Clotel*, which depict two disparate environments.

In comparison, action verb data perhaps didn't produce results that were as interesting with regard to *Life on Wheels*. The fact that "stand" is underutilized in a novel about a paraplegic woman is not especially surprising. Still, I believe that there is more to be learned from a computational analysis of embodiment in HBW's 42 Books / 42 Years corpus. Therefore, the next blog post in this series will delve into the syntax of the 42 novels that make up this corpus.

I will end this post with [Figure 3](#Figure3), which is an extended version of [Figure 2](#Figure2) that contains slopegraphs for all 42 novels in the corpus. Using [Figure 3](#Figure3), you can engage in analyses of action verbs in each of the 42 novels in a way that goes beyond this blog post.

<figure id="Figure3" class="plotly-figure">
  <iframe
    class="plotly-embed"
    src="/blog42/assets/figs/blog2/fig3.html"
    title="Figure 3. Interactive slopegraphs comparing action-verb frequencies"
    scrolling="no">
  </iframe>

  <figcaption>
    <i>Figure 3.</i>
    Two slopegraphs respectively showing: (1) the relative frequencies of the top 25 most frequent action verbs in the corpus versus the relative frequencies of these verbs in the selected novel, and (2) the top 25 most frequent action verbs in the selected novel versus the relative frequencies of these verbs in the corpus. Using the dropdown menu, one of the 42 novels can be selected.
  </figcaption>
</figure>

## Notes
[^1]: It is important to note here that I am using the pos_ attribute of a spaCy token, which gives the coarse-grained part-of-speech of a word. This is in contrast to the tag_ token attribute, which denotes the fine-grained part-of-speech of a word. Coarse-grained and fine-grained are two terms of art that respectively refer to broader and narrower part-of-speech categories. As regards the English language, the coarse-grained part-of-speech may reveal grammatical relationships within a sentence, whereas the fine-grained part-of-speech may reveal morphological information on the word. To demonstrate this phenomenon, take the sentence "He is running." The coarse-grained parts-of-speech for "is" and "running" are respectively AUX (meaning auxiliary verb) and VERB (self-explanatory). In comparison, the fine-grained parts-of-speech of these two words are respectively VBZ (meaning a third-person singular present verb) and VBG (meaning a verb in the form of a gerund or a present participle). It will be seen from this explanation that the pos_ attributes of spaCy tokens are more helpful to my analysis compared to the tag_ attributes.
[^2]: Okri, Ben. *Astonishing the Gods*. Phoenix House, 1995.
[^3]: Note, also, that John Oliver Killens's *Great Gittin' Up Morning* (Killens_Morning_1972) is in close proximity to the works of these early Black novelists. As stated in the previous post, Killens's use of a nineteenth-century document as a historical source may have led him to unconsciously take from the style of that century. It follows from [Figure 1](#Figure1) that this stylistic transfer observed in Killens's novel also manifests in the dimension of embodiment.
[^4]: On the slopegraphs, I use the relative frequency of each verb—that is, the raw frequency of a verb divided by the total frequency of all verbs. I use relative frequencies instead of z-scores, as I am interested in what verbs dominate each of these four novels, as well as what verbs dominate the corpus at large. If I were to use z-score values for each verb, two issues would arise. Firstly, the top verbs for each novel would not be representative of the actual frequencies of the verbs; if Okri, say, included here and there in his novel a verb used by no one else in the corpus, that would come to define his novel, as opposed to verbs he actually uses with great frequency. Secondly, since the z-score normalization process measures the distance between data points and their means in the dataset, the z-score for each verb as regards the corpus would be 0. Accordingly, the right slopegraph in [Figure 2](#Figure2) could not exist.
[^5]: I do not mention "reply" in this analysis, as the preference of reply over other forms of utterance seems a stylistic one.
