---
layout: post
title:  "An agent-based model of user interactions in a simulated polarized Reddit environment"
---

<blockquote>
<small><i>Abstract: This study constructs an agent-based model in a Reddit-like social media environment. Agents and their posts are oriented around a single, virtual, polarizing issue, with polarities scored in [-1,1]. Agent behavior and various system metrics are analyzed with an interest in homophilic behavior: i.e. the tendency for agents of similar polarity to dominante the main interaction unit, a thread or reply tree. The model is successful in replicating a prominent result from the literature: that homophily and system-wide happiness are correlated. We also find that (a) high levels of post curation can have systemic negative effects, reflected in numerous metrics, and (b) permitting agent churn, coupled with sufficient agent policing-downvoting, produces marked majority-polarity dominance effects, with eventual near-complete homophily due to minority-polarity attrition. As a corollary, when downvoting is disallowed the runaway majority-dominance effect disappears, but this comes at the expense of substantially higher churn of polar-neutral agents.</i></small>
</blockquote>


This informal study examines the effects of various agent- and environmental control parameters on homophilic agent behavior in a simulated social media setting.


<!--more-->

{% include mathjax.html %}

<style>
  .simpletable {
      width: auto; /* Ensures the table is only as wide as its content */
      font-size: 12px; /* Controls text size */
      border-collapse: collapse; /* Removes extra spacing between borders */
  }

  .simpletable th, .simpletable td {
      padding: 2px 5px; /* Reduces padding to keep it compact */
      border: 1px solid black; /* Adds a border for visibility */
      text-align: left;
  }

  caption {
	  caption-side: bottom;
      font-size: 14px; /* Slightly larger caption for readability */
	  margin-top: 5px;
  }
</style>


## Introduction

Since the beginning of large-scale social media in the late 1990s, researchers have been studying user interactions in this relatively new communcation space. A common subfield of research in this area is opinion dynamics, which is interested in the development and spread of opinions on social networks. The study of the formation and propogation of opinions has importance for many stakeholders, including companies--which may have an interest in consumer trends or brand opinions--and political entities--which may seek to capitalize on and even influece raw political sentiment.

This work is interested in a close associate of opinion formation: the homogeneity of opinion in the user's environment. The literature generally refers to this type homogeneity as homophily. A homophilic social network features like-minded users clustering together under some measure of distance, such as the number of links in a social network graph. A heterophilic social network, by contrast, features relatively high mixing among users of differing opinions in any given network region. It is reasonable to assume that, just as user opinions might inform their tendency to cluster, clustering might also influence a user's opinions. Popular related terms like filter bubbles or echo chambers allude to extremes in such aggregations, where the environment has become so homogeneous that opinion formation and expression may be artificially constrained.


## Existing research

The existing research on homophily in social networks is quite broad, but it will be useful to highlight some studies relevant for this work.

In <a href="#Haque">[Haque, 2023]</a>, an agent-based model was constructed that allowed altering both the agents' tolerance for differing opinions, and the opinions the agents were exposed to--"congenial" for like-minded opinions, and "cross-cutting" for opinions that did not agree with the agent's own. They conclude that higher user tolerance both reduces user polarization and reduces user satisfaction--an environment that exposes users to contrary opinions is "not necessarily the environment that produces enthusiastically participatory individuals." This is supported by research suggesting that people who participate in social networks featuring higher levels of disagreement are less likely to participate in politics <a href="#Mutz_a">[Mutz, 2002a]</a>, <a href="#Mutz_b">[Mutz, 2002b]</a>. The study by <a href="#Haque">[Haque, 2023]</a> also concludes that higher selective exposure leads to higher polarization, and lower user reach; combined with higher tolerance, they find that "surprisingly, higher tolerance also leads to a more homophilic social network." Their work also cites earlier research <a href="#Stroud">[Stroud, 2010]</a>, that concludes congenial exposure aggravates confirmation bias and further polarizes opinions. Other research cited in <a href="#Haque">[Haque, 2023]</a>, namely <a href="#Coscia">[Coscia, 2022]</a> and <a href="#Kim">[Kim Y, 2015]</a>, indicates that either / both cross-cutting exposure and a higher user tolerance for cross-posts reduces polarization.

In <a href="#Morales">[Morales, 2021]</a>, a study of real-life Reddit interactions was conducted around the time of the 2016 US elections. They also found slight support for the hypothesis that exposure to cross-cutting political opinions is associated with diminished political participation. They also highlight an effect that appears in other, similar studies, that cross-cutting can have a "backfire" effect: if a user is exposed to "highly" radical opinions from the other side, they may become more firmly entrenched in their own side's opinions. The main purpose of the study was to examine the extent that users from each of two camps (supporters of Hilary Clinton or Donald Trump) engaged with one another. The authors found the prevalence of homophilic echo chambers was lower than expected, though showed asymmetry between the two camps.

In <a href="#Simchon">[Simchon, 2022]</a>, they find "substantial evidence" of a tendency for homophily to arise in the context of politically contentious topics, and, like <a href="#Morales">[Morales, 2021]</a>, that cross-cutting exposure can ironically increase polarization, via a backfire effect.

