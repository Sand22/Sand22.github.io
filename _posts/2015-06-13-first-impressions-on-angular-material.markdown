---
layout: post
title:  "First impressions on Angular Material"
date:   2015-06-13
lang: en
image: /assets/article_images/angular-material/angular-material.png
---

Recently at my current company we’ve been developing applications to remotely 
monitor people with heart disease and minimize the need of hospitalization. 
We’ve extensively been using material 
design as our primary set of UI design principles. In this post I’m going 
to express my admiration and possibly some doubts about Angular Material. 
Hence my main point of interest is the web, I decided to skip the mobile 
part and leave the eventual problems with Android to the smarter guys.

##tl;dr

Both Material design and Angular material are amazing. 
You should definitely try them out in your next project. 
You won’t regret it and your client will be dancing around with joy.

##What the heck is material?

If you haven’t heard of Material before, I seriously think
 you missed a huge milestone in UI design (shame on you).
  Google did a hell of a job putting several brilliant ideas
   together and polishing it enough to impress even the most 
   demanding folks out there.
   
I don’t remember who posted info about Google shipping Material, 
but it was definetely via Twitter (every tech news comes from Twitter).
 I remember my reaction after reading the first spec:
 
 ![My face after reading material design spec](http://i.memeful.com/media/post/WwlODdE_700w_0.jpg)

Everything was so cohere, I’ve fallen in love at first sight with 
these so-called “meaningful transitions” that much, guys at 
my company kept asking if I won the jackpot. Googlers behind 
interaction design definitely knew what they were doing. Last 
time I’ve seen something like that was the slick animation on 
iOS when you try to minimize currently running app.

The quite new approach was put on the stage: the paper design. 
If you happen to organize your layouts with cards, you’ll grasp 
it in no time. In a very few words I can say the clue is to layer 
your cards with the help of shadows and proper visual order. 
If this means nothing to you, imagine several piecies of paper 
set in relations and I don’t mean the mess on your desk. Some 
of those piecies would be placed vertically to each other and 
some of would lie on top of another. The same thing you can do 
on the web by making use of shadows, contrasting colors and bold 
typography to show the relations between elements in the interface. 
When you combine all the piecies together you should see your 
layout as more slender and with more bold nature.

Just look at some outstanding showcases here: [https://www.materialup.com/](https://www.materialup.com/).

Likewise, I highly recommend watching this awesome video from 
Chrome Dev Summit: [https://www.youtube.com/watch?v=tfSiXRy1vEw](https://www.youtube.com/watch?v=tfSiXRy1vEw) 
explaining key parts of Material Design. It should get you 
along and you’ll know what I’m talking about.

##My baby steps with Angular Material

First of all, there are several implementations of some Material 
design principles. The one I primarily have used is Angular Material 
[https://material.angularjs.org](https://material.angularjs.org). The 
other I can remember are Materialize, Polymer Paper Elements and Material 
UI. The latter consists of React components, so if you use the famous 
Facebook library in your stack, then it’s the fine way to go. However, 
if you’re more Angularish, then Angular Material offers some neat solutions as well.

I must say, I’ve been quite a lukewarm guy after seeing live demos of 
Angular Material components. My scepticism very likely stem from the fact 
I had run all snippets on Firefox at the early stage and noticed some minor 
flaws (i.e. the accordion kept expanding in the way the snippet code wasn’t 
readable at all). The interface was also a bit sluggish, but we’ll come to 
the performance issues later on.

Going deeper and seeing that bugs are constantly being smashed (or fixed), 
and the team behind the framework is overall very responsive, I feel much 
more secure when using this solution. At this stage of our project, the app 
behaves more or less the same in all major (should say evergreen) browsers 
and even on the IE the layout is roughly consistent. But there's more: I 
actually shouldn’t have gone deep into the code of Angular Material itself 
or create any workarounds to accomplish the logical concise. To me it’s a great 
benefit, shows that stuff have been tested and proves the high quality.

I really enjoyed working with form components, especially given the fact 
how flexible they are. For instance, I wanted to create more custom 
select element by putting some different style for each rows and 
including icons. No problem, pretty easy: just throw anything 
you want into md-option tag. Ok, how about rendering the select result 
in different format with respect to chosen options? No problem, at all: 
just make use of md-select-label tag and ngModel of course.

I know it’s just one short example, but the truth is it’s not an 
exception and the very easy-to-use feeling accompanied me much often.

However, if I had to choose one killer feature, it would be the grid 
list. I was actually quite humble when it all just clicked, because 
the solution was so neat. The possibilities in creating layouts, which 
usually happen to scale as great as our bellies when drinking too much 
beer, are endless.

Wanna have a pinterest-like dashboard coded in a quarter, meanwhile 
making a coffee for your team leader? Oh, well, there you go:

![Look how easy it is](http://i.memeful.com/media/post/oMJ28xM_700wa_0.gif)

I’ve used it with flex containers. Actually, the flexbox was incorporated for pretty 
much everything in the app - what a relief with not having to clear 
floats anymore! The final effect was so decent that one of my teammates had a 
nerdgasm. Even our designer looked at it and said something like 
“pixel perfect” or “you’re good”, whatever.

The best part was that my productivity clearly increased. Previously 
I had been implementing several projects in Bootstrap, Angular UI, 
using bought templates as a starting point. Now, we’ve made a full 
design from scratch and the switch from Bootstrap to Material was 
noticeable, especially if we think of animations and already mentioned 
grid system. All these state transitions may be tricky and time-consuming 
when designing and coding from scratch. The same we can say about grids, 
which honestly weren’t that flexible in the other CSS frameworks 
(flexbox and calc to the rescue!).

Of course, if you wish to accomplish something more complex, 
you’re quite on your own. Fortunately, there are so many tools  
delivered by the others (GSAP, velocity to name a few), that you’re actually not as much 
of a lone warrior on the battlefield as it may seem.

##A fly in the oinment a.k.a. the but section

It won’t be a long section, because all the minor shortcomings 
are constantly being ameliorate and it’s very likely that when you 
read my words, something is already not a problem anymore.

It’s fair to admit, that we couldn’t make a good use of theming 
system in Angular Material. Perhaps it’s a matter of preference, 
but we stick to the Sass way. I believe some of you (as we did) 
may consider it too strict, especially if you want to go slightly 
beyond material design. We would have ended up maintaining two 
sides of configuration (Sass and JS) and that’s unacceptable. 
Perhaps the fine lesson for us to learn is to stay more in the material 
field? Frankly, it’s a difficult topic to discuss, so I’ll leave it open.

The performance is unfortunately a real problem here. Have you used 
Inbox? I have and it’s sometimes like angry woman with a period or 
guy with a runny nose. A great visually product, yet not 
suitable for some machines, I’m afraid. That’s probably the cost of evolution. 
So be cautious and analyze your target audience diligently. You 
(or they) may not be ready for Material.

Lastly, more or less significant issues include lack of datepicker 
(even though you can find some featured on codepen), problems with 
positioning dialogs or calculating padding when combining flex with 
grid list, little buggy backdrop (for instance the one in md-select 
can make your layout crash, but as I’ve seen issue has been already 
reported), and so on.

But the thing is, most of them are not so tough to tackle by yourself. So be it.

##Conclusion

My new toy seems to be pretty mature based on my experience and some minor 
difficulties I have faced. The performance is not great on some devices, though. 
I admit, it may take a while to become better by making improvements or waiting 
for crappy machines to die or be replaced. But if you don't mind it so much and want to create
a truly modern interface with smooth transitions and all those eye-pleasing details, 
I cannot think of a better framework out there. In terms of how fast the working 
solutions can be delivered it has a very solid position. I hope it'll only get better.
