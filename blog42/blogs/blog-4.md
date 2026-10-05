---
layout: default
title: Analyzing Parts of Speech
---
<link
  rel="stylesheet"
  href="{{'/blog42/assets/css/blog42.css'}}">

* TOC
{:toc}

# 42 Books / 42 Years Blog Post 4: Topic Modeling
This is the fourth post in a series of blogs that use digital humanities methodologies to examine the books comprising the [History of Black Writing (HBW)](https://hbw.iu.edu)'s 2025 exhibit [*42 Books / 42 Years*](https://hbw.iu.edu/news-events/events/42Books-42Years/index.html). This post deviates from the stylometric methodology of the first three blog posts, and puts focus on the predominant topics in the *42 Books / 42 Years* corpus, using topic modeling.

## Introduction
In the first three blog posts, I had undertaken a stylometric analysis of our corpus of 42 books. In [the second post](./blog-2.html), I landed on the topic of embodiment as manifesting in the linguistic style of the novel. In [the third post](./blog-3.html), my analysis of embodiment in style branched out into the questions of narrative voice, temporal perspective in narration, and non-human embodiment.

I have been, then, thinking about topics, matters, questions—things that appear in the novel. An evident continuation of this line of thought is to look directly at the words that co-occur within and among the textual narratives that make up our corpus. In so doing, I can examine the discursive contours of our corpus of 42 novels.

## Methodology
But, how? The answer to this question comes by way of topic modeling, a longstanding tool of computational literary analysis. In short, topic modeling takes a set of documents, looks at words that co-occur in different documents, and assigns words into topics based on their probability of co-occurrence.

### On Preprocessing
Applied to novels, topic modeling promises to highlight the salient discourse(s) that characterize a given narrative. But, how to apply this method to a corpus of novels? The answer to this question mainly lies in preprocessing. Carson Sievert and Kenneth Shirley,[^1] in their conference paper introducing the LDAvis topic model visualization tool, use a corpus of posts from discussion forums. Such posts are short, and, being actual instances of communication, often have a clear topic. Novels are not so—they are documents of extensive length, with multifarious themes, characters, and plotlines. If I feed the topic model with the 42 novels as uniform documents, all I will get is very general topics; i.e. ubiquituous matters that occur in almost every novel and don't really say anything special about the novels. Thus, one preprocessing step I took was the segmentation of each novel into chunks of 500 words. In this way, instead of 42 documents that may offer only general themes, I have 1,284 documents that yield meaningful thoroughlines occurring among the 42 novels. Chunking also allows me to track the narrative trajectory of each novel by examining which topic is more prevalent in which chunk(s).

Prior to chunking, I part-of-speech-tagged the corpus (again), and excised any word that wasn't a noun, and then lemmatized these nouns. My rationale is that nouns are the most likely part of speech (POS) to throw the thematic elements of novels into relief. Closed class POSs like determiners and pronouns (so called because they are of a limited number, and new ones cannot be easily created in a language) are bound to say more about the style of a novel rather than about its content. In the case of open class POSs, proper nouns will inevitably create topics that exclusively pertain to one novel; adjectives and adverbs are primarily descriptive, and unlikely to yield much in the way of thematic information; verbs are, admittedly, likely to produce this kind of information, but since I already engaged in an analysis of action verbs in a previous post, I opted to not include them in this analysis.[^2 interjections] Finally, I lemmatized the nouns, as such a difference as that between "apple" and "apples" does not create any sort of meaningful thematic difference.

Aside from filtering for nouns alone, I also added two conditions to my preprocessing pipeline: (1) I excluded any noun that appeared in less than 4 chunks; (2) I did the same for any noun that appears in over 80 percent of the chunks. The first condition will remove any overly specific terms from the corpus, while giving space to words that, while perhaps specific to a novel, take up a considerable part of said novel. The second condition removes any overly general terms that will not yield and specific thematic information from the corpus.

The second condition also helps with the imbalanced number of chunks produced by each novel across the corpus. The imbalance is greatly caused by Leon Forrest's *Divine Days* (1992), which produces a whopping 193 chunks compared to the corpus's mean of around 30 chunks. Forrest's gargantuan novel is an outlier in the corpus, and as [Table 1](#Table1) shows, removing *Divine Days* from the corpus decreases the standard deviation of the dataset considerably. Removing Forrest's novel also makes the dataset less skewed (as evidenced by a decrease in the mean). Less skewness in the dataset means that the data is more symmetrical, which in turn provides proof that *Divine Days* is an outlier. Nevertheless, removing *Divine Days* from the corpus altogether would ruin this series of posts' focus on the 42 novels in the *42 Books / 42 Years* corpus. I could alternatively pick only a random number of chunks from *Divine Days* that is close to the mean, but that would most likely mean losing important information residing in the left out chunks. In comparison to these options, making it so that words that appear in above 80% of the chunks makes *Divine Days*'s chunks less likely to influence the dataset on the whole.

-- table--

Another, admittedly more important, matter that makes the imbalance created by Forrest's novel less significant lies in a preprocessing step I didn't take. It is common in topic modeling applications to limit the vocabulary of the topic model to the *n* most frequent words in the corpus (usually *n* is around 5,000). By skipping this step, I make it so that chunks from other novel's don't have to fight with *Divine Days*'s chunks in order to be selected in the model's vocabulary. The issue of very specific words is virtually resolved by the exclusion of proper nouns and any noun that appears in fewer than 4 chunks, anyhow.

### On What Comes After Preprocessing
What comes after this rather boring and convoluted preprocessing step is rather simple: I use Latent Dirichlet Allocation (LDA) to create a topic model with 10 topics.[^3 lda] Once the topic model is created, I use three visualization methods:

1. **PyLDAvis**: PyLDAvis—the Python port of LDAvis mentioned above—allows me to see the most salient words in the topic model, as well as words that are the most relevant to each topic. A word's salience is computed according to a formula that checks its frequency across the corpus as well as its distinctiveness across topics. In other words, high overall frequency + high specificity = high salience. Relevance is computed according to a formula that looks for either words that are the most representative for that topic. The relevance metric is affected by the lambda (λ) parameter, which can be between 0 and 1: a lower λ value looks for words exclusive to a given topic, whereas a higher λ value looks for words that occur most frequently in that topic. The λ value of 0.6 is often used as a sweet spot for purposes of the human interpretability of the topic, as identified by Sievert and Shirley[^4]. Following this, I take 0.6 as the default λ value. Aside from providing the most salient and relevant words, pyLDAvis also visualizes the distance between topics on a two-dimensional plot.
2. **Principal Component Analysis (PCA) on Topic Prevalence Values**: PyLDAvis is limited by the fact that it gives exclusive focus to topics, and overlooks the documents that underlie these topics. Accordingly, it gives us no information on which topic defines which novel(s). So as to resolve this problem, I took the means of the prevalence of each topic in every chunk in a given novel, for all 42 novels. This gave me the percentage of each topic's prevalence in each novel. I then put each novel, as represented by ten topic prevalence values, on two-dimensional and three-dimensional PCA plots. The latter PCA plot is new to this series of blog posts, and tells us a bit more about what PCA plots can and cannot show.
3. **Heatmaps**: Using the topic prevalence values for each chunk, I produce 42 heatmaps that display the narrative trajectory of each novel according to the movement among topics. These heatmaps can also tell us about how broad the variance of topics in each novel is—some novels are bound to be rather one-note, whereas others will show more variance across topics.

## Analysis
### PyLDAvis Dashboard: What Can 10 Topics Tell Us About 42 Novels?
My plan for this blog post is that its analysis will be less focused on certain novels, as the two previous posts have been. The pyLDAvis dashboard displayed in [Figure 1](#Figure1) provides good ground to begin a post with such a general approach.




## Analysis
### Principal Component Analysis (PCA): What's Different? What's the Same?
Let's start with a PCA plot again. In the second post's PCA plot, which used action verb data of the 42 novels, I saw that Carolyn Tillman's *Life on Wheels* was no longer the stylometric outlier that it was on the first post. I ascribed this change to the fact that an analysis of action verbs is, effectively, an analysis of content or the thematic layers of novels. Thus, the outlier becomes Ben Okri's *Astonishing the Gods*, which was also the outlier in PCA plots that used a higher number of most frequent words in the first post.

Since this post looks at the syntactic roles of words instead of the words themselves, what it presents is not an analysis of content. Accordingly, as [Figure 1](#Figure1) shows, the result of the analysis to be different from that observed in the previous post. Tillman's novel, far away from the crowding around the center of the plot on the right, is trailed, not very closely, by Jasmine Guillory's *By the Book*. Interestingly, a proximity between these two novels was also observed in the previous post. I had ascribed this proximity to the focus on emotions and a positive valence observed in both novels. It may be the case that a focus on emotion and affect may require a different kind of embodiment—possibly, one that relegates a language of external interaction to the background in favor of a more internal discourse.

<figure id="Figure1" class="plotly-figure plotly-figure--pca">
  <iframe
    class="plotly-embed plotly-embed--pca"
    src="{{'/blog42/assets/figs/blog3/fig1.html'}}"
    title="Figure 1"
    aria-describedby="Figure1Caption"
    loading="lazy">
  </iframe>

  <figcaption id="Figure1Caption">
    <i>Figure 1.</i>
    An interactive PCA plot of the 42 novels, displaying how the novels differ with regard to the frequency with which they use each of the 13 POSs. Three novels are highlighted and named in the legend.
  </figcaption>
</figure>

In addition to Tillman and Guillory's respective novels, I highlighted two other novels. Ben Okri's *Astonishing the Gods* was highlighted due to their significance in the previous post. On [Figure 1](#Figure1), this novel stands on the outskirts of the cloud of data points around the center of the plot. Aside from Okri's novel, I selected Countee Cullen's *My Lives and How I Lost Them* (1942), which deviates from the corpus average in the same quadrant as Tillman's novel. Out of the 42 novels in the corpus, it is the only one whose protagonist is a non-human animal. That the main character of *My Lives* is a cat requires the author to imagine a different kind of bodily interface with the world.

As with the previous post, though, such claims need to be substantiated—or disproven—by way of a deeper look. So, in the next section, I will introduce a new way of engaging with data in close proximity.

### Radar Charts: Embodiment in Regular and Irregular Shapes
In order to give each novel a closer look, I will use radar charts. A radar chart is comprised by a series of radial axes, each of which represents a quantitative variable. The values of each variable for an item that contains these variables are plotted and connected along each axis to form a polygon. In the context of my analysis, my radar charts will have 13 axes for the 13 POSs I measure the frequencies of. There will be two polygons displayed: first, there will be a polygon representing one of the novels; second, there will be a polygon representing the corpus average. As the z-scores for each POS have to be 0 when the corpus at large is considered, the latter polygon will be a regular tridecagon. In comparison, the former polygon will have an irregular shape, as no novel is likely to have POS frequency ratios that match the corpus average. The irregularity of the former polygon is what will allow me to understand each novel's styles of embodiment: a novel whose verb axis spikes and whose noun and adjective axes crater may prioritize feeling and perceiving, whereas a novel which has the opposite features may prioritize physical action.

[Figure 2](#Figure2) consists of radar charts for the three novels highlighted in [Figure 1](#Figure1); by default, the chart for *Life on Wheels* is displayed, but you can switch to another chart using the dropdown menu. The verb z-score for Tillman's novel is more than one standard deviation above the mean, suggesting a significant predilection for action. This complicates my finding in the previous post that *Life on Wheels* uses "tell," "ask," and "call" more than average while using "turn" and "stand" less than average, which reflects its author's and protagonist's physical disability.

<figure id="Figure2" class="plotly-figure plotly-figure--wide">
  <iframe
    class="plotly-embed plotly-embed--wide"
    src="{{'/blog42/assets/figs/blog3/fig2.html'}}"
    title="Figure 2"
    aria-describedby="Figure2Caption"
    loading="lazy">
  </iframe>

  <figcaption id="Figure2Caption">
    <i>Figure 2.</i>
    A radar chart showing a polygon that plots the z-scores of each of the 13 POSs in one of the three novels highlighted in the previous figure, superimposed on another polygon representing the corpus mean. The former polygon can be switched using the dropdown menu. 
  </figcaption>
</figure>

However, if we look at the adposition axis on the chart, we see that *Life on Wheels* uses adpositions less than two standard deviations below the mean. Since adpositions are used to establish spatial and temporal relations between persons, objects, and locations, we can surmise that the novel's frequent use of verbs do not signal a high frequency of action. This reading is bolstered by the fact that Tillman's novel uses subordinating conjunctions (e.g., whether, because, while, if), particles (e.g., 's, not), and interjections (e.g., oh, psst, um) at a higher frequency than the corpus average. All three of these POSs point not to movement, but to dialogue and internal monologue. In this light, the radar chart for Tillman's novel supports and develops the previous post's reading. *Life on Wheels*'s mode of embodiment is one of calling, asking, and telling. Accordingly, its sentences are structured in a way that lacks POSs that signify action. Although *Life on Wheels*'s disabled protagonist may be considered a thematic, and not a stylistic, element of the novel, POS analysis shows how embodiment injects itself into the stylistic dimension of a narrative.

In addition, *Life on Wheels*: (1) uses pronouns at a considerably greater frequency than the corpus mean, (2) uses nouns at an even more considerably lesser frequency than the corpus mean, and (3) uses proper nouns at a slightly lesser frequency than the corpus mean. Using pronouns at a greater rate than the corpus mean suggests either a high level of internality, which comes by way of a plenitude of the first-person singular pronoun, or a high degree of familiarity and/or intimacy with objects or persons, which comes by way of a plenitude of second- and third-person pronouns.

Looking at [Figure 3](#Figure3), which is another series of radar charts showing the z-score values of the top 10[^4] most frequent pronouns occurring across the 42 novels, we see that Tillman's novel uses "I" more than four (!) standard deviations above the mean, and "my," more than three standard deviations above the mean. The high frequency of these first-person singular pronouns can suggest simply that the book has a first-person narrative voice. While this is true, it cannot be the only conclusion reached by way of the pronoun radar chart for *Life on Wheels*. This is so, for narrative voice alone cannot explain why Tillman's novel additionally uses the third-person pronouns "he" and "it" as well as the second-person "you" at a greater rate than the corpus average, in view of the fact that the other 41 novels are also narrated in either the first or the third person. High z-scores observed in first-, second-, and third-person pronouns necessitate a reading that goes beyond narrative voice.

<figure id="Figure3" class="plotly-figure plotly-figure--wide">
  <iframe
    class="plotly-embed plotly-embed--wide"
    src="{{'/blog42/assets/figs/blog3/fig3.html'}}"
    title="Figure 3"
    aria-describedby="Figure3Caption"
    loading="lazy">
  </iframe>

  <figcaption id="Figure3Caption">
    <i>Figure 3.</i>
    A radar chart showing a polygon that plots the z-scores of each of the top 10 most frequent pronouns in one of the three novels highlighted in Figure 1, superimposed on another polygon representing the corpus mean. The former polygon can be switched using the dropdown menu. 
  </figcaption>
</figure>

One such reading is that the pronoun frequency ratios of *Life on Wheels* signal both a high level of internality, and likewise a high (but relatively lesser) degree of familiarity with one's close surroundings. In conjunction with the novel's low use of nouns and proper nouns seen in [Figure 2](#Figure2), [Figure 3](#Figure3) suggests that the style of embodiment in *Life on Wheels* is one which precludes movement among a large number of persons, and which confines the disabled body to introspection as well as personal interaction with only a tight circle of friends and family.

The drops in adjectives, determiners, and nouns in Tillman's novel in [Figure 2](#Figure2) contrasts with spikes in the same POSs in the radar chart for *Astonishing the Gods*. This points to a language of external description. This externality is accompanied by a high frequency of adpositions—another POS with a low z-score in *Life on Wheels*—which suggests a high degree of movement. Interestingly, *Astonishing the Gods* has a proper noun z-score that is three standard deviations below the mean; this hints at a solipsistic mode of embodiment. It could be the case that *Astonishing the Gods*'s unique world of invisible beings requires the consistent centering of the protagonist or the narrator, which may leave less room for interpersonal interaction.

This hypothesis is supported by the pronoun radar chart for *Astonishing the Gods* in [Figure 3](#Figure3). This radar chart shows only one spike, which is for the pronoun "he." Aside from this spike, Okri's novel does not deviate much from the norm. The other nine pronouns are mostly close to the norm, with the exception of the feminine pronouns, which hang around the range of one standard deviation below the mean. Okri's novel is narrated in the third person, and has a male protagonist. This being the case, it can be said that the narrator needs to focus on what the protagonist is or does in order to flesh out the narrative's setting. Note, also, that unlike "he," the possessive pronoun "his" sticks close to the norm. What is predominantly in view in this story is not what the protagonist has, whether in an internal or an external sense; rather, what we see is what the protagonist does in the world he inhabits.

Moving from Okri's novel to Cullen's *My Lives and How I Lost Them* in [Figure 2](#Figure2), we see a chart that is unlike Okri's, a little like Tillman's, but altogether distinct. Three POSs stick out due to their hovering around the range of two standard deviations above the mean: subordinating conjunctions, auxiliary verbs, and adverbs. All three of these POSs were also above the mean for Tillman's novel. Thus, a comparison between the dynamics of embodiments inhering in these two novels' use of these POSs.

Subordinating conjunctions have been discussed above in context of Tillman's novel as an indicator of dialogue and internal monologue. This makes sense for a novel with a first-person point of view like *Life on Wheels*. Cullen's novel, too, is written in the first-person, albeit the "person" in question is Christopher Cat. Christopher, like the other animals in the novel, is heavily anthropomorphized. Accordingly, he engages in frequent dialogue with the animals around him, not to mention his owner. Further, Christopher's actions are mediated by the narrative frame of Christopher's recounting his past eight lives. This framing imbues the narration of Christopher's actions with ample rumination—in other words, internal monologue.

We can thus see that similarly high subordinating conjunction z-scores in Tillman's and Cullen's novels have to do with a link in narrative styles, and that this link does not necessarily carry over in terms of embodiment. This link also explains the high auxiliary verb z-scores that Tillman's and Cullen's novels have. In an English-language prose narrative, such auxiliary verbs as "was," "have," and "had" can signal a backward glance on one's life. Auxiliary verbs like "could," "would," and "should" can likewise indicate rumination. A story that has both of these elements can contain such constructions as "should have had," which will cause the frequency of auxiliary verbs in the story to be distinctly high. *My Lives and How I Lost Them* is one such story, and so is *Life on Wheels*. Indeed, the two novels are very similar in this regard. Just like Christopher Cat, the narrator-protagonist of Tillman's novel relates her life's story in the retrospective. This is even even more evidence that *My Lives and How I Lost Them* uses certain POSs as a result of its retrospective narrative style, rather than any foregrounded form of embodiment. Further, the same could be said of *Life on Wheels*, which complicates the analysis on embodiment in Tillman's novel I put forward in previous paragraphs.

While *Life on Wheels*'s adverb z-score is lesser than 1, that of *My Lives and How I Lost Them* is greater than 2. Why does Cullen uses adverbs in his novel to such a degree? One reading of this abundance of adverbs in the novel is that it's a stylistic element of the novel's genre. Cullen's novel is aimed at an audience of children. A whimsical language suited for child readers may necessitate a high frequency of adverbs. An example of such a language appears in the introduction of the novel, where Christopher's Human Being is very briefly the narrator. The Human Being wonders: "why should he scratch me *none too gently* until I was *wide* awake to hear him say, 'Read me that'?"[^5] The italicized adverbs in the quoted excerpt are not functionally necessary, but give a playful quality to the language.

An alternative reading is that Cullen imagined feline embodiment to be much more animated than the human mode of embodiment, which entailed a high frequency of adverbs. Evidence as to the hypothesis that Cullen imagined a distinct mode of embodiment for cats can be seen in the radar chart for Cullen's novel in [Figure 3](#Figure3). Whereas the first-person *Life on Wheels* prioritizes first-person singular pronouns, and whereas the third-person *Astonishing the Gods* prioritizes "he", the pronoun with the highest z-score for *My Lives and How I Lost Them* is "we." Granted, "I" and "my" still have high z-scores, which attests to the first-person narrative voice of the novel. Nevertheless, Cullen's novel uses "we" more than two standard deviations above the mean, which is around 1 standard deviation greater than "I" and "my." This suggests that Cullen imagines a feline embodiment that is more collective than human embodiment. In this mode of embodiment, the plural "we" is as much of an agent as the singular "I," if not more.

## Conclusion
To sum up, POS-tagging the 42 novels in our corpus added nuance to the stylometric analysis of embodiment I had started in the previous blog post. Further, POS-tagging allowed me to filter for only pronouns, and hence to look deeper into the ways in which novels use pronouns. The word class of pronouns are especially important with regard to embodiment. In addition to being a strong indicator of a novel's narrative voice, pronoun frequencies can also tell us how a novel positions and looks at its characters. To pull an example from the paragraphs above: if, in a corpus of 42 novels, one novel uses the pronoun "I" more than four standard deviations above the norm, that says something more than simply that the novel has a first-person narrator.

At the same time, as we have seen in the comparative analysis of Tillman's and Cullen's novels, POS frequency observations may not necessarily have anything to do with embodiment. Data must, in the end, be interpreted by human eyes, just like text. On this note, I'll end with [Figure 4](#Figure4) and [Figure 5](#Figure5), which respectively show POS and pronoun radar charts for all 42 novels, instead of just the three novels discussed above. Using these two interactive figures, you'll be able to come up with your own interpretations.

<figure id="Figure4" class="plotly-figure plotly-figure--wide">
  <iframe
    class="plotly-embed plotly-embed--wide"
    src="{{'/blog42/assets/figs/blog3/fig4.html'}}"
    title="Figure 4"
    aria-describedby="Figure4Caption"
    loading="lazy">
  </iframe>

  <figcaption id="Figure4Caption">
    <i>Figure 4.</i>
    A radar chart showing a polygon that plots the z-scores of each of the 13 POSs in one of the 42 novels in the corpus, superimposed on another polygon representing the corpus mean. The former polygon can be switched using the dropdown menu. 
  </figcaption>
</figure>

<figure id="Figure5" class="plotly-figure plotly-figure--wide">
  <iframe
    class="plotly-embed plotly-embed--wide"
    src="{{'/blog42/assets/figs/blog3/fig5.html'}}"
    title="Figure 5"
    aria-describedby="Figure5Caption"
    loading="lazy">
  </iframe>

  <figcaption id="Figure5Caption">
    <i>Figure 5.</i>
    A radar chart showing a polygon that plots the z-scores of each of the top 10 most frequent pronouns in one of the 42 novels in the corpus, superimposed on another polygon representing the corpus mean. The former polygon can be switched using the dropdown menu. 
  </figcaption>
</figure>
## Notes
[^1]: Marcus, Mitchell P., et al. “Building a Large Annotated Corpus of English: The Penn Treebank.” Computational Linguistics, vol. 19, no. 2, 1993, pp. 313–30.
[^2]: Accessible [here](https://universaldependencies.org/u/pos/).
[^3]: In addition to these POSs, I also excluded the SPACE POS, which is one of spaCy's supplements to the Universal Dependencies tagset. SPACE denotes any whitespace character that is not simply the single space between words, and is mostly an indicator of minor noise in digitized text.
[^4]: Limited to this number, as there are over a hundred pronouns in the English language. Including every pronoun would make for a series of radar charts that are hard to interpret.
[^5]: Cullen, Countee. *My Lives and How I Lost Them*. Harper & Brothers, 1942. p. viii.
