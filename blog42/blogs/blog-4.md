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
2. **Principal Component Analysis (PCA) on Topic Prevalence Values**: PyLDAvis is limited by the fact that it gives exclusive focus to topics, and overlooks the documents that underlie these topics. Accordingly, it gives us no information on which topic defines which novel(s). So as to resolve this problem, I took the means of the prevalence of each topic in every chunk in a given novel, for all 42 novels. This gave me the percentage of each topic's prevalence in each novel. I then put each novel, as represented by ten topic prevalence values, on two-dimensional and three-dimensional PCA plots. In addition to the 42 novels, I also placed the loadings for each topic on the plots. The loading of a given topic displays to what direction that topic pulls the novels which center on it.
3. **Heatmaps**: Using the topic prevalence values for each chunk, I produce 42 heatmaps that display the narrative trajectory of each novel according to the movement among topics. These heatmaps can also tell us about how broad the variance of topics in each novel is—some novels are bound to be rather one-note, whereas others will show more variance across topics.

## Analysis
### PyLDAvis Dashboard: What Can 10 Topics Tell Us About 42 Novels?
My plan for this blog post is that its analysis will be less focused on certain novels, as the two previous posts have been. The pyLDAvis dashboard displayed in [Figure 1](#Figure1) provides good ground to begin a post with such a general approach. The dashboard displays the 10 topics in varying levels of distinctiveness; the most distinctive topic on the Y (PC2) axis appears to be topic 10. The top ten words most relevant to this topic (visible when the topic bubble is hovered over or clicked) are "phone," "kid," "book," "mom," "guy," "beach," "bag," "stuff," "job," and "lot." These words pertain to a mix of topics, including modern concerns ("phone," "job"), family ("kid," "mom"); there is, less conclusively, also a set of words that may appear in conversations between women ("guy," "bag"). Topic 3, the most similar topic to topic 10, continues the discourse of modernity ("car," "door," "street," "truck") with an added note of words that seem to belong in a work of noir fiction ("gun", "cop"). Likewise, topic 9, the next topic most similar to topic 10, shares the discourse of family ("mother," "father"), and adds to it the discourse of domesticity ("house," "room," "kitchen," "table").

On the bottom-right quadrant of the plot, topics 4, 5, and 1 form a cluster. Topic 4 unambiguously pertains to antebellum racial injustice ("race," "slave," "master," "white," "slavery," "negro"). Accordingly, it is likely to be the primary topic of 19th century black novels like William Wells Brown's *Clotel* (1853), as well as later books that center on 19th century racial injustice, like John Oliver Killens's *Great Gittin' Up Morning* (1972). At first glance, topic 5 seems to have little similarity to topic 4; it is primarily concerned with internal being and morale ("soul," "spirit," "power") and orality ("voice," "story"). But, if you click on topic 5's bubble and hover over "soul," "spirit," "power," and "order"—doing so allows you to see the percentage distribution of a word across topics—you can see that topic 4 assigns these words a relatively high probability. The same could be said of topic 1, as regards "power" and "order." This suggests the existence of a subtheme among these three topics that are distinct in their primary themes. This suggests that the discourses of internality, orality, and racial injustice in some of the 42 novels tend to run parallel to a discussion of authority and power. We can add to this observation topic 1's primary themes of family (child, wife, son, father, mother, daughter, family, boy) and education (teacher, school). The discourses of internal character, racial injustice, family, and authority are easy to link to the early Black novel: in Paul Laurence Dunbar's *The Sport of the Gods* (1902), a Black family suffers the dissolution of their very sense of being due to a false allegation of theft levied by a powerful white man; in Frank J. Webb's *The Garies and Their Friends* (1855), one interracial and one Black family go through, and attempt to overcome, a series of misfortunes caused by racial prejudice. The theme of education is important in Frances E. W. Harper's *Iola Leroy* (1892), in which the protagonist works as a teacher, and in Sutton E. Griggs's *Imperium in Imperio* (1899), which opens with the employment of a racist white teacher at a school for Black children. Orality is more difficult to pin onto the early Black novel, as nineteenth- and early-twentieth-century Black thinkers exalted literacy and the written word as a tool of power and liberation. It may be that out of the two discourses of topic 5 identified above, it is the discourse of spirituality, character, and morale is what motivates the early Black novel, rather than orality, which appears on other novels. Or, it could be the case that an author like Killens, who writes of the nineteenth century in the post-literate latter half of the twentieth century, makes use of orality in his novel without any ideological scruples.

Note that, when you click on topic 1 and hover over such words as "child," "wife," "son," "father," "mother," the size of topic 9 either stays the same or increases, whereas topic 10 more or less disappears. Topics 1, 9, and 10 all deal with the discourses of domesticity and family; one thing that differentiates topic 10 from the former two is its use of shorter, perhaps more modern, words like "kid," "mom," and "boy." This suggests that there is a chronological element that separates these topics, and that a higher occurrence of topic 10 in a given novel is an indicator that that novel has a recent date of publication.

Topics 6, 2, and 7 seem to form another cluster on the bottom-left quadrant of the intertopic distance map. Topic 6, the largest topic out of the ten, appears to primarily concern the human body ("body," "hair," "arm," "mouth," "blood," "finger," "voice," "lip," among others). Specifically, certain words ("girl," "mother," "breast," "dress") suggest that this topic's primary discourse pertains to the female body, and it could be said that such words as "hair" and "lip" are associated more with femininity than masculinity. The undertones of sensuous romance ("blood," "finger," "lip," "shoulder," "pain") and motherhood/girlhood ("girl," "mother," "baby," "child," "doll") should also not be ignored. All in all, then, topic 6 stands as a nexus of topics that relate to femininity. It is altogether not very surprising this topic exists and occupies so much of the 42 novels—after all, 19 of the 42 authors in our corpus are women, and it's illogical to suggest that male authors don't write about women. What is perhaps surprising is the state of topic 2 as the male counterpart to topic 6. "Boy" stands as the fourth most relevant word for the topic, and is surrounded by words that primarly relate to adventure and the outside world ("light," "wind," "sea," "sun," "air," "land," "world", "stone," "tree"). Words pertaining to urban life ("landlord," "street," "crowd," "city") underline this connection between male embodiment and externality. This connection contrasts with Topic 6's focus on corporality and sensuality. The divide is reminiscent of our first blog's [discussion of a gender divide](./blog-1.html#a-gender-divide-light-darkness-the-city-and-style) that hinged on the binary of internality and externality between the female- and male-authored halves of our corpus. That being said, the word "mother" is also very relevant to topic 2, and could suggest that I am misreading the topic altogether. More research is needed, as they say; such research may be found in [an upcoming section]( -- this is where heatmaps go --).

Touching both topics 6 and 2, topic 7 seems to be interested in a very different kind of embodiment. "Slave" appears as the key word that relates to the body; this word is surrounded by words that primarily depict the environment of the slave ("field," "road," "horse," "house," "river," "land," "plantation," "crop," "cotton"). The words "war" and "soldier" suggest the unrest of the Civil War. If you hover over such words as "slave," "war," and "plantation," you will see that topic 4 is deeply linked to topic 7 in its discussion of slave life. And yet, they are far apart from each other on the map, at least on the X axis. This distance likely stems from the fact that topic 4 is interested more in the social and political dimensions of being enslaved and/or Black ("country," "freedom," "law," "education," "crime"), whereas topic 7 is interested in the bodily experience of being enslaved. It is intriguing that the topic model put these two matters in separate topics. It could be that they occur in different novels, for some reason. Or, maybe they co-occur in different parts of the same novel. They could even co-occur in same parts of a novel, but their different levels of occurrence in different novels could warrant their being separated.

You might have noticed the existence of "gwine" in topic 7. This word is an interesting inclusion, as it is not a noun at all; it's the present participle of "go" in early Black vernacular. In other words, it's the early Black counterpart to standard written English's "going." "Going," of course, doesn't appear in any topic whatsoever; it's been correctly parsed as a verb by the POS-tagger. The inclusion of "gwine," which is bound to occur in a novel depicting early Black American life, in topic 7 says something about the computational methods I use. Earlier, I had mentioned that a POS-tagger, being a machine learning model, is trained on annotated corpora of English-language text. "White" dialects of English provide the "norm" to which ethnic English varieties are the periphery in many different aspects of Anglophone existence; the field of machine learning is no exception. This fact has the unfortunate effect of making such machine learning models as POS-taggers potentially work suboptimally when used on Black and other non-white texts due to such texts' use of non-white English.

Finally, topic 8 stands as the smallest topic on the map; it also keeps its distance from the rest of the topics on the bottom-left quadrant. A closer look at the most relevant words reveals why topic 8 is unique: the words "douen," "allyou," "eshu", "hinte," and "tallpeople" occur exclusively in Nalo Hopkinson's *Midnight Robber* (2000); additionally, while "pickney" occurs in both *Midnight Robber* and Claude McKay's *Banjo* (1929), it occurs in the former novel over 100 times, while occurring in the latter novel only 3 times. "Pickney" is an informal word for "child" used in Caribbean varieties of English; similarly, "douen" is a spirit entity in Trinidadian folklore. "Allyou" is the plural of "you" in Trinidadian Creole—again, the confusion of the POS-tagger when faced with non-white English rears its head. "Eshu" is the deity of messengers and language in West African Yoruba religion. One may assume that Eshu should be a proper noun, but "eshu" is actually an analogue of the Internet in Toussaint, the Caribbean-inspired planet where *Midnight Robber* is set. "Hinte" and "tallpeople" are Hopkinson's inventions; the former refers to female douens, while the latter is used by Toussaint natives to refer to human settlers. Topic 8 is interesting in that it signals the uniqueness of *Midnight Robber*'s setting compared to the other 41 novels. At the same time, topic 8 comes with the disadvantage of not saying much about any novel other than that of Hopkinson's.

Now that we have delved into the predominant topics in our corpus, it's time to see 