This study explores factors that affect homophily around a single, virtual, polarizing topic. (The topic is "virtual" in the sense there is no concrete issue under discussion--the only salient feature of the topic in the simulation are the users' relative polarity scores with respect to the topic.) The social media environment was constructed to resemble the interaction dynamics of the Reddit platform. Reddit was chosen because the platform is quite popular at the time of writing, and, unlike for instance Facebook, the degree of inter-user networking is relatively low (e.g. <a href="#Treen">[Treen, 2022]</a>, which shows relatively few consistent interactions between any given pair of Reddit users), making it less likely inter-agent familiarity ("friends") might affect the behavior of a given user. This allows dropping the complexity of networks and inter-agent links from the simulation.

The model chosen for this study is an agent-based model. Each user agent is assigned various parameters determining their behavior, most notably a polarity score in the range [-1,1], indicating their opinion on the single central, virtual topic. Agents then make virtual posts in individual trees, or threads, in reponse to existing posts in the thread.  (The posts are "virtual" in the sense there is no text body.) Agent posts receive a polarity score, again in the range [-1,1], where the polarity of the post is in tunable agreement to the agent's own polarity--ie agents are likely to create a post of polarity similar to their own. Each post may also be voted on, once, by any given agent, with upvoting always allowed, and downvoting permitted or not permitted in a given simulation run. As with commenting, agents will cast votes according to their own polarity score: for example, an agent of polarity -1 will be likely to upvote a post with polarity -1, and likely downvote a post with polarity +1. Agents can be further tuned to seek out posts to reply to that are likely to produce a reward--i.e. an upvote--and unlikely to produce a punishment--i.e. a downvote. Other important tuning parameters include policing behavior, where an agent deliberately seeks out cross-cutting posts for the purpose of downvoting, as well as altruism, which refers to agents suspending their concerns for punishment (downvotes), in the interest of making cross-cutting posts.

Other agent-based models reviewed for this study included <a href="#Macy">[Macy, 2021]</a>, which used agents with differing alignments on 10 different issues, all related to a single polarity score (similar to how a given left-right political polarity involves multiple political issues). Tuning parameters included party (polarity) affiliation strength, and tolerance for disagreement. In this model, a distance metric was created, to measure proximity of agents in the (10-dimensional) issue space. Agents may move closer or farther from neighbors in this issue space, depending on their tolerance for disagreement and their party affiliation strength. Of especial interest were "bimodal" outcomes, where agents tended to crystalize into, roughly, two different camps--this necessarily involved compromises in the opinions of individual agents on given issues, to where the dimensionality somewhat collapsed into one--i.e. two distinct opinion clusters across all 10 issues.

In <a href="#Zhu">[Zhu, 2022]</a>, an agent-based model is used to examine agent expressivity under reward seeking behavior. Agents are allowed to choose whether to express their opinion or not, depending on receiving anticipated favorable or unfavorable returns from the social network. Those that estimate favorable reception, will express their opinions, possibly under some degree of modification. Those that estimate an unfavorable reception will remain silent. One result was that agents with extreme opinions were more likely to express their opinions than agents with more neutral opinions.

The authors in <a href="#Murdock">[Murdock, 2024]</a> create an agent-based model most similar to the one used in this study. The model's main purpose was to simulate a Reddit environment, with agents tunable for their tendencies toward posting (i.e. lead posts, or topic-starter posts), voting, and commenting (i.e. replying to the posts of others). The study develops the model, and tests it against known Reddit datasets for realism. The study however does not explore the model in specific contexts, such as political polarization.

As a final note, the primary research question for this study is to consider what model control inputs affect homophily the most. However, since the modeling assumptions were somewhat generous, in that ideally more data would be used to fortify the a priori assumptions, this study should be viewed more for hypothesis generation than firm research conclusions beyond what the existing literature offers.


## Setup

The model used for this study was built from scratch, using Python. Code for the project is available [here](https://github.com/tmwine/reddit_simulation). However, agent-based modeling packages do exist, and the curious reader may consider, for example, [NetLogo](https://ccl.northwestern.edu/netlogo/), and the [Construct API](https://github.com/CASOS-IDeaS-CMU/Construct-API).

**Major modeling assumptions:**
- the primary element in the model is a Reddit-like tree, composed of a single, model-generated lead post (the "OP"), with zero or more recursive dependent posts in reply; as the simulation runs, agents vote on and add posts to the various trees; see Figure 1.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/tree_diagram_2.png" alt="a typical Reddit-like thread, depicted as a directed graph with root node at top" width=600><p>Figure 1: an example tree; posts are made by individual agents in reply to existing posts, with the original post (OP) at the root (top) of the tree; shown in each post is the post's polarity score, total upvotes, total downvotes, base visibility score, and an entry for the ID of the agent who authored the post (the OP is randomly generated, and has no agent author); visibility scoring, discussed later, involves (a) applying an attenuation factor of 1/2 for any posts with net upvote minus downvote score below some threshold (here, 0), and (b) multiplying the base visibility score of a given post by the attenuation scores of any and all parent posts (recursively); (graphic generated by ExcaliDraw)</p></div>

- simulation process:
	- an initial agent pool (in practice, of size 60) is created, with probabilistically derived agent characteristics
	- each round features a randomly chosen subset of agents (say half of the 60 available)
	- the simulation is run over some fixed number of rounds (in practice, 100)
	- to simulate a user browsing through posts, each "active" agent has a skim list selected from all posts in all the trees, with favorability toward popular trees (measured by upvote-downvote aggregate score)
	- the skim list is split into "reward intent" and "other," according to the polarity and visibility of the trees posts
	- to simulate a user reading posts, and possibly voting (up/down) on the post or replying to the post, the agent chooses a subset of the skim list plus a subset of any posts in direct reply to their own for reward intent, policing, altruism, and commenting
- the "reward intent" skim list, and subsequent engagement when a given agent replies to posts in their personalized skim list, simulates agents seeking to maximize their scores (the score amounting to the difference in net upvotes minus net downvotes across all posts the agent has made so far)--agents primarily seek to interact with posts that have polarity "close" to their own
- more polarized agents (those with polarity scores near -1 or +1) are programmed, on average, to exhibit more activity (voting and commenting) than more neutral agents (those with polarity scores nearer to 0); this follows indications in the literature that polarized individuals tend to be more active
- agents are allowed to churn (leave the simulation, or retire from activity), depending on a parameter setting (nonzero allows churn); if the churning parameter is nonzero, a fixed number of agents with the lowest relative scores, the number determined by the parameter, will leave the simulation each round (we emphasize that the score threshold is relative, not fixed--this is from assuming particpants in social media mainly consider their relative, not absolute score as important for feedback and reward)
- a tree is retired after its posts have been read by some majority of the agents (for example, 50%)
- "no network"--this model assumes no effects from inter-agent (e.g. "friend" or neighbor) effects, with the work of <a href="#Treen">[Treen, 2022]</a>, mentioned above, guiding this choice for our Reddit-like environment
- post visibility
    - is determined by (a) the score of the post, and (b) the scores of parent posts, all the way up to the root (OP) of the tree; in this way, a low scoring post can inhibit the visibility of any dependent post lower in the tree
    - no time value to posts--in actual use, Reddit places a time value on its posts: more recent posts obtain more value than older posts; this model ignores the time value of posts
- no moderation, no trolls, and everything is on topic--social media platforms typically encounter user misbehavior, including users who are intentionally disruptive or manipulative in polarized environments, possibly acting in coordinated groups; this model does not attempt to simulate those behaviors


## Model details

A given model run begins with generating a fixed number of agents, with randomly-determined parameters that include, polarity, and voting, commenting, and post-reading tendencies. An initial set of trees are also generated, with randomly-chosen polarities for the lead post (OP). Each active agent for each round first has a skim list generated, a subset of all the posts of all the trees (conditioned on the agent not already having read them). The skim list simulates the typical user inclination to skim through or browse posts of initial interest, which the user may then choose to comment or vote on. The skim list also allows simulating content curation, as with social media algorithms, by selectively presenting posts the agent may be more likely to engage with. The agent then proceeds to select posts from the skim list, depending on intent: reward seeking, altruism, policing, or replies--which meanings are explained in the control variables section just below.

### Control variables

The model contains a total of 6 principal control variables, with multiplicities {3,4,4,4,2,3}, for a total of 1152 combinations of control variable settings. In detail:

- SIG:
	- controls width of a Gaussian; used for skim list generation, based on polarity gating
	- posts with polarities within SIG-determined, random distance of the agent's polarity are gated into a reward intent list, deemed likely to result in favorable upvotes, and an "other" list, consisting of all other posts in the skim list (less likely to produce upvote, or reward)
	- for levels, SIG=0.10 would be regarded as very strict curation / avoidance of cross-posts, while SIG=0.5 and higher are fairly loose curation / openness to cross-posts
	- values: one of 3, in {0.1,0.35,0.6}

- po_attn, al_attn, re_attn: 
	- these control three additional agent tendencies, enacted once the skim list has been chosen and skim posts assigned to either reward intent, or "other" categories
	- (**note** these are somewhat unfortunately named after "attenuation," but where higher values (closer to 1) for these parameters actually refer to lower attenuation)
	- po_attn--policing tendency; the agent's propensity for reviewing cross-cutting posts in the "other" list for purposes of downvoting
	- al_attn--altruism tendency; the agent's propensity for replying to or voting on posts in the "other" list, in a way presumably contrary to pursuing rewards (upvote)
	- re_attn--reply tendency; the agent's propensity to reply to posts made in reply to their own posts (i.e. reply chaining)
	- ranges: each in [0,1]; specifically, each can be one of 4, {0.10,0.25,0.5,1.0}

- the (nm_lo, nm_hi, vot_rad) triplet:
	- this is also based on a probabilistic Gaussian, and controls how tightly an agent bases their commenting and voting around their own polarity; if this setting is tight, or narrow, an agent for example of polarity +0.5 is less likely to upvote a post of polarity -0.5 than if the setting is loose, or broad
	- this triplet can have one of two values: (0.010,0.030,0.20) (tight setting), and (0.025,0.075,0.35) (loose setting)

- churn_prop
	- churn parameter; nonzero values allowed a proportional number of the lowest scoring agents to churn or retire from the simulation (go inactive)
	- values: of 3, in {0.0,0.0025,0.005}

Note an additional parameter, "sensitivity," was not used, but is available; this control parameter allowed adjusting polarity drift of agents: agents polarities were allowed to drift in proportion to this parameter, in accord with some average of the polarity of the content the agent interacted with.

### Output variables of interest

At the end of each simulation run, in addition to the control variables, recorded variables included the following:

- total upvotes and total downvotes accumulated over all posts in all trees
- agent retirement counts, binned by polarity
- total comments posted over all trees for all rounds in the simulation
- pln_prp--the homophily measure; for each tree, the proportion of majority-side posts is computed, and the average of these proportions is taken over all the trees (e.g. a tree with 5 posts of polarity < 0, and 10 posts of polarity >= 0 (making this the majority side), the proportion of majority-side posts is 10/15=2/3)


## Results and analysis of simulation runs

The main simulation runs were performed in two batches: one with downvoting allowed (batch A), and the other with downvoting turned off (batch B). For the batch with downvoting turned on, the entire set of 1152 different control parameter combinations was used. For the batch with downvoting turned off, since policing behavior is irrelevant if agents cannot downvote, the po_attn parameter was fixed at 1.0. The total number of different control parameter combinations in this case was 288. For both batches, 60 agents were initially generated, the simulations were run over 100 rounds. If churn_prop was non-zero, for every k agents retired at the end of a round exactly k new agents were created to replace them.

### Homophily and polarization, in brief

To understand how the polarity measure pln_prp relates to tree homophily, Figure 2a-c shows plots from several simulation runs at varying control parameter values. At values pln_prp=0.70, per-tree homophily, averaged over all trees in the simulation above a minimum post count cutoff of 10, is relatively low. At pln_prp=0.80, the trees are evidently becoming more homophilic, or imbalanced in their polarity distributions. At the extreme, pln_prp=0.95, almost all trees are seen to be markedly homophilic.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/pln_prp_fig_1.png" alt="stacked bar plot showing frequencies of posts, binned by polarity quarters, [-1,-0.5), [-0.5,0.0), [0.0,0.5), [0.5,1.0], for the most active trees" width=400><p>Figure 2a: Frequency of posts, binned by polarity quarters, [-1,-0.5), [-0.5,0.0), [0.0,0.5), [0.5,1.0], for the most active trees; pln_prp=0.71--with each tree being approximately balanced in post frequencies over the polarization bins, this figure depicts a relatively low homophily simulation run</p></div>

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/pln_prp_fig_6.png" alt="stacked bar plot showing frequencies of posts, binned by polarity quarters, [-1,-0.5), [-0.5,0.0), [0.0,0.5), [0.5,1.0], for the most active trees" width=400><p>Figure 2b: Frequency of posts, binned by polarity quarters, [-1,-0.5), [-0.5,0.0), [0.0,0.5), [0.5,1.0], for the most active trees; pln_prp=0.81--with each tree showing greater imbalance in post frequencies by polarity bin, this figure depicts a mid-level degree of homophily in this simulation run</p></div>

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/pln_prp_fig_7.png" alt="stacked bar plot showing frequencies of posts, binned by polarity quarters, [-1,-0.5), [-0.5,0.0), [0.0,0.5), [0.5,1.0], for the most active trees" width=400><p>Figure 2c: Frequency of posts, binned by polarity quarters, [-1,-0.5), [-0.5,0.0), [0.0,0.5), [0.5,1.0], for the most active trees; pln_prp=0.95--with each tree fairly highly dominated by one or the other polarity extremes, <0.0 or > 0.0, this figure depicts a relatively high degree of homophily in the simulation run</p></div>

Agents at the polar extremes were designed to be more active. Figure 3 shows the distribution of agent activity, as a function of polarity, for batch A (with downvoting off), with both batches showing a similar effect. As intended, polarized agents showed more activity (in this case the number of posts they generated) than neutral agents. Agents in the polar extremes, near +/-1, commented approximately twice as frequently as neutral agents, with polarity near 0.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1a_1.png" alt="column plot over differing agent polarities, showing frequency of posts" width=300><p>Figure 3: Agent posting activity as a function of agent polarity score; in a reflection of results from the literature, the model was designed to correlate agent posting and voting activity with the absolute value of agent polarity score</p></div>

The simulation generates uniformly-random polarity scores for each agent, in the range [-1,1]. Figure 4 shows the effect of random skew in the polarity distribution of agents on the overall polarity. Overall simulation polarity was computed by averaging the polarity of all posts over all trees. The figure represents the data for batch A, but both batches are similar in this respect. There is a clear positive correlation between initial imbalances in the agents' polarity distribution and the overall post polarity average over all the trees. High curation (low SIG, in this case $0.1$) sees the highest slope.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1b.png" alt="scatterplot showing positive correlation between agent polarity skew at start of simulation and polarity of individual trees at end of simulation" width=400><p>Figure 4: Showing the effect of initial skew in agent polarity distribution on end-of-simulation tree post polarization; shown at varying levels of the curation parameter, SIG (SIG=0.1 is the highest level of curation allowed)</p></div>

Another effect of interest is how the polarity of agents relates to average per-agent net reward (total upvotes minus total downvotes). Tables 1a-b shows that net reward was consistently higher for polars than neutrals. Again this was for batch A, but a similar effect was observed for batch B (with downvoting off).

<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1.00, -0.67)</td>
        <td>[-0.67, -0.33)</td>
        <td>[-0.33, 0.00)</td>
        <td>[0.00, 0.33)</td>
        <td>[0.33, 0.67)</td>
        <td>[0.67, 1.00)</td>
    </tr>
    <tr>
        <td>0</td>
        <td>32.3512</td>
        <td>27.0763</td>
        <td>2.2959</td>
        <td>0.972765</td>
        <td>25.1703</td>
        <td>42.5762</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>41.52</td>
        <td>30.6185</td>
        <td>6.99697</td>
        <td>6.87309</td>
        <td>29.6689</td>
        <td>47.3351</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>42.4772</td>
        <td>33.7792</td>
        <td>9.89276</td>
        <td>8.60928</td>
        <td>33.2943</td>
        <td>50.9126</td>
    </tr>
	<caption>Table 1a: per-agent upvote-downvote averages, as a function of agent polarity (binned) and the amount of agent churn allowed in the simulation</caption>
</table>


<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1.00, -0.67)</td>
        <td>[-0.67, -0.33)</td>
        <td>[-0.33, 0.00)</td>
        <td>[0.00, 0.33)</td>
        <td>[0.33, 0.67)</td>
        <td>[0.67, 1.00)</td>
    </tr>
    <tr>
        <td>0</td>
        <td>0.248011</td>
        <td>0.207572</td>
        <td>0.0176008</td>
        <td>0.00745742</td>
        <td>0.192961</td>
        <td>0.326398</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>0.254704</td>
        <td>0.187829</td>
        <td>0.0429229</td>
        <td>0.0421629</td>
        <td>0.182004</td>
        <td>0.290377</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>0.237349</td>
        <td>0.188747</td>
        <td>0.0552775</td>
        <td>0.0481058</td>
        <td>0.186038</td>
        <td>0.284483</td>
    </tr>
	<caption>Table 1b: same as Table 1a above, but with each row normalized to sum to 1.0; this shows a slight proportional increase in neutrals as churn increased, slightly at the expense of the polars</caption>
