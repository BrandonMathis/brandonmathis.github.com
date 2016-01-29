---
layout: post
published: true
title: I Suck at Estimating
---


An ongoing struggle I've wrestled with all my career is regarding estimations
and their purpose in software development. A key component of SCRUM is that,
before a sprint starts, user stories are estimated on a scale (S M L XL).

My issue is, when I provide estimates to the business side of the company
I am employed to, that estimate then has solid business goals and a monetary
value attached to it. My estimations are seen as a guarantee of delivery
rather than a best guess of when a feature/story is completed. If I had my way,
I would not do estimates, ever. However, that is not the world we live in.

Now, I will not go into the solution to "Estimates are Hard" because I have not
found one yet. I will, however detail why I think it is nearly impossible for
a Production Team to give a Production Owner and Client a truly accurate estimate.

##"No" Estimates

The twitter sphere has been chattering about #NoEstimates for a while:

<a class="twitter-timeline" href="https://twitter.com/hashtag/NoEstimates" data-widget-id="618833205479796737">#NoEstimates Tweets</a>

Now, I do not buy 100% into the mentality of "no estimates ever." but once I started reading
into some of the excellent blog posts people have been writing, I learned a few things.

##Estimates, Forecasts, and Promises.

According to Merriam-Webster


> **An Estimate is**
> 
> an opinion on the nature, character, or quality of something.
> 
> **A Forecasting is**
> 
> to calculate or predict (some future event or condition) usually as a result of study and analysis of available pertinent data predict
> 
> **A Promise is**
> 
> a statement telling someone that you will definitely do something or that something will definitely happen in the future

I think that distinguishing these three categories when communicating the scope, timeline, and deliverability
of future work is important. We fall into a mine-filed when:

* A Product Owner (PO) asks for an estimate.
* The Production Team gives an opionion.
* The PO then delivers this as a promise.
* No forecasting is preformed

As a developer. I try my best to estimate but I do not forecast the work that I have
been asked to estimate. This problem is exacerbated when the PO then presents the estimates they
have received from their Production Team to the client as a promise.

##The Problem with Estimates.

The problem with no estimates is that people can't seem to live without them. When you get
in line for a roller coaster you want to know how long the wait is. When you order a pizza
you want to know when it will show up. When you re-model your house you want to know how long
workers will be tearing apart the home that you live in. It is a natural desire and I can
empathize with. Business goals need to be met and the people paying for the software you are
building need to know how long the work is going to take. Especially if they are paying
for hour time on an hourly basis.

Now, I do not take issue with estimates. I take issue when them when they are interpreted
as a guaranteed time of delivery. Let me give you a scenario.

> A Product Owner walks up to me with the most perfectly defined user story every created.
> Its acceptance criteria is flawless, it is a task I have done one million times before
> (like user authentication) and I even have a team of people who have been given 2 hours
> to evaluate and estimate this story.

Despite all these conditions being fulfilled, I still can only give you my best guess. Now, if
you want to run your business off guesses my team makes then I wish you the best of luck but
I know, and so should you, that the estimate I have presented to you is not a guaranteed time
of delivery. It is a guess. 90% of Estimating is guesswork.

Now, compound that problem with the reality that:

* 80% of software features clients ask has to be written from scratch.
* The work I am often asked to estimate, I have never done before.
* I cannot predict the future, critical bugs can happen.
* I have no guarantee that 100% of developers time will be spent writing code.

##Estimation vs Proposal

Often, a feature/story is described to me in medium to low details and it is them up to myself and the
team I am on to come to a conclusion of how long it will take to deliver that feature/story. That
estimation is then delivered to the client as a "This will take X days to complete" manner. However,
wen work start we quick come to the realization that the feature we are working on will actually take
X * 1.5 days/hours/points to complete.

When this is the primary data-point of a story, clients fixate on it. They grill you for not hitting
your estimates and how can we blame them? That is the only data-point we provided them. Maybe instead
we should be providing them with the estimate as a small part of a larger proposal.

###Proposals

I see proposals as a multi-faceted response to a request for work that includes.

1. How long will this take?
2. What is the value add to the user?
3. How will we implement this?
4. What part of the existing application will be impacted?
5. What potential risks to we foresee from this?

Then, on delivery, one could re-answer these questions

1. How long did this take?
2. What valid did we add?
3. How did we implement this?
4. What parts of the application were impacted.
5. What risks did we encouter and how did we address them?

Answered to questions 2-5 could shed more light on why a feature/story that was estimated at
1 day actually took 4.

##I Still Do Not Have the Answers

I still don't have the answer to my problem. I feel that when a client request an estimate, there
is a question behind that question that must be answered. Maybe it is "When will the app be production
ready?" or "Is this request I have made even possibly in a **reasonable** amount of time." Maybe estimates
really are just a security blanket that hold no REAL value. I am not sure. I want to live in a world
where I never have to estimate but I think that place is a fantasy. Client need to know when you
are going to be done but maybe telling them that is not as important as telling a client where
we currently are at. Regular status updates on the state of a project are possible. However, looking
into the future clouds things the further ahead you try to look.

My conclusion, estimates are hard. Really, really, really hard.
