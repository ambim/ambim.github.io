---
layout: post
title: Come Dine With Me - National Digital Twin edition

---

<div class="img-div" markdown="0">
<img src="/images/cdwm.png" height="100" />
</div>


In 2017, the National Infrastructure Commission set out a bold vision to create a National Digital Twin of the UK's infrastructure [1]. 'Put simply' this would be a real-time, digital representation of the country's physical assets which unlocks value principally by enabling better decisions about how the physical assets are built, operated, maintained or used [2].

The problem is, as Mark Enzer recognises in [2], this is not a simple problem. Far from it. There are many unsolved problems which stand in the way, and to get even close to becoming a reality, the infrastructure industry will need to engage with many groups of people it never really has before.

So who do we need to invite to the party? Here's my take on the not-so-obvious groups that I feel are vital to advancing us on this mission to create a national digital twin:
The big data architect: Imagine a national digital twin comprising a million (cautious estimate?) multi-modal sensors, sending information every minute. In a year, that would equate to over 500,000,000,000 data points to ingest, store & query for any sort of inference or machine learning model. Therefore, any data/ information framework developed needs to grounded in the reality of data structures that work for data at this scale. What we should be careful of, is letting infrastructure professionals define a digital twin ontology/ data structure without the consultation of a big data architect. Only to be that their ontology will never scale, or they will never be able to perform inference/ ML over such a structure.

Related: I went to a great talk last year by a Cesar Delgado (Siri Platform Architect, Apple). For slides, see [3]. The Siri platform deals with millions of requests every minute, around the world. Cesar went into to detail about the complex data structures that enable the Siri platform to work with data at this scale; it has been refined over many, many iterations. We need this kind of thinking.

Roboticist: How do you make sense of millions of data points (time-series, images, text data) across different spatial and temporal resolutions, in near-real-time? Tricky, but one method is called sensor fusion which roboticists have been studying for decades [4]. 

Mean-field games describe complex multi-agent dynamic system, like a school of fish
Mathematician/ AI researcher: Another approach to understanding and making decisions from large systems of interacting sub-systems is through the mathematical formulation known as [mean-field games](http://www.science4all.org/article/mean-field-games/). It has been used to describe complex multi-agent dynamic systems, such as a Mexican wave, or a school of fish. I won't go into detail in this article, but those looking to dig deeper on this topic should checkout an awesome [paper](https://arxiv.org/pdf/1803.05028.pdf) released last year by [prowler.io](https://www.prowler.io/), which combines mean-field games with multi-agent reinforcement learning (MARL). With accompanying, more accessible, [blog post](https://www.prowler.io/blog/decentralised-learning-with-many-many-agents).

The data visualisation creator: Those thinking about digital twins from a BIM background naturally assume that we should be visualising data against some sort of 3D model. This might be acceptable for very small scale twins, but going back to our one million data point NDT example, a human cannot visually interpret this much data, so what value is a 3D model adding? It would be more useful to take a 'system-of-systems' approach to data visualisation, and create new ways to describe visually how the different systems are interacting, at different aggregated levels.

The ethicist: Last but not least, we need an ethicist at the table from early on. The [Gemini Principles](https://www.cdbb.cam.ac.uk/Resources/ResoucePublications/TheGeminiPrinciples.pdf), published earlier this year, starts this conversation, but it doesn't end it. Given the potential societal impact of such a technology, we need teams of dedicated people thinking about and modelling scenarios. Quantifying risk. Asking philosophical questions. Demanding algorithmic explanations. Keeping true all the other dinner party guests at every stage.

In summary
I think you'll agree: quite the dinner party! Obviously, this post only begins to scratch the surface of the stakeholders required to plan and implement such an undertaking, but I've tried to focus on some not-so-obvious candidates.

What do you think? Have I missed off any key stakeholders? Let me know what you think in the comments and share the article with people you think should read it.

References

[1] [Data for the Public Good](https://www.nic.org.uk/publications/data-public-good/) report, 2017, National Infrastructure Commission

[2] '[The National Digital Twin sounds exciting – but we need to do the hard stuff](https://www.ice.org.uk/news-and-insight/the-civil-engineer/april-2019/national-digital-twin-sounds-exciting)', 2019, Mark Enzer (CTO, Mott MacDonald and Chair of the Digital Framework Task Group)

[3] "[Pitfalls of Apache Spark at Scale](https://databricks.com/session/apple-talk)", 2018, DB Tsai & Cesar Delgado (Apple)

[4] [Sensor Data Fusion in Robotic Systems](https://www.sciencedirect.com/science/article/pii/B978012012739950015X), 1991, Aggarwal & Wang