</table>



### Clustering in the data

In general, the data exhibited distinct clustering patterns. Figure 5 shows a facet grid with 5 of the 6 control variables (all but churn_prop), along with a measure of polarity skew in the simulation's agents (0.0 is no skew). The homophily score pln_prp lies on the x axis, and the measure total activity, amounting to the sum of upvotes plus downvotes for the whole simulation, is used for the y axis. The use of total activity helps reveal the clustering more fully. The sub plot showing different SIG values clearly shows that SIG levels are responsible for the primary clustering behavior, while the subplot showing different al_attn levels shows a strong effect on clustering within the different SIG levels. Again, the _attn parameters are somewhat unfortunately named after attenuation, since higher values actually correspond to less attenuation. 

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1d.png" alt="scatterplots of the data against homophily and total activity, with colored factor levels for 5 of 6 control variables" width=800><p>Figure 5: Scatterplots involving 5 of the 6 control variables (churn_prop does not appear), against homophily, on the x-axis, as the pln_prp variable, and total activity (i.e. total votes), on the y-axis, as tot_activity; the plot involving SIG shows very distinct clustering, which is further degenerate at SIG=0.1</p></div>

If we assume upvotes in a social media system count as a kind of currency, or reward, and, in a system that allows downvotes, that downvotes are typically subtracted from an agent's upvote total (as they are in this simulation), then the total, system-wide upvote minus downvote total might be considered proxy for overall system happiness. Figure 6 shows a grid similar to that of Figure 5, with the only change the y axis now representing net_reward=upvotes-downvotes. The least "happy" simulations featured low curation (SIG=0.6), strict agent polarity matching ((nm_lo, nm_hi, vot_rad) triplet), high policing behavior (po_attn), and high altruism (al_attn). With these factors at their “worst” settings, the system-wide reward was actually negative, by a substantial amount.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1d_a.png" alt="scatterplots of the data against homophily and net reward, with colored factor levels for 5 of 6 control variables" width=800><p style="font-size: 0.9em; color: #666; font-style: italic;">Figure 6: Same as Figure 5, but with net reward (i.e. upvotes minus downvotes) in place of total activity on the y-axis; policing (po_attn) and agent polarity-selection strictness ((nm_lo, nm_hi, vot_rad) triplet) have noticeable effects on net reward; further, under some control parameter settings, the net system-wide reward was negative</p></div>
 

