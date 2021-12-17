
<h1 align="center"> DataShot </h1>

<h2 align="center"> Public perspective on gun ownership and violence in relation to gun shooting events over time.</h2>

![image_title](/images/title_image.png)

### Introduction :

#### Abstract:

Gun ownership and regulation is a very divisive topic in the United States. It is clear that the country faces a problem with **gun violence**, this is reflected by the **yearly 14 000 deaths following gun related homicides** (statistic for 2019). However, people's opinions over how this problem should be tackled differ greatly.

In the case of our project, we basically want to know :

&quot;Who is thinking what and when?&quot;.

This includes, political **shifts in opinion**, the impact of shooting events in the public eye, and the **temporal evolution** of public opinions.

We wish to include an external dataset containing shooting events information and cross-reference with the QuoteBank dataset to see how given events influence what is said publicly, and how much these events take over the press.

#### Research questions:

How do general opinions in the media shift following gun shooting events?

How does the political sphere, and the opinion of significant public figures, evolve in relation to gun shootings?

#### Methods:

The first step of the project was to import the Quotebank dataset as well as the gun violence related dataset into 2 separate DataFrame for further analysis. The gun violence dataset was obtained from the following [website](https://www.gunviolencearchive.org/). The quotes have been filtered over a set of key-words that relate to gun violence, see below the list of keywords and their distribution across dataset.  We have also further filtered out quotes related to certain keywords that seem to be not significantly related to gun violence topics and the ones under a certain number of words. For the gun violence dataset, we selected a subset of events based on the number of deaths. We choose a threshold of 7 deaths by visually analyzing the graphs.

We needed to identify whether the person quoted is sharing a positive or negative opinion with respect to gun violence related topics. Sentimental analysis was applied to quotes to see whether they display a positive or negative sentiment. Furthermore, we performed emotional analysis to see the evolution of particular emotions, namely fear, anger, sadness, agression, joy, optimism. There are already numerous sentiment classification algorithms easily accessible online which are available and we need to select the best ones for the purpose of the project. We settled on Vader and Bert. Finally we classified the quotes in 3 categories: positive, neutral or negative. Later on we decided to apply emotion analysis to observe the evolution of different emotions around gun shootings events.

This website will present our work and our final analysis, for more informations or to see the source code, refer to the github repository (link on top of this article).

#### Dataset filtering

We started our work by filtering the Quotebank dataset with a set of keywords. We manually selected relevant terms generated from the Empath library, and applied these keywords to the complete dataset. We then searched for character encoding errors but didn't find anything too problematic. After working on subsequent analysis, we found out that it was best to remove quotes from unknown speakers as they generated a lot of noise and given the scope of our analysis having quotes all from identified speakers might better represent the overall set of public opinions. We also chose to remove quotes where the only keyword found was "firearm", "assault weapon" or "handguns", as they also seemed to be associated with quotes not specifically related to gunshot violence topics. When these keywords were associated with another word however we kept them. In order to remove meaningless quotes, we also removed quotes with less than 5 tokens (which corresponded more or less to words in our case), this value was chosen arbitrarly as we expect to only keep meaningful quotes by doing so. 
Here you can see the top 10 most frequent keywords throughout our original dataset (before removing the key-words mentioned before).
![keywords_repartitions](/images/keywords_graph.png)

#### Identifying subtopic - LDA

After treating the dataset, we wished to identify relevant subtopics. To do so, we performed topic detection using lda. However, we found out that 99.9% of our quotes lie in a single topic, characterized by keywords that are all relevant to gun violence (and often similar to the key words used to filter the data). This suggest that our dataset might not contain too much noise. We plotted below the results of the LDA.
![LDA_analysis](/images/LDA_result.png)

#### Timewise comparison
The first step was to compare the time occurences of peaks in quotations number to the events timeline in order to get a first intuition on the potential correlation between the quotes and the gunshot violence dataset. On the next image you can observe the number of quotes related to gun events on the same timeline as the number of deaths in gun shootings event. The idea behind this comparaison is that events involving high number of deaths shuold have more impact on total number of quotations in a similar time window compared to other time windows non overlapping with relevant gunshot events.
![timelines](/images/timeline_basic.png)
After having explored to carry out our analysis over all the times series data of the quotes vs gun shots events deaths and getting unsuccessful results, we limited the analysis over a handful of significant events with high number of deaths as explained in a later section. We think this might be due to noise generated by gunshot violent related quotes not directly related to gunshot violence events but instead significant events like political elections or pro gun control protests picked up by media outlets.
In general we observe that visually most of the peaks in deaths from shootings events seem to correspond with an increase in the number of quotes related to gun violence with a few exception like for example the second highest gunshot events in terms of total number of deaths not really correlating with a big spike in the quotations number.

#### Sentiment analysis - which quotes are on the dark side...

##### VADER vs BERT
<img src="images/vader.png" width="250" height="250"/>  <img src="images/bert.png" width="250" height="250"/>

###### VADER 
We decided to use the VADER sentimental analysis tool as it is specifically attuned to sentiments expressed in social media where sentiment are very polarized and we would expect sentiment towards gun violence to also follow a bimodal sentiment distribution in a similar way to political arguments. We applied it to the filtered quotes and obtained the following result:

![vader_plot](/images/vader_plot.png)

As you can see, we obtain a really sentiment type distribution, with a skew towards negative sentiment. We suspect that it is a consequence of the Vader sentiment analysis struggling to properly classify the quotes. We would have expected a more polarized distribution in respect to sentimetn type. Looking trough a sample of quotes we think this might be at least in part due to the fact that some of the quotes are really hard to classify even as human readers. In addition to the previous point, Vader is a lexicon and rule-based model not able to account for the context of the words and given that we expect all the quotes to use strong words and expressions to to display strong feelings towards the subject that might bias the classification.

###### BERT
BERT to the rescue! We explored a more refined sentiment analysis tool by implementing a fine-tuned BERT on a google play store reviews dataset to do sentiment classification. We obtained the following results.
![bert_plot](/images/bert_plot.png)

Despite having been fine-tuned on a dataset with coming from a source different than media articles, we can clearly see that the sentiment distribution is much more balanced than VADER while being more polarized towards negative and positive than neutral. After also having looked at the labels assigned to a sample of quotes, we assume that it is a satisfying result for the purpose of our analysis.

We also transformed the quotes into 768-dimensional embedding vectors with a pretrained BERT expert model trained on Wikipedia and book corpus, and fine-tuned on SST-2 dataset in order to be employed for sentiment analysis tasks. We applied principal component analysis to the vector embeddings to reduce the dimensionality to 2.

#### Emotion extraction 
Fearing that sentiment score might be too restrictive, we chose to take our analysis further than a simple positive/negative split and investigated the main emotions that came up within quotes. We turned to the empath library and chose a range of emotions from the standard categories. These were: anger, aggression,  joy, pride, nervousness, suffering, fear, neglect, deception, disgust, optimism, sadness and disappointment. 

###### Entire dataset
We first ran lexicon.analyse() function on all quotes related to gun violence and plotted emotions that yielded significant results. Emotions were averaged over a month to reduce the noise from variation. This is shown below:

![emotion_extraction](/images/emotion_whole_data_set.png)

Aggression is what appears the most. This might be either a consequence of people speaking agressively about gun violence or because our chosen theme is inherently violent and matches with a lot of keywords that might be considered as "aggressive" or both to a different degree. Furthermore, we notice a slight increasing trend in agression over the entire dataset.
We also notice that speakers regularly express fear and sadness. Funnily enough, there seems to be a big peak in those two emotions in the time frame following the 2016 presidential election. We also note optimism and finally, we notice speakers usually display little anger, disapointment and joy.  

#### Shootings event and number of quotes correlation analysis

We did a selection of important shooting events first by visualising which events triggered a noticeable spike in number of quotes. This appears to concern events with at least 7 deaths apart from a few exception that we choose to drop as we consider having a statisfying number of event to analyse. We are left with the following 15 events:

<img src="images/dataframe.png" width="250" height="440"/>

To further quantify this relation between shooting events and the number of quotation, we do a time lagged cross correlation for each of the selected events on a time window of one week before and after the event.

This time lagged analysis allows us to take into account the fact that as we can expect, the quotation spike appear after the event.
![correlation](/images/correlation.png)

#### Sentiment shift around significant shooting events
We would just like to firstly note that during our extended analysis we often found high degree of noise in our sentiment shift results which might be due to a number of factors. At least in our case there is no way of accounting for significant shooting events abroad. Some seemingly "small" shootings can have big repercussions in the media if they are filmed, or happen at the "right" time. As already mentioned before there might be a series of unobserved events such as an election of a rally that cause a spike in gun violent related quotes in a time window which does not overlap with a relevant gun shooting event.

As explained in the VADER vs BERT section and after unsuccesful attempts at using the Vader labels, we grouped by the sentiment categories given by the fine-tuned BERT over every day in a time window of 1 week before and after the event. We implement this approach first for all the quotes and then for the quotes of particular speakers. We chose the 3 events who provided enough information before and after the event to draw some conclusions, which happens to be in top 4 events of the number of deaths.

![first](/images/first_bert_plot.png)
![second](/images/third_bert_plot.png)
![third](/images/second_bert_plot.png)
 
From the above plots, we can infer 3 main points. First, we see that the daily total number of quotes increases massively just after the shooting event. Second, the sentiment distribution is significantly skewed towards negative (for example in the first plot we see almost double the negative quotes in respect to the positive ones).  Thirdly from our limited number of samples we observe that the overall buzz and sentiment towards gunshot violence seem to come back to the levels precedent to the gunshot event just a couple of days after the date of the latter. This might be anectodal evidence of the typical media behavior jumping on hot topics one after the other in a matter of days.

We then tried to carry a similar analysis but this time only for the quotes assigned to a particular speaker. Despite having tested different speakers among the top ones in terms of total number of quotes we did not get satisfying results especially if compared with the case of all the quotes. We think this might have been caused by not having enough quotes in the selected time window and that significant public figure in particular politicians might express their opinion towards gun violence more seldomly than other topics (being a more politically sensitive topic in a way). A sampling bias might also affect significantly the results as politicians might express their opinion towards gun violence topics more often just after gun shooting events thus affecting the analsyis.

In addition we also tried to use the 768 dimensional vector embeddings from the BERT expert model, mean aggreagate them by day and reduce them into 2 dimensions with PCA to try to see the sentiment opinion evolution of selected speakers before and after some relevant gunshot violence events but did not again arrive at satisfying results which might be caused by the exact same reasons highlighted in the previous paragraph.

###### Significant shootings
We then ran the same analysis for the fifteen "significant" gun shooting events in the five days before and after each event. The emotion scores were averaged over a day. The results are shown below:

![emotion_extraction](/images/emotions_big_events.png)

Because of the already mentioned factors the results must be taken with a grain of salt as also shown by the plots' "noisiness". Nevertheless, we often notice that there is a small spike in emotions following a shooting event. This is most notable for aggression, sadness and anger.

A small note, the event on the 12th of June 2016 generates an odd plot, this is not a coding error, we genuinely have not identified any quotes relating to gun violence on the five days prior to this event. It seems to be part of the numerous "blanks" that appear in 2016. 

###### Significant speakers
Finally, in the aim of understanding the opinions of significant public figures, we identified the top 10 speakers who appear the most in our dataset and plotted their individual emotion scores averaged over a day. The results are shown below:

![emotion_extraction](/images/emotions_big_speakers.png)

Despite high variance and some blank periodes we can nonetheless extract some information from these graphs.

Barack Obama is often sad when talking about gun violence. 

Hillary Clinton is aggressively active on the gun violence topic prior to the 2016 election, but suddenly falls silent following her loss. Perhaps she was using this highly politiced topic as a tool to gain following among the community that supports wider gun control?

David Hogg appears from nowhere in 2018 following the Stoneman Douglas High School shooting in which he was a victim, and we notice he talks about gun violence with a lot of aggression and fear.

Richard Blumenthal and Chris Murphy are two members of the US Senate, who are in favour of gun control. They seem to regularly talk about the subject, and do so expressing a lot of aggression and fear. 

Alan Gottlieb, a conservative political activist and gun rights advocate also regularly talks about gun violence. He seems to do so expresing regular suffering, fear, agression, but also optimism ... This last emotion is not displayed as often be speakers who would favour gun control.

Bernie Sanders seems to talk about gun violence almost exclusively in the lead up to the 2016 and 2020 elections. Perhaps a political tool?

#### Conclusion
After so many tears, blood and sweat...

We observed a correlation between a shooting event and the number of quotes, but as we know, correlation doesn't imply causation. Even though we tried to reduce the noise as much as posssible, there are still too many hidden cofactors that could bias our analysis. Outside events can have a lot of impact on the number of quotes, either directly (in the case of the gun violence rally in early 2018 for example) or indirectly, because if another significant event generates a lot of quotes, we will have less focus on the gun violence topic. 

Nevertheless, we identified a significant increase in the number of quotes related to gun violence, a significant shift in sentiment towards negative values as well as a spike in negative emotions (i.e. aggression, sadness and fear) in correspondance to relevant gunshot violence events. Furthermore, we showed that sentiment distribution seem to quickly shifts back to the "norm" in just a matter of days after the events.

We were not able to get satisfying results for the sentiment evolution of only the quotes assigned to particular speakers but noticed signicant figures display varying types of emotion with respect to gun violence. Alan Gottlieb, a gun activist, displayed more hope than others, while Barack Obama showed more sadness. We've also been given hints that gun violence is used as a political tool by presidential canditates, such as Bernie Sanders and Hillary Clinton. 
<p align="center">
  <img src="images/barrack-obama-obama-out.gif"/>
</p>
