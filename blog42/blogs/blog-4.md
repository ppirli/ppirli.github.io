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

The second condition also helps with the imbalanced number of chunks produced by each novel across the corpus. The imbalance is greatly caused by Leon Forrest's *Divine Days* (1992), which produces a whopping 193 chunks compared to the corpus's mean of around 30 chunks. Forrest's gargantuan novel is an outlier in the corpus, and as [Table 1](#Table1) shows, removing *Divine Days* from the corpus decreases the standard deviation of the dataset considerably. Removing Forrest's novel also makes the dataset less skewed (as evidenced by a increased proximity between the mean and the median). Less skewness in the dataset means that the data is more symmetrical, which in turn provides proof that *Divine Days* is an outlier. Nevertheless, removing *Divine Days* from the corpus altogether would ruin this series of posts' focus on the 42 novels in the *42 Books / 42 Years* corpus. I could alternatively pick only a random number of chunks from *Divine Days* that is close to the mean, but that would most likely mean losing important information residing in the left out chunks. In comparison to these options, making it so that words that appear in above 80% of the chunks makes *Divine Days*'s chunks less likely to influence the dataset on the whole.

<table id="Table1">
  <caption>
    <i>Table 1.</i>
    Placeholder caption
  </caption>
  <thead>
    <tr>
      <th scope="col"></th>
      <th scope="col">With *Divine Days*</th>
      <th scope="col">Without *Divine Days*</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Mean of chunks per novel</th>
      <td>30.57</td>
      <td>26.61</td>
    </tr>
    <tr>
      <th scope="row">St. dev. of chunks per novel</th>
      <td>28.55</td>
      <td>12.65</td>
    </tr>
    <tr>
      <th scope="row">Minimum number of chunks</th>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <th scope="row">First quartile of chunks per novel (25%)</th>
      <td>20</td>
      <td>20</td>
    </tr>
    <tr>
      <th scope="row">Median number of chunks per novel (50%)</th>
      <td>23.5</td>
      <td>23</td>
    </tr>
    <tr>
      <th scope="row">Third quartile of chunks per novel (75%)</th>
      <td>35.75</td>
      <td>35</td>
    </tr>
    <tr>
      <th scope="row">Maximum number of chunks</th>
      <td>193</td>
      <td>57</td>
    </tr>
  </tbody>
</table>

Another, admittedly more important, matter that makes the imbalance created by Forrest's novel less significant lies in a preprocessing step I didn't take. It is common in topic modeling applications to limit the vocabulary of the topic model to the *n* most frequent words in the corpus (usually *n* is around 5,000). By skipping this step, I make it so that chunks from other novel's don't have to fight with *Divine Days*'s chunks in order to be selected in the model's vocabulary. The issue of very specific words is virtually resolved by the exclusion of proper nouns and any noun that appears in fewer than 4 chunks, anyhow.

### On What Comes After Preprocessing
What comes after this rather boring and convoluted preprocessing step is rather simple: I use Latent Dirichlet Allocation (LDA) to create a topic model with 10 topics.[^3 lda] Once the topic model is created, I use three visualization methods:

1. **PyLDAvis**: PyLDAvis—the Python port of LDAvis mentioned above—allows me to see the most salient words in the topic model, as well as words that are the most relevant to each topic. A word's salience is computed according to a formula that checks its frequency across the corpus as well as its distinctiveness across topics. In other words, high overall frequency + high specificity = high salience. Relevance is computed according to a formula that looks for either words that are the most representative for that topic. The relevance metric is affected by the lambda (λ) parameter, which can be between 0 and 1: a lower λ value looks for words exclusive to a given topic, whereas a higher λ value looks for words that occur most frequently in that topic. The λ value of 0.6 is often used as a sweet spot for purposes of the human interpretability of the topic, as identified by Sievert and Shirley[^4]. Following this, I take 0.6 as the default λ value. Aside from providing the most salient and relevant words, pyLDAvis also visualizes the distance between topics (based on the co-occurrence of words among topics) on a two-dimensional plot that uses multidimensional scaling (MDS)[^5] for dimensionality reduction.
2. **Principal Component Analysis (PCA) on Topic Prevalence Values**: PyLDAvis is limited by the fact that it gives exclusive focus to topics, and overlooks the documents that underlie these topics. Accordingly, it gives us no information on which topic defines which novel(s). So as to resolve this problem, I took the means of the prevalence of each topic in every chunk in a given novel, for all 42 novels. This gave me the percentage of each topic's prevalence in each novel. I then put each novel, as represented by ten topic prevalence values, on two-dimensional and three-dimensional PCA plots. The latter PCA plot is new to this series of blog posts, and tells us a bit more about what PCA plots can and cannot show.
3. **Heatmaps**: Using the topic prevalence values for each chunk, I produce 42 heatmaps that display the narrative trajectory of each novel according to the movement among topics. These heatmaps can also tell us about how broad the variance of topics in each novel is—some novels are bound to be rather one-note, whereas others will show more variance across topics.