### Homophily in more detail

Figure 7 shows a histogram of homophily scores for all simulation runs within batch A (downvoting on). The distribution appears multimodal, likely in reflection of the clustering shown in Figures 5 and 6. A majority of simulation runs saw a homophily score near the low end, of around 0.72.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1e.png" alt="a histogram showing simulation run frequency as a function of pln_prp" width=400><p>Figure 7: Histogram of data batch A (downvoting on), showing frequency of simulation runs as a function of homophily measure pln_prp</p></div>

From the plots of Figures 5 and 6, we obtain some more detailed information on factors affecting homophily:
- SIG, or curation, has a very large effect, especially at SIG=0.1 where homophily gets very high
- al_attn, or agent altruism in a willingness to cross-post, also has a large effect, where the highest levels of altruism, at al_attn=1.0, typically see the lowest homophily

Regarding the effect of al_attn, it is important to keep in mind that the agents were effectively forced to cross-post, by virtue of the al_attn parameter setting, and that this cross-posting, effectively within each tree, directly affects the homophily score. So the value of the effect of al_attn is perhaps less because the effect is a surprise (it is not), and more because it allows us to study the magnitude of the effect relative to other control variables.

To gain an initial understanding of the effects of the control and other variables on homophily scores, simple linear regression was run on batch A of the data. Due to the large varations in outcomes induced by varying SIG (explained in the next section), SIG was fixed at 0.6 for the regression fit (SIG=0.35 gave similar results). Blocking was also conducted on churn_prop, which had fairly strong, possibly multivariate correlations with the other variables, leading to a high condition number in the regression fit when it was allowed to vary.

Table 2 shows the regression fit at SIG=0.6 (minimal curation) and churn_prop=0.005 (maximum churn), with the variable fields standardized. The overall R^2 is around 0.8. Since all predictor variables are standardized, the magnitudes of slopes of the regression fit should roughly indicate the importance of each variable's contribution.

<table class="simpletable">
<tr>
  <th>Dep. Variable:</th>         <td>pln_prp</td>     <th>  R-squared:         </th> <td>   0.793</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.784</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   93.39</td>
</tr>
<tr>
  <th>Date:</th>             <td>Tue, 11 Mar 2025</td> <th>  Prob (F-statistic):</th> <td>5.15e-40</td>
</tr>
<tr>
  <th>Time:</th>                 <td>14:05:59</td>     <th>  Log-Likelihood:    </th> <td> -80.867</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>   128</td>      <th>  AIC:               </th> <td>   173.7</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>   122</td>      <th>  BIC:               </th> <td>   190.8</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     5</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
       <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th>  <td>-6.779e-15</td> <td>    0.041</td> <td>-1.65e-13</td> <td> 1.000</td> <td>   -0.082</td> <td>    0.082</td>
</tr>
<tr>
  <th>al_attn</th>    <td>   -0.6573</td> <td>    0.042</td> <td>  -15.565</td> <td> 0.000</td> <td>   -0.741</td> <td>   -0.574</td>
</tr>
<tr>
  <th>po_attn</th>    <td>    0.0778</td> <td>    0.050</td> <td>    1.554</td> <td> 0.123</td> <td>   -0.021</td> <td>    0.177</td>
</tr>
<tr>
  <th>re_attn</th>    <td>    0.4363</td> <td>    0.041</td> <td>   10.587</td> <td> 0.000</td> <td>    0.355</td> <td>    0.518</td>
</tr>
<tr>
  <th>nm_lo</th>      <td>   -0.2288</td> <td>    0.043</td> <td>   -5.351</td> <td> 0.000</td> <td>   -0.313</td> <td>   -0.144</td>
</tr>
<tr>
  <th>ag_pl_skew</th> <td>    0.4773</td> <td>    0.052</td> <td>    9.144</td> <td> 0.000</td> <td>    0.374</td> <td>    0.581</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 1.117</td> <th>  Durbin-Watson:     </th> <td>   1.715</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.572</td> <th>  Jarque-Bera (JB):  </th> <td>   1.193</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 0.161</td> <th>  Prob(JB):          </th> <td>   0.551</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 2.654</td> <th>  Cond. No.          </th> <td>    2.04</td>
</tr>
<caption>Table 2: OLS Regression Results for data batch A, SIG=0.6 (minimal curation), churn_prop=0.005 (maximal churn); all covariates are significant, except for policing (po_attn)</caption>
</table>

The most influential input by slope was al_attn, which recall controlled agent cross-posting "altruism." Since altruism increased with al_attn value, the negative slope indicates agent altruism and homophily are inversely proportionate. Another prominent parameter is re_attn, which is the tendency for agents to reply to replies to their own posts. Also of some significance was the agent polarity spread triplet, (nm_lo, nm_hi, vot_rad), which also had a negative slope--unsurprisingly, the more strict the agents are in matching post polarities to their own, the higher the homophily. The variable po_attn, which controlled policing (downvoting) behavior of the agents showed comparitively little effect. Finally, but not least, was the measure of the overall polarity skew of the agents in the simulation, ag_pl_skew. As Figure 4 indicated, agent polarity skew has a noticeable effect on the overall polarity of posts. And as will be shown later, when downvoting and churn is allowed initial imbalances in agent polarity can grow much larger as the simulation runs, which leads to majority-minority dominance effects and can drive homophily higher.

To emphasize, the rough regression fit does not give the whole story. Seemingly minor variables in the fit, especially al_attn, actually turn out to be quite significant under the right conditions. And, as mentioned, the variables we blocked on, SIG and churn_prop, also can exhibit high influence.


### Specific variables

**SIG:**

The effects of SIG can be pronounced, but nonlinear in their magnitude. Recalling Figure 5, values SIG=0.35 and SIG=0.6 show little change in the homophily vs total activity plot, while SIG=0.1 shows a "sudden," very large change.

