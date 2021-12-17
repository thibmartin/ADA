
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

The first step of the project was to import the Quotebank dataset as well as the gun violence related dataset into  2 separate DataFrame for further analysis. The quotes have been filtered over a set of key-words that relate to gun violence, see below the list of keywords and their distribution across dataset.  We also have filtered out incomplete and meaningless quotes, by removing quotes shorter than a certain word count threshold. For the gun violence dataset, we selected a subset of events based on the number of deaths. We choose a threshold of 8 deaths by visually analyzing the graphs.

We needed to identify whether the person quoted is sharing a positive or negative opinion with respect to gun ownership. Sentimental analysis was applied to quotes to see wheter they display a positive or negative sentiment. Furthermore, we performed emotional analysis to see the evolution of particular emotions, namely fear, anger, sadness, agression, joy, optimism. There already exist numerous sentiment classification algorithms online which are available and we need to select the best ones for the purpose of the project. We settled on Vader and Bert. Finally we classified the quotes in 3 categories: positive, neutral or negative, based on their score *insert classifier decision here*. Later on, we decided to apply emotion analysis to observe the evolution of different emotions around gun shootings events.

This website will present our work and our final analysis, for more informations or to see the source code, refer to the github repository (link on top of this article).

#### Dataset filtering
*needs complementary infos on lorenzo's work*

We started our work by filtering the Quotebank dataset with a set of keywords. We manually selected relevant terms generated from the Empath library, and applied these keywords to the complete dataset. We then searched for character encoding errors but didn't find anything too problematic. After working on subsequent analysis, we found out that it was best to remove quotes from unknown speakers as they generated a lot of noise. We also chose to remove quotes where the only keyword found was "firearm", "assault weapon" or "handguns", as they also seemed to generate some noise. When these keywords were associated with another word however we kept them. In order to remove meaningless quotes, we also removed quotes with less than 5 words, this value was chosen arbitrarly as we expect to only keep meaningful quotes by doing so. 
Here you can see the top 10 most present keywords throughout our dataset.
![keywords_repartitions](/images/keywords_graph.png)
*Take the correct image?*

#### Identifying subtopic - LDA

After treating the dataset, we wished to identify relevant subtopics. To do so, we performed topic detection using lda. However, we found out that 99.9% of our quotes lie in a single topic, characterized by keywords that are all relevant to gun violence (and often similar to the key words used to filter the data). This implies that our dataset doesn't contain too much noise. We plotted below the results of this analysis.
![LDA_analysis](/images/LDA_result.png)

#### Timewise comparison
The next step was to compare the time occurences of peak in quotations number to the events timeline in order to get a first intuition on the potential correlation. On the next graph you can observe the number of quotes related to gun events on the same timeline as the number of deaths in gun shootings event. The idea behind this comparaison is that events involving high number of deaths would have more impact on the number of quotations.

![image_title](/images/timeline_basic.png)

In general we observe that peaks in deaths from shootings events are correlated with an increase in the number of quotes related to gun violence. In short, people talk about gun violence more when there are a lot of shooting events.

#### Sentiment analysis - which quotes are on the dark side...

##### Vader vs Bert
![Vader](/images/vader.png) ![Bert](/images/bert.png)
*add method used and maybe fonctionment?* Add graphs of scores, etc...

##### Bert

*The following was on our milestone 2, not sure what to do about it*

We can plot the sentimental score of all quotes against time and observe the evolution of opinion. We can also add labels on the time axis for dates corresponding to significant shooting events (high death toll for example).

We can also select a few significant speakers who reoccur over the entire time frame of our dataset and plot their individual opinion evolution (or lack there-of).

Further analysis will be decided based on the results. We might explore the following supplementary questions …

How do different types of shooting events (officer involved, accident, mass shooting, …) affect public opinion, and which type matters most in the public eye?

What was the dialogue of Republican leaders with respect to gun violence and legislation before and after the 2016 election, specifically following mass shooting events?

