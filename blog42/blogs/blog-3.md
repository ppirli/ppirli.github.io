---
layout: default
title: Analyzing Parts of Speech
---
<link
  rel="stylesheet"
  href="{{'/blog42/assets/css/blog42.css'}}">

* TOC
{:toc}

# 42 Books / 42 Years Blog Post 2: Analyzing Parts of Speech: or, A Stylometry of Embodiment, Part II
This is the third post in a series of blogs that use digital humanities methodologies to examine the books comprising the [History of Black Writing (HBW)](https://hbw.iu.edu)'s 2025 exhibit [*42 Books / 42 Years*](https://hbw.iu.edu/news-events/events/42Books-42Years/index.html). The second and third posts in the series present a continuation of the first post's stylometric analysis.

## Introduction
In [the previous post](./blog-2.html), I engaged in a stylometric analysis of the way in which the novels in the *42 Books / 42 Years* corpus deployed different modes of embodiment. I did this by excluding stopwords (high-frequency, low-semantic-value words) as well as any words that weren't verbs, which left me with a corpus of documents consisting of only action verbs.

While my analysis of action verbs was fruitful in uncovering how some novels exhibit starkly different modes of embodiment in accordance with the different environments they depict, it left me with the inkling that there was more to learn about the different modes of embodiment that manifested themselves in the 42 novels in our corpus. Therefore, this post takes another look at embodiment, this time from a different vantage point.

## Methodology
At the end of the previous post, I mentioned that action verbs have a connection to the thematic layers—or, what makes up the content—of a novel. To produce results that are different, I need to come up with a different stylometric methodology that still pertains to embodiment. But, how? The answer may lie in going beyond the words that populate the surface of the text, and looking into the syntactic makeup of these words.

In other words, I will look at the frequency of each part of speech (POS) in each of the 42 novels. To this end, I use the Python natural language processing (NLP) library [spaCy](https://spacy.io/)'s POS-tagger, which associates each word in a document with its POS. Those who read the notes in the previous blog post will remember that I'd used a POS-tagger to exclude any word which was not a verb from the corpus, and that in particular, I used the coarse-grained output of the POS-tagger, rather than using fine-grained POSs. Coarse-grained POSs denote the broad grammatical category of words, whereas fine-grained ones give morphological information on words in more detail. I will continue to use the coarse-grained output in this post.

Why? The simple answer is that fine-grained POSs are, as the name indicates, too granular; the more precise morphological information they yield comes at the cost of easily interpretable data. Take, for example, the sentence "I went to that place because I have been wanting to go there." [Table 1](#Table1) displays the coarse- and fine-grained outputs of the sentence after being parsed by a POS-tagger. As can be seen, the fine-grained output has many different identifiers for the broad part of speech category of the verb. Additionally, the fine-grained output gives "to" in the role of an infinitive particle (e.g., to go, to eat, to be) a class of its own, rather than including it within the broader class of particles, which includes the possessive marker "'s" and the negation marker "not."

<table id="Table1">
  <thead>
    <tr>
      <th scope="col">Word</th>
      <th scope="col">Coarse POS (<code>pos_</code>)</th>
      <th scope="col">Fine POS (<code>tag_</code>)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>I</td>
      <td><code>PRON</code> (Pronoun)</td>
      <td><code>PRP</code> (Personal pronoun)</td>
    </tr>
    <tr>
      <td>went</td>
      <td><code>VERB</code> (Verb)</td>
      <td><code>VBD</code> (Verb, past tense)</td>
    </tr>
    <tr>
      <td>to</td>
      <td><code>ADP</code> (Adposition)</td>
      <td><code>IN</code> (Preposition or subordinating conjunction)</td>
    </tr>
    <tr>
      <td>that</td>
      <td><code>DET</code> (Determiner)</td>
      <td><code>DT</code> (Determiner)</td>
    </tr>
    <tr>
      <td>place</td>
      <td><code>NOUN</code> (Noun)</td>
      <td><code>NN</code> (Noun, singular or mass)</td>
    </tr>
    <tr>
      <td>because</td>
      <td><code>SCONJ</code> (Subordinating conjunction)</td>
      <td><code>IN</code> (Preposition or subordinating conjunction)</td>
    </tr>
    <tr>
      <td>I</td>
      <td><code>PRON</code> (Pronoun)</td>
      <td><code>PRP</code> (Personal pronoun)</td>
    </tr>
    <tr>
      <td>have</td>
      <td><code>AUX</code> (Auxiliary verb)</td>
      <td><code>VBP</code> (Verb, non-3rd person singular present)</td>
    </tr>
    <tr>
      <td>been</td>
      <td><code>AUX</code> (Auxiliary verb)</td>
      <td><code>VBN</code> (Verb, past participle)</td>
    </tr>
    <tr>
      <td>wanting</td>
      <td><code>VERB</code> (Verb)</td>
      <td><code>VBG</code> (Verb, gerund or present participle)</td>
    </tr>
    <tr>
      <td>to</td>
      <td><code>PART</code> (Particle)</td>
      <td><code>TO</code> (To)</td>
    </tr>
    <tr>
      <td>go</td>
      <td><code>VERB</code> (Verb)</td>
      <td><code>VB</code> (Verb, base form)</td>
    </tr>
    <tr>
      <td>there</td>
      <td><code>ADV</code> (Adverb)</td>
      <td><code>RB</code> (Adverb)</td>
    </tr>
  </tbody>
</table>

To be sure, spaCy's list of fine-grained POSs are fine-tuned for the English language, insofar as it follows the list of POSs (tagset) made for the Penn Treebank Project, which is a large annotated corpus of English text.[^1] The Penn Treebank tagset gives precise information on the way in which English language is constructed. I am not, however, as interested in the differences between different verb or particle forms as I am interested in which authors' texts are dominated by which POS categories. That a novel deviates from the rest of the corpus by its frequent use of adpositions, which primarily express spatial (e.g., "under," "atop," and "in") and temporal (e.g. "after," "during," and "before") relations, may exemplify a different form of embodiment than a novel that uses verbs considerably more than the corpus average. In order to look into these differences in frequency, I opt for spaCy's coarse-grained output, which uses the Universal Dependencies tagset.[^2] Table 2 displays the 19 POSs in the Universal Dependencies tagset.

<table class="typeindex">
  <thead>
    <tr>
      <th>Open class words</th>
      <th>Closed class words</th>
      <th>Other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>ADJ</code> (Adjective)</td>
      <td><code>ADP</code> (Adposition)</td>
      <td><code>PUNCT</code> (Punctuation)</td>
    </tr>
    <tr>
      <td><code>ADV</code> (Adverb)</td>
      <td><code>AUX</code> (Auxiliary verb)</td>
      <td><code>SYM</code> (Symbol)</td>
    </tr>
    <tr>
      <td><code>INTJ</code> (Interjection)</td>
      <td><code>CCONJ</code> (Coordinating conjunction)</td>
      <td><code>X</code> (Other)</td>
    </tr>
    <tr>
      <td><code>NOUN</code> (Noun)</td>
      <td><code>DET</code> (Determiner)</td>
      <td> </td>
    </tr>
    <tr>
      <td><code>PROPN</code> (Proper noun)</td>
      <td><code>NUM</code> (Numeral)</td>
      <td> </td>
    </tr>
    <tr>
      <td><code>VERB</code> (Verb)</td>
      <td><code>PART</code> (Particle)</td>
      <td> </td>
    </tr>
    <tr>
      <td> </td>
      <td><code>PRON</code> (Pronoun)</td>
      <td> </td>
    </tr>
    <tr>
      <td> </td>
      <td><code>SCONJ</code> (Subordinating conjunction)</td>
      <td> </td>
    </tr>
  </tbody>
</table>

In my analysis, I excised any token which was tagged as PUNCT, SYM, X, and NUM—the first two are typographical rather than syntactic or grammatical categories, the third denotes unrecognized POSs, and the fourth are not quite pertinent to a discussion of what words an author uses to string together sentences.[^3] I am, thus, left with 13 POSs.

In short, this blog post will examine not words, but the grammatical categories that underlie them. The fundamental principle is otherwise the same as the previous two posts: instead of looking at the *n* most frequent words or action verbs, I will be looking at frequencies of POSs.

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
    <i>Figure 2.</i>
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