The general effect of SIG on homophily is, again, not directly surprising. If agents, either by their own choice or by curation algorithms, only see posts that closely match their own polarities, then most if not all interactions in any given tree should be fairly strictly homophilic--i.e. the polarities of posts in the tree should align quite closely. 

To explain the sudden change at SIG=0.1, it's possible that polarity selectivity at this SIG level was so narrow that it was below the agents' natural breadth of polar engagement. In other words though agents may have been amenable to a wider range of post polarities for upvoting, they face an artificially restricted range of posts due to the low SIG value. This could induce a sudden drop in cross-posting and mixing as the effects of SIG dominated natural agent tendencies. Also notable, SIG=0.1 amplifies the effect of another control variable. Figure 5 indicates the distinct subclusters at SIG=0.1 are solely due to different al_attn (agent cross-posting altruism) values. The "worst" region for homophily in the modeling runs is arguably the SIG=0.1, al_attn={0.1,0.25} outlier cluster: for this cluster, homophily is very high (ca 0.95), and total activity, as measured by total votes, is maximally suppressed. Otherwise, intra-cluster variations are not as pronounced between clusters for the other control variables.

**al_attn:**

Recall, al_attn directly controlled the agents' appetites for cross-posting. This variable affected agent actions after the skim list was created, with its reward intent, and "other" split determined by curation parameter SIG: al_attn determined the agents' propensities for engaging with posts in the "other" grouping, which were less well-aligned with the given agent's polarity score.

All else being equal, when agents are more willing to cross-post, the overall system improves in both measures of reduced homophily and increased activity (Figure 5). However note the boxplot of Figure 8: there is a slight but noticeable decline in overall system reward as agents become more altruistic. In a system that allowed agents to operate dynamically based on reward feedback (which this model does not), there would arise some push-pull effects if lower homophily was a simultaneous goal. Altruistic behavior would reduce homophily, but reduce agents' overall happiness under the measure of net upvotes minus downvotes. Also note the remarks under the SIG variable section: al_attn, at least in magnitude, may have its effect moderated by at least one other variable (SIG).

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1g.png" alt="box plot showing declining trend in net reward as function of agent altruism (cross-posting) levels" width=400><p>Figure 8: A declining trend is seen in net reward with increasing agent altruism; data is from batch A (downvoting on), with only SIG={0.35, 0.6} used</p></div>


**churn_prop:**

The control input for agent churn, churn_prop, had differing effects, depending on whether downvoting was on (batch A) or off (batch B). We will discuss the effects on batch A here, and in a separate section below, one that considers the overall differences turning downvoting off creates, the remaining effects of churn_prop will be considered.

To appreciate the effects of churn, Table 3a shows the average counts of retired agents in the polarity ranges, [-1,-1/3], [-1/3,1/3], [1/3,1], as a function of churn_prop value. Neutral agents (in [-1/3,1/3]) are seen to churn at the highest level, by up to around 80% more than polar agents at the highest level of churn_prop (=0.005). Table 3b shows average agent frequency by polarity at different churn levels, for only those agents remaining at the end of the simulation. Since the parameters of agents generated to replace agents who retired are drawn uniformly, when neutral agents churn at higher proportions it likely has a direct impact on the proportion of polar agents as the simulation proceeds.

<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1,1/3)</td>
        <td>[-1/3,1/3)</td>
        <td>[1/3,1]</td>
    </tr>
    <tr>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>3.94792</td>
        <td>5.96181</td>
        <td>3.26476</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>7.55556</td>
        <td>12.2726</td>
        <td>6.33767</td>
    </tr>
	<caption>Table 3a: Average retirement activity by agents by (binned) polarity (downvoting on); the highest churn level, churn_prop=0.005, shows about a 2:1 ratio of netural-third agents to agents of either polar third</caption>
</table>


<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1.00, -0.67)</td>
        <td>[-0.67, -0.33)</td>
        <td>[-0.33, 0.00)</td>
        <td>[0.00, 0.33)</td>
        <td>[0.33, 0.67)</td>
        <td>[0.67, 1.00)</td>
    </tr>
    <tr>
        <td>0</td>
        <td>9.73958</td>
        <td>10.1571</td>
        <td>10.1597</td>
        <td>9.90712</td>
        <td>10.0477</td>
        <td>9.98872</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>10.0252</td>
        <td>10.4306</td>
        <td>9.17882</td>
        <td>9.28559</td>
        <td>10.5174</td>
        <td>10.5625</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>10.1892</td>
        <td>10.8229</td>
        <td>8.08941</td>
        <td>8.31858</td>
        <td>10.9306</td>
        <td>11.6493</td>
    </tr>
	<caption>Table 3b: Average agent counts at the end of each simulation run, over differing levels of allowed churn and (binned) polarity (downvoting on)</caption>
</table>


Figure 9 shows the effect of differing levels of churn on total system reward (net upvotes minus downvotes). There is a slight increase in system reward as churn levels increase. This makes sense, since the churn process intentionally retired the least happy agents (those with the lowest upvote minus downvote scores), and since the parameters of replacement agents were uniformly drawn. Referring back to Tables 1a-b, which depicted a more detailed breakdown of the effect of churn on net reward, we see higher churn levels led to a slight decrease in reward for the polars, but increased reward for neutrals. To explain this, we'll first take a look at the effect of churn on homophily.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1i.png" alt="box plot showing net reward as a function of churn levels" width=400><p>Figure 9: A slight upward trend is seen in the batch A data for net reward as agent churn increases; the least happy agents leave</p></div>

In Figure 10, we see the relation between agent churn and homophily scores with SIG limited to {0.35,0.60}. In fact, churn_prop has a statistically significant effect on homophily (ANOVA, with intra-churn distributions reasonably normal, p=2e-6): the greater the churn, the higher the homophily. To understand this, and the effect of churn against agents of different polarities as in Tables 1a-b, a closer look at intra-run simulation dynamics will be helpful.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1h.png" alt="" width=400><p>Figure 10: Homophily as a function of churn levels, from data batch A with SIG in {0.35,0.60}; the effect is statistically significant (ANOVA, p=2e-6)</p></div>

