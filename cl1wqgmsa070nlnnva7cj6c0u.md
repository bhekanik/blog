## Explaining code quality to management: The technical debt metaphor

---
title: Explaining code quality to management: The technical debt metaphor
published: true
description: Managing internal code quality is an increasingly critical aspect of producing cost-effective, timely, and high-quality software. But it can often be difficult to explain what this internal code quality is to non-technical people, unfortunately, management usually consists of non-technical people as far as coding is concerned. The technical debt metaphor is one way that can be used to clarify this. In this article, I will define external and internal quality and then talk about technical debt, it's definitions and how it is used as a metaphor for discussing quality with non-technical business people. 
tags: technical debt, project management, code quality, management
cover_image: https://thepracticaldev.s3.amazonaws.com/i/u5am6bureo9y0yejd51e.png
---

Managing internal code quality is an increasingly critical aspect of producing cost-effective, timely, and high-quality software. But it can often be difficult to explain what this internal code quality is to non-technical people, unfortunately, management usually consists of non-technical people as far as coding is concerned. The technical debt metaphor is one way that can be used to clarify this. In this article, I will define external and internal quality and then talk about technical debt, it's definitions and how it is used as a metaphor for discussing quality with non-technical business people. 

That said, if you just came here for a definition, here it is: 

> A term used to describe the obligation that a software organization incurs when it chooses a design or construction approach that is expedient in the short term but that increases complexity and is more costly in the long term. It is a metaphor that facilitates the communication between business and technical people regarding implementation artefact inadequacies and the risks they carry.

### TL;DR

Software systems are prone to the build-up of deficiencies in internal quality that make it harder than it would ideally be to modify and extend the software. In their haste to deliver features (usually do due to time pressure from management), developers sometimes engage in less-than-optimal coding practices. If not addressed, these shortcuts can ultimately yield rework costs that offset the benefits of rapid delivery. 

Technical Debt is a metaphor, coined by Ward Cunningham, that frames how to think about dealing with this by viewing it in the same way as financial debt. The extra effort that it takes to add new features to code that was not done right is the interest paid on the debt. The longer you take to fix the code, the more time it takes to extend or maintain it and the higher the interest you pay on the technical debt. Taking shortcuts to expedite the delivery of features in the short term incurs technical debt, analogous to financial debt, that must be paid off later to optimize long-term success.

### Let's break it down

What should be understood first is that technical debt is connected to the quality of software, so before going too far, let's first talk a little bit about quality. 

There are two types of quality; external quality and internal quality. External quality is the quality that can be perceived by a user or a customer - that might be things like performance, security, scalability, stability, responsiveness, etc. Anything that can be measured or experienced by users. Because it is a feature of the product, it is usually managed by the product owner or business people, they can easily draw the link between this quality and how the users will respond to the software. 

The more complex part of software development is that there is also internal quality. Internal quality can only be perceived by developers or technical people who can understand the code. It's anything that makes maintaining or extending the code easier or harder. Examples might be existence or lack of tests - if there are more tests then the code is easier to change, it might be about the architectural styles used, it can also be about coding issues - whether the code is too complex. Software developers talk a lot about quality code and good code but the value of this is not always immediately obvious to non-technical people if's not always clear to them why we should invest in code quality when the software already works. That's where the technical debt metaphor comes in. 

Technical debt is used to describe code that is not quite right, to put it loosely. We use this metaphor of debt to draw comparisons with financial debt which more people are able to comprehend easily. This helps in explaining that if we want to build something on top of code that is not quite right, over time it will get expensive to extend that kind of code and it will take longer to build new features if the code base is of bad quality. 

The term was coined by Ward Cunningham, one of the authors of the Agile Manifesto. He said that some problems with code are like financial debt. It’s ok to borrow against the future, as long as you pay it off. In 1992, at the OOPSLA conference, he famously said the following (slightly paraphrased):

> Shipping first-time code is like going into debt. A little debt speeds development so long as it is paid back promptly with refactoring. The danger occurs when the debt is not repaid. Every minute spent on code that is not quite right for the programming task of the moment counts as interest on that debt. Entire engineering organizations can be brought to a stand-still under the debt load of an un-factored implementation, object-oriented or otherwise.

This basically says that technical debt is about shipping something quickly and thereby going into debt where you will need to repay that debt by increasing the quality later on. If you don't do that, then you will need to pay increasingly higher interest rates as your productivity goes down and you're slowed down in your development. The interest is the extra time you will spend refactoring the code later as opposed to doing it now. Ward explains this much clearer here:

> With borrowed money you can do something sooner than you might otherwise, but until you pay back that money you will pay interest. I thought borrowing money was a good idea. I thought that rushing software out the door to get some experience with it was a good idea. But that, of course, you would eventually go back and as you learned things about that software you would repay that loan by refactoring the program to reflect your experience as you acquired it.

This is a good metaphor for talking to management because they should be familiar with financial terms and it is easier to talk to them in these terms and tell them that this is just like getting a loan at the bank, you get some benefit for some time but at some point you will need to repay it and you repay it with interest, the longer you take to repay the higher the interest. 

Ward, however, makes it clear that technical debt is not to say you should write code poorly with the expectation that you will make it good later, writing poor code is never a good thing. The idea is that you can write code that reflects your current understanding of the problem at the time even if that understanding is partial. So that when the time comes to refactor it's clear what you were thinking and you can then align it with your new understanding and that time spent going back to realign the code is the interest on the debt.

A different definition that, perhaps, many are familiar with is by Steve McConnell. He says technical debt is: 

> A design or construction approach that is expedient in the short term but that creates a technical context in which the same work will cost more to do later than it would cost to do now (including increased cost over time).

Put simply, it means doing something quick and dirty and we gain something in the short term but with the knowledge that it will bite us in the long term. A delicate balance is needed between the desire to release new software features rapidly to satisfy users and the desire to practice sound software engineering that reduces rework and improves the efficiency of maintenance and extension. This is especially true because internal quality sooner or later becomes external quality and that's something that's important to communicate. For instance, if we have a bad code base, we get more and more bugs, we slow down and that will eventually bubble up to all kinds of stakeholders of a project. 

Ultimately technical debt is not a developer problem, it's a company problem. If you accrue too much technical debt, in the worst cases, whole engineering departments can come to a standstill. It can be argued that managing technical debt is one of the key points in successfully developing commercial software. Hopefully, this short piece has clarified your understanding of technical debt and will help you out the next time you try to explain why you need to fix code that works instead of working on new features.  

In the next article, I will talk about whether technical debt is entirely bad and how to handle it. Keep an eye out for that one.