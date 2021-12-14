
<h1 align="center"> DataShot </h1>

<h2 align="center"> Public perspective on gun ownership and violence in relation to gun shooting events over time.</h2>

![image_title](/images/title_image.png)

### Introduction :

#### Abstract:

Gun ownership and regulation is a very divisive topic in the United States. It is clear that the country faces a problem with **gun violence**, this is reflected by the **yearly 14 000 deaths following gun related homicides** (statistic for 2019). However, people&#39;s opinions over how this problem should be tackled differ greatly.

In the case of our project, we basically want to know :

&quot;Who is thinking what and when?&quot;.

This includes, political **shifts in opinion**, the differing impacts of shooting event types in the public eye, and the **temporal evolution** of public opinions.

We wish to include an external dataset containing shooting events information and cross-reference with the QuoteBank dataset to see how given events influence what is said publicly, and how much these events take over the press.

#### Research questions:

How do general opinions in the media shift following gun shooting events?

How does the political sphere, and the opinion of significant public figures, evolve in relation to gun shootings?

#### Methods:

The first step of the project was to import the Quotebank dataset as well as the gun violence related dataset into  2 separate DataFrame for further analysis. The quotes have been filtered over a set of key-words that relate to gun violence, see below the list of keywords and their distribution across dataset.  We also have filtered out incomplete and meaningless quotes, by removing quotes shorter than a certain word count threshold. For the gun violence dataset, we selected a subset of events and create different categories for these events (officer related, mass shooting, accidents).

We will needed to identify whether the person quoted is sharing a positive or negative opinion with respect to gun ownership. Sentimental analysis was applied to quotes to see whether they are pro 2nd amendment/ in favour of gun ownership, or against. There already exist numerous sentiment classification algorithms online which are available and we need to select the best ones for the purpose of the project. We settled on *insert method here*. Finally we classified the quotes in 3 categories: positive, neutral or negative, based on their score *insert classifier decision here*.

Our aim was for each data entry from Quote Bank to be reduced to: speaker, date and sentimental score.

Here you can see the top 10 most present keywords throughout our dataset.
![keywords_repartitions](/images/keywords_graph.png)

#### Skeleton of development

The next step was to compare the time occurences of peak in quotations number to the events timeline. On the next graph you can observe the nummber of quotes related to gun events on the same timeline as the number of deaths in gun shootings event. The idea behind this comparaison is that events involving high number of deaths have more impact.

![image_title](/images/timeline_basic.png)

*insert comments and analysis here*

Now it's time for the biggest part of our analysis: the sentiment analysis. *add method used and maybe fonctionment?* Add graphs of scores, etc...


*The following was on our milestone 2, not sure what to do about it*

We can plot the sentimental score of all quotes against time and observe the evolution of opinion. We can also add labels on the time axis for dates corresponding to significant shooting events (high death toll for example).

We can also select a few significant speakers who reoccur over the entire time frame of our dataset and plot their individual opinion evolution (or lack there-of).

Further analysis will be decided based on the results. We might explore the following supplementary questions …

How do different types of shooting events (officer involved, accident, mass shooting, …) affect public opinion, and which type matters most in the public eye?

What was the dialogue of Republican leaders with respect to gun violence and legislation before and after the 2016 election, specifically following mass shooting events?