Up to this point, the only measures in use have been static measures, made or accumulated over the life of each simulation run. However, it is useful at this point to consider the dynamics of agent polarity distributions, and how the distributions differ between the beginning and end of each simulation run when agent churn is allowed. To measure polarity skew in agent distributions for the polar thirds, define,\
\
$$
\mbox{skew} = \frac{\mbox{# agents in polarity range [-1,-1/3]}}{\mbox{# agents in polarity range [1/3,1]}}
$$

Let skew_start and skew_end be the measure of agent polar-thirds skew at the start and end of each simulation run, respectively. Then the differences between log(skew_start) and log(skew_end) will reveal changes in proportions of the polar thirds during the course of the simulation. Figure 11 shows a scatter plot of 50 log ratios. Each point has coordinate (log(skew_start), log(skew_end)). Each of the 50 points represent an independent run of the simulation with the usual 60 agents, over 100 rounds, and where the other control parameters are fixed as follows: at al_attn=0.5, po_attn=0.5, re_attn=0.5, SIG=0.35, (nm_lo, nm_hi, vot_rad) = (0.025,0.075,0.35), and churn at its highest setting, churn_prop=0.005.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_2a.png" alt="" width=400><p>Figure 11: Polarity skew of agents for given simulation runs are shown as x,y coordinate pairs, (log(skew_start),log(skew_end)) (downvoting on); there is a strong correlation, $r\approx0.85$, slope $\approx 2.1$</p></div>

The scatterplot appears to have a linear trend, with Pearson's r=0.85. A simple regression fit produces a slope of about 2.1. To interpret this, the polar skew for the agent population under this measure changes geometrically as a function of a fixed number of rounds. In this case, after every 100 rounds, we can expect a starting skew ratio x to become skew ratio x^2 at the end of the 100 rounds. This suggests under this model that when churn is permitted, the system may show negative dynamic stability with respect to the agents's polarity distribution. Slight asymmetries in agent distribution are likely to lead to a geometric runaway effect, where any slight majority of polar-thirds (those near -1 or those near +1) will eventually dominate the overall polarity distribution. Though the number of rounds for the simulations falls well short of such polar-majority dominance, and the amount of skew increase may not be constant, it is still likely sufficient to explain the reason for higher homophily scores when agent churn is allowed: the whole system is becoming unipolar, which of course would lead to greater homophily on a per-tree basis.

Recalling the effects of churn on agent rewards by polarity, from Tables 1a-b, the polar-majority dynamics explained just above suggest a cause for the slight reward increase for neutrals, and slight decrease for polars with higher churn levels. On average, polars suffer a slight decrease in net reward because majority-dominant downvotes reduce, sometimes aggressively, the happines of the polar minority. Meanwhile, neutral agents are less affected by the churn-induced inter-polar conflict, and so slightly benefit from higher churn levels.

Overall, as we saw earlier with al_attn, the effect of churn presents another challenge in reducing homophily: higher churn leaves agents happier, on average, as measured by net upvotes minus downvotes, but at the expense of higher homophily, to the point of runaway polar-majority dominance if the simulation is allowed to run long enough.


**(nm_lo, nm_hi, vot_rad)**

Figure 12 shows the effect of changing the overall agents' polarity-preference tightness on homophily. There is an expected slight increase in homophily when the agents are more strict about the acceptable polarity proximity to their own for upvoting, and how close the polarity of a comment matches their own polarity. Histograms show the distributions of homophily for SIG=0.35 and SIG=0.6 approximately normal, and a t-test for batch A restricted to those two SIG values showed the effect of agent polarity preference is statistically significant (p=4e-11). This significance was suggested earlier, in the linear regression fit at fixed SIG and churn_prop (Table 2).

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1k.png" alt="box plot showing a relation between homophily and agent post-selection polarity strictness" width=400><p>Figure 12: The effect of the agent polarity strictness triple (nm_lo, nm_hi, vot_rad) on homophily; higher strictness (lower values in the tuple) is correlated with higher homophily, to statistical significance (t-test, p=4e-11)</p></div>


**polarity skew**

To take another look at homophily as a function of agent polarity skew, consider the measure ag_pl_skew, computed by weighting the polarity frequency of non-retired agents at the end of the simulation, in 6 evenly spaced polarity bins in the usual range [-1,1]. Each bin frequency is multiplied by the polarity of the midpoint of the bin (so the first bin, for example, is over [-1,-2/3], with resulting polarity of -5/6 at the midpoint). The value ag_pl_skew is simply the sum of these frequency-weighted polarities over all 6 bins.

Figure 13 shows a simple scatterplot for the batch A data, prior to averaging within each control parameter set, with churn_prop $=$ 0.005, SIG $\ne$ 0.1, and al_attn fixed at 0.5. There is a slight, but noticeable increase in homophily as a function of increasing ag_pl_skew. This is consistent with the earlier result from the linear regression fit, in Table 2. Also note the upward trend vanishes at churn_prop=0.0, which is consistent with the tendency toward majority-dominated homophily when churn is allowed, demonstrated above in Figure 11.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_1l.png" alt="" width=400><p>Figure 13: Homophily measure pln_prp as a function of polarity skew of all agents active at the end of each simulation run, batch A data (downvoting on), with SIG=0.1 precluded, al_attn=0.5, and churn at maximum; there is a slight relation between increasing agent polarity skew and homophily</p></div>


**po_attn**

The policing parameter controlled an agent's propensity for seeking off-side polarity posts for purposes of downvoting. Overall this control parameter had seemingly few direct effects on homophily. The slope associated with po_attn in the casual regression fit of Table 2 for example was the lowest of all variables. However, the effects of po_attn on homophily aren't so simple. As we'll see in the section below with downvoting off, po_attn has quite an effect on mediating the speed of majority dominance when agents are allowed to churn. For now, however, we'll confine remarks to observing the po_attn parameter's effect on net system reward. Figure 6 shows distinct strata in the po_attn plot. Policing behavior has a marked effect on overall system reward, lowering the net upvotes minus downvotes totals in rough proportion to po_attn magnitude.


### Turning downvoting off

Data batch B featured the same modeling structure, and the same range of control parameter settings, but with agent downvoting turned off: agents were unable to cast downvotes for any reason. It is worth noting that turning downvoting off at the system level is more extreme than simply setting the policing control parameter, po_attn, to zero. Agents do not just vote for policing purposes (where they vote on off-sides posts), but also vote in proportion to the number of posts they make for reward intent purposes, as well as for posts in reply to posts that are replies to their own posts. However, we do expect similar system-wide behavior between turning off downvoting and reducing po_attn to 0.0, or a low value.

Overall, the average homophily over all simulation runs is about the same between batch A and batch B (pln_prp=0.776 vs 0.771). Facet plots similar to Figures 5 and 6 which include the major control parameters (not shown) are also similar. The effects of most of the control variables on homophily and net system reward, with the exception of po_attn, was also similar to those discussed in the "downvoting on" sections.

Table 4 shows a simple linear regression fit for batch B, with the outcome variable being homophily. The po_attn parameter was left out of the regression model since for batch B, po_attn was set to 1.0--it only controlled agent downvoting behavior. The other variable slopes in the regression fit roughly match those in the regression fit with downvoting off, in Table 2. One exception is the effect of the agent polarity skew measure, ag_pl_skew, which seemed to have less of an effect on homophily when downvoting was turned off, than when downvoting was turned on.

<table class="simpletable">
<tr>
  <th>Dep. Variable:</th>         <td>pln_prp</td>     <th>  R-squared:         </th> <td>   0.858</td>
</tr>
<tr>
  <th>Model:</th>                   <td>OLS</td>       <th>  Adj. R-squared:    </th> <td>   0.837</td>
</tr>
<tr>
  <th>Method:</th>             <td>Least Squares</td>  <th>  F-statistic:       </th> <td>   40.92</td>
</tr>
<tr>
  <th>Date:</th>             <td>Thu, 13 Mar 2025</td> <th>  Prob (F-statistic):</th> <td>4.36e-11</td>
</tr>
<tr>
  <th>Time:</th>                 <td>11:11:13</td>     <th>  Log-Likelihood:    </th> <td> -14.129</td>
</tr>
<tr>
  <th>No. Observations:</th>      <td>    32</td>      <th>  AIC:               </th> <td>   38.26</td>
</tr>
<tr>
  <th>Df Residuals:</th>          <td>    27</td>      <th>  BIC:               </th> <td>   45.59</td>
</tr>
<tr>
  <th>Df Model:</th>              <td>     4</td>      <th>                     </th>     <td> </td>   
</tr>
<tr>
  <th>Covariance Type:</th>      <td>nonrobust</td>    <th>                     </th>     <td> </td>   
</tr>
</table>
<table class="simpletable">
<tr>
       <td></td>         <th>coef</th>     <th>std err</th>      <th>t</th>      <th>P>|t|</th>  <th>[0.025</th>    <th>0.975]</th>  
</tr>
<tr>
  <th>Intercept</th>  <td> 2.442e-15</td> <td>    0.072</td> <td> 3.37e-14</td> <td> 1.000</td> <td>   -0.149</td> <td>    0.149</td>
</tr>
<tr>
  <th>al_attn</th>    <td>   -0.7284</td> <td>    0.075</td> <td>   -9.662</td> <td> 0.000</td> <td>   -0.883</td> <td>   -0.574</td>
</tr>
<tr>
  <th>re_attn</th>    <td>    0.3131</td> <td>    0.073</td> <td>    4.318</td> <td> 0.000</td> <td>    0.164</td> <td>    0.462</td>
</tr>
<tr>
  <th>nm_lo</th>      <td>   -0.3335</td> <td>    0.072</td> <td>   -4.602</td> <td> 0.000</td> <td>   -0.482</td> <td>   -0.185</td>
</tr>
<tr>
  <th>ag_pl_skew</th> <td>    0.1957</td> <td>    0.076</td> <td>    2.591</td> <td> 0.015</td> <td>    0.041</td> <td>    0.351</td>
</tr>
</table>
<table class="simpletable">
<tr>
  <th>Omnibus:</th>       <td> 0.757</td> <th>  Durbin-Watson:     </th> <td>   2.060</td>
</tr>
<tr>
  <th>Prob(Omnibus):</th> <td> 0.685</td> <th>  Jarque-Bera (JB):  </th> <td>   0.828</td>
</tr>
<tr>
  <th>Skew:</th>          <td> 0.271</td> <th>  Prob(JB):          </th> <td>   0.661</td>
</tr>
<tr>
  <th>Kurtosis:</th>      <td> 2.428</td> <th>  Cond. No.          </th> <td>    1.34</td>
</tr>
<caption>Table 4: OLS Regression Results for data batch B, SIG=0.6 (minimal curation), churn_prop=0.005 (maximal churn); all covariates are significant at a p=0.05 cutoff</caption>
</table>

For the effect of turning off downvoting on system reward as a function of polarity and churn level, see Tables 5a-b. In contrast to the same tables with downvoting turned on, Tables 1a-b, we see that net reward for given agent polarity ranges are virtually unchanged as a function of churn level. (With downvoting on, we saw neutrals fare a bit better at higher churn levels, and polar thirds fare a bit worse at higher churn levels.)

<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1.00, -0.67)</td>
        <td>[-0.67, -0.33)</td>
        <td>[-0.33, 0.00)</td>
        <td>[0.00, 0.33)</td>
        <td>[0.33, 0.67)</td>
        <td>[0.67, 1.00)</td>
    </tr>
    <tr>
        <td>0</td>
        <td>106.109</td>
        <td>86.5824</td>
        <td>45.7373</td>
        <td>46.1624</td>
        <td>84.3808</td>
        <td>108.95</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>101.188</td>
        <td>82.9294</td>
        <td>44.8543</td>
        <td>45.773</td>
        <td>79.3034</td>
        <td>103.029</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>96.5455</td>
        <td>79.2193</td>
        <td>37.9039</td>
        <td>40.3721</td>
        <td>77.5283</td>
        <td>98.6918</td>
    </tr>
	<caption>Table 5a: per-agent upvote-downvote averages, as a function of agent polarity (binned) and the amount of agent churn allowed in the simulation</caption>
</table>

<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1.00, -0.67)</td>
        <td>[-0.67, -0.33)</td>
        <td>[-0.33, 0.00)</td>
        <td>[0.00, 0.33)</td>
        <td>[0.33, 0.67)</td>
        <td>[0.67, 1.00)</td>
    </tr>
    <tr>
        <td>0</td>
        <td>0.222022</td>
        <td>0.181164</td>
        <td>0.0957002</td>
        <td>0.0965897</td>
        <td>0.176558</td>
        <td>0.227966</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>0.221381</td>
        <td>0.181434</td>
        <td>0.0981328</td>
        <td>0.100143</td>
        <td>0.173501</td>
        <td>0.225409</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>0.224388</td>
        <td>0.184119</td>
        <td>0.0880952</td>
        <td>0.0938316</td>
        <td>0.180189</td>
        <td>0.229377</td>
    </tr>
	<caption>Table 5b: same as Table 5a above, but with each row normalized to sum to 1.0</caption>