## Analysis
### PyLDAvis Dashboard: What Can 10 Topics Tell Us About 42 Novels?
My plan for this blog post is that its analysis will be less focused on certain novels, as the two previous posts have been. The pyLDAvis dashboard displayed in [Figure 1](#Figure1) provides good ground to begin a post with such a general approach. The dashboard displays the 10 topics in varying levels of distinctiveness; the most distinctive topic on the Y (PC2) axis appears to be topic 10. The top ten words most relevant to this topic (visible when the topic bubble is hovered over or clicked) are "phone," "kid," "book," "mom," "guy," "beach," "bag," "stuff," "job," and "lot." These words pertain to a mix of topics, including modern concerns ("phone," "job"), family ("kid," "mom"); there is, less conclusively, also a set of words that may appear in conversations between women ("guy," "bag"). Topic 3, the most similar topic to topic 10, continues the discourse of modernity ("car," "door," "street," "truck") with an added note of words that seem to belong in a work of noir fiction ("gun", "cop"). Likewise, topic 9, the next topic most similar to topic 10, shares the discourse of family ("mother," "father"), and adds to it the discourse of domesticity ("house," "room," "kitchen," "table").

On the bottom-right quadrant of the plot, topics 4, 5, and 1 form a cluster. Topic 4 unambiguously pertains to antebellum racial injustice ("race," "slave," "master," "white," "slavery," "negro"). Accordingly, it is likely to be the primary topic of 19th century black novels like William Wells Brown's *Clotel* (1853), as well as later books that center on 19th century racial injustice, like John Oliver Killens's *Great Gittin' Up Morning* (1972). At first glance, topic 5 seems to have little similarity to topic 4; it is primarily concerned with internal being and morale ("soul," "spirit," "power") and orality ("voice," "story"). But, if you click on topic 5's bubble and hover over "soul," "spirit," "power," and "order"—doing so allows you to see the percentage distribution of a word across topics—you can see that topic 4 assigns these words a relatively high probability. The same could be said of topic 1, as regards "power" and "order." This suggests the existence of a subtheme among these three topics that are distinct in their primary themes. This suggests that the discourses of internality, orality, and racial injustice in some of the 42 novels tend to run parallel to a discussion of authority and power. We can add to this observation topic 1's primary themes of family (child, wife, son, father, mother, daughter, family, boy) and education (teacher, school). The discourses of internal character, racial injustice, family, and authority are easy to link to the early Black novel: in Paul Laurence Dunbar's *The Sport of the Gods* (1902), a Black family suffers the dissolution of their very sense of being due to a false allegation of theft levied by a powerful white man; in Frank J. Webb's *The Garies and Their Friends* (1855), one interracial and one Black family go through, and attempt to overcome, a series of misfortunes caused by racial prejudice. The theme of education is important in Frances E. W. Harper's *Iola Leroy* (1892), in which the protagonist works as a teacher, and in Sutton E. Griggs's *Imperium in Imperio* (1899), which opens with the employment of a racist white teacher at a school for Black children. Orality is more difficult to pin onto the early Black novel, as nineteenth- and early-twentieth-century Black thinkers exalted literacy and the written word as a tool of power and liberation. It may be that out of the two discourses of topic 5 identified above, it is the discourse of spirituality, character, and morale is what motivates the early Black novel, rather than orality, which appears on other novels. Or, it could be the case that 

child/kid -- mother/mom