</table>


An analysis similar to that involving Figure 10 also reveals that with downvoting turned off, homophily no longer significantly varies with respect to churn level (ANOVA, though with poorer intra-churn normal fit, p=0.58).

To help further understand the effects of turning off downvoting, consider profiles of retired agents by polarity. Table 6a shows the average counts of retired agents by polarity ranges, as a function of churn levels. In contrast to the analgous table with downvoting on, Table 3a, the neutrals churn at a noticeably higher rate relative to polars when downvoting is turned off. Whereas the ratio of either polar third to the neutral third was about 1:2 at churn_prop=0.005 in the downvoting on case, in the downvoting off case, that proportion falls to roughly 1:5; over twice as many of the churned agents in this case are neutrals. Table 6b shows the expected impact on the final composition of the remaining non-retired agents at the end of the simulation runs. Overall, neutrals have been reduced by approximately 50% after 100 rounds with downvoting off at the churn_prop=0.005 setting, while they are only reduced by approximately 20% with downvoting turned on.

<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1,1/3)</td>
        <td>[-1/3,1/3)</td>
        <td>[1/3,1]</td>
    </tr>
    <tr>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>1.57639</td>
        <td>10.1111</td>
        <td>1.63194</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>4.13889</td>
        <td>18.4688</td>
        <td>3.68403</td>
    </tr>
	<caption>Table 6a: Average retirement activity by agents by (binned) polarity (downvoting off); the highest churn level, churn_prop=0.005, shows about a 4.5:1 ratio of netural-third agents to agents of either polar third</caption>
</table>


<table class="simpletable">
    <tr>
        <td>churn_prop</td>
        <td>[-1.00, -0.67)</td>
        <td>[-0.67, -0.33)</td>
        <td>[-0.33, 0.00)</td>
        <td>[0.00, 0.33)</td>
        <td>[0.33, 0.67)</td>
        <td>[0.67, 1.00)</td>
    </tr>
    <tr>
        <td>0</td>
        <td>9.79167</td>
        <td>10.184</td>
        <td>9.90625</td>
        <td>9.875</td>
        <td>10.2049</td>
        <td>10.0382</td>
    </tr>
    <tr>
        <td>0.0025</td>
        <td>11.6042</td>
        <td>11.4722</td>
        <td>7.125</td>
        <td>7.16319</td>
        <td>11.0486</td>
        <td>11.5868</td>
    </tr>
    <tr>
        <td>0.005</td>
        <td>13.0799</td>
        <td>11.8194</td>
        <td>5.03125</td>
        <td>5.29167</td>
        <td>11.7639</td>
        <td>13.0139</td>
    </tr>
	<caption>Table 6b: Average agent counts at the end of each simulation run, over differing levels of allowed churn and (binned) polarity (downvoting off)</caption>
</table>

For additional insight, refer back to the discussion of intra-run simulation dynamics in the churn_prop section above. With downvoting on, the change in agent polarity composition was considered at the start and at the end of each simulation run. For those runs, po_attn was fixed at 0.5. Reconsider those runs with downvoting on, but allow po_attn to vary, with additional values 0.25 and 1.0. The regression fit to the scatterplots analagous to Figure 11, showed respective slopes of 1.1 and 3.9. This indicates po_attn mediated the speed of the majority dominance effect in agent polarity distribution when churn was allowed.

Now consider a similar computation, but with downvoting off. The resulting scatterplot is shown in Figure 14. A regression fit yields a slope of 0.8. Recalling from the discussion in the churn_prop section above, the slope estimates the ratio between the starting and ending polar-thirds skew values, and a slope below 1.0 indicates a stabilizing effect to agent polarity skew. In other words, with downvoting turned off, the majority dominance effect to polar skew may be dampened, or even reversed. A similar effect might be suspected if policing parameter po_attn were reduced to near zero.

<div style="text-align: center"><img src="{{site.url}}/images/Reddit_figures/Fig_2n.png" alt="" width=400><p>Figure 14: Polarity skew of agents for given simulation runs are shown as x,y coordinate pairs, (log(skew_start),log(skew_end)) (downvoting off); the regression slope is $\approx 0.8$</p></div>

The question remains: if turning downvoting off had a noticeable effect on agent churn rates by polarity vs. rates with downvoting on, then why does the analysis between batch A and batch B otherwise look quite similar? And, relatedly, why did considering differing values of po_attn in batch A seem to have little effect, other than the obvious effects on total system reward?

One possibility for the limited visible effects of downvoting controls in these simulation runs is that the simulation was not allowed to run long enough. Since the primary effect of po_attn on homophily seemed indirect, mediated through churn and causing gradual minority-polar agent attrition, a longer run time (say, to 200 or even 500 rounds), would likely aggravate the majority-dominance effect when churn was allowed, especially for high po_attn values, which should drive the system to complete homophily as a direct result of almost all agents being in one polarity extreme. It is also possible that with downvoting on, while the system likely tended toward one-side polar dominance, the relatively high levels of neutral retainment, compared to cases with downvoting off or po_attn low, helped offset potential homophily from the emerging polar thirds imbalance.


## Discussion

We saw that the control inputs with the largest effects on homophily were (a) skim list curation (SIG), which controlled the polarity of posts agents previewed for reward intent, and (b) altruism (al_attn), which determined how willing agents were to engage with posts with polarities "far" from their own.

High levels of skim list curation, in fact, had markedly negative effects. Agents who are highly selective of what posts they choose to browse with the intent to post or vote, or social media algorithms that attempt to highly curate content for agent viewing, can have profound negative effects on the system, to include high homophily, low total system reward, and various lowered measures of total activity.

While curation is likely an exogenous factor, one set by social media algorithms, a willingness to cross-post, in spite of potential lower rewards, is up to the individual user. Though this suggests that encouraging more tolerance among the social media platform's users may reduce homophily, the review of the literature in the introduction suggests that those participating in social networks with higher levels of disagreement may tend to be apolitical. Under the assumption that either most polarized social media environments tend to be political, or that research interest in homophily and echo chambers should be focused on political, and not apolitical contexts, this presents a problem: if political interest causes reduced tolerance for cross-posting, then exhorting users to be more tolerant may not succeed in the cases of interest.

Other homophily inputs included agent polarity post-matching tightness ((nm_lo, nm_hi, vot_rad) triplet), the initial agent population polarity skew, agent reply-chain propensity (re_attn), and especially churn (churn_prop).

We saw that the effects of agent churn on homophily arose dynamically, as the simulation progressed, and not statically, as for many of the other control variables. Churn, when combined with downvoting and sufficient levels of agent-induced policing, could cause relatively rapid conversion to majority-polar dominance across the whole system. Though the simulations were only run for 100 rounds and we likely did not observe the full effects, the initial exponential growth of even slight imbalances in the polar distribution strongly suggests high levels of policing behavior under this model would eventually render the system almost completely uni-polar when churn is allowed. These imbalances in polarity of the active agents, in turn, would obviously drive homophily.

To mitigate the uni-polar runaway effect of churn, agents would need to exhibit less policing (downvoting) behavior, downvoting could be turned off at the social media platform level, or churn could be reduced. We'll consider each of these, in turn.

If mechanisms were available to reduce agent policing behavior, this would present one of several tradeoffs. Analysis of model output showed that lower policing can produce higher system reward, and can reduce homophily in cases where churn is allowed. A drawback, however, is that policing may help curb low-quality posts, and/or discourteous or even trolling behavior, which our model made no attempt to include.

Eliminating downvoting at a system level presents a related tradeoff: while this does help prevent runaway majority-dominance effects, it unfortunately tends to churn neutrals at a substantially higher rate. So the system with little or no downvoting loses engagement by, and perhaps in general loses the stabilizing effect of, neutral agents. We also lose policing behavior, by extension, when we turn off downvoting altogether, which again creates the additional drawbacks just mentioned.

Finally, to mitigate the majority-polar dominance threat of churn, churn itself could be reduced. This could be a definitive solution to minority-polar attrition, but it might be the hardest to implement. For one possibility, a platform might monitor for agent churn, especially when runaway majority-dominance effects start to arise, and provide extra incentives for minority-polar and/or neutral users to remain with the platform.

When consiering total system reward, or overall "happiness" as measured by total upvotes minus total downvotes over all trees, more tradeoffs arise. Generally, whatever makes the system happier, tends to make it more homophilic. This was certainly true for post curation, and for agent altruism, but also for policing, and the strictness of polarity matching when agents considered commenting or voting for reward intent purposes. This reflects a quote cited in the introduction that bears repeating: exposure to contrary opinions is "not necessarily the environment that produces enthusiastically participatory individuals."





## References

<p id="Coscia">[Coscia, 2022]: Coscia M, Rossi L (2022) How minimizing conflicts could lead to polarization on social media: an agent-based model investigation. PLoS ONE 17(1):e0263184.</p>

<p id="Haque">[Haque, 2023]: Haque, A., Ajmeri, N. & Singh, M.P. (2023) Understanding dynamics of polarization via multiagent social simulation. AI & Soc 38, 1373–1389.</p>

<p id="Kim">[Kim, 2015]: Kim Y (2015) Does disagreement mitigate polarization? How selective exposure and disagreement affect political polarization. J Mass Commun Q 92(4):915–937.</p>

<p id="Macy">[Macy, 2021]: M.W. Macy,M. Ma,D.R. Tabin,J. Gao,& B.K. Szymanski (2021) Polarization and tipping points, Proc. Natl. Acad. Sci. U.S.A. 118 (50) e2102144118.</p>

<p id="Morales">[Morales, 2021]: De Francisci Morales, G., Monti, C. & Starnini, M. (2021) No echo in the chambers of political interactions on Reddit. Sci Rep 11, 2818.</p>

<p id="Murdock">[Murdock, 2024]: Isabel Murdock, Kathleen M Carley, and Osman Yağan (2024) An Agent-Based Model of Reddit Interactions and Moderation. In Proceedings of the 2023 IEEE/ACM International Conference on Advances in Social Networks Analysis and Mining (ASONAM '23). Association for Computing Machinery, New York, NY, USA, 195–202.</p>

<p id="Mutz_a">[Mutz, 2002a]: Mutz DC (2002a) The consequences of cross-cutting networks for political participation. Am J Polit Sci 46(4):838–855.</p>

<p id="Mutz_b">[Mutz, 2002b]: Mutz DC (2002b) Cross-cutting social networks: testing democratic theory in practice. Am Polit Sci Rev 96(1):111–126.</p>

<p id="Simchon">[Simchon, 2022]: Almog Simchon, William J Brady, Jay J Van Bavel, Troll and divide: the language of online polarization (2022) PNAS Nexus, Volume 1, Issue 1, March 2022, pgac019.</p>

<p id="Stroud">[Stroud, 2010]: Stroud NJ (2010) Polarization and partisan selective exposure. J Commun 60(3):556–576.</p>

<p id="Treen">[Treen, 2022]: Treen, K., Williams, H., O’Neill, S., & Coan, T. G. (2022). Discussion of Climate Change on Reddit: Polarized Discourse or Deliberative Debate? Environmental Communication, 16(5), 680–698.</p>

<p id="Zhu">[Zhu, 2022]: Zhu, Jiefan & Yao, Yiping & Tang, Wenjie & Zhang, Haoming (2022) "An agent-based model of opinion dynamics with attitude-hiding behaviors," Physica A: Statistical Mechanics and its Applications, Elsevier, vol. 603(C).</p>



