---
layout: default
title: Feedback
parent: Pull Requests
grand_parent: Development Guidelines
nav_order: 4
---

# Giving and Receiving Feedback
{: .no_toc}

When a reviewer gives feedback on a Pull Request it is important for them to empathize with the submitter and to focus on providing constructive, objective comments that focus on a positive outcome. As a submitter, it is equally important to empathize with the reviewer by focusing on the learning opportunity at hand and the positive outcome the reviewer desires. As both submitters and reviewers of Pull Requests we can ensure a positive review process by focusing on the following three directives: Be Kind, Be Constructive, and Be Concise. The following sections explore these directives in more detail through the lenses of both submitters and reviewers of Pull Requests.

1. TOC
{: toc}

## Directive One: Be Kind

Pull Request reviews are crucial moments for coaching and leadership within teams so it’s important to be kind. There are many frameworks on how to _be kind_, but the Emotional Intelligence model works well.

In their book [Emotional Intelligence 2.0](https://www.talentsmart.com/products/emotional-intelligence-2.0/), Travis Bradberry and Jean Greaves define Emotional Intelligence (EQ) as “…your ability to recognize and understand emotions in yourself and others, and your ability to use this awareness to manage your behavior and relationships.”

According to Bradberry and Greaves, EQ is made up of four core skills under two primary competencies.

Personal Competence: “ability to stay aware of your emotions and manage your behavior and tendencies.”
Self-awareness: “ability to accurately perceive your own emotions in the moment and understand your tendencies across situations.”
Self-management: “ability to use your awareness of your emotions to stay flexible and direct your behavior positively.”

Social Competence: “ability to understand other people’s moods, behavior and motives in order to improve the quality of your relationships.”
Social Awareness: “ability to accurately pick up on emotions in other people and understand what is really going on with them.”
Relationship Management: “ability to use your awareness of your own emotions and those of others to manage interactions successfully.”

|[TalentSmart](https://www.talentsmart.com/media/uploads/pdfs/Emotional%20Intelligence%202.0%20Step%20By%20Step.pdf)|What I See|What I Do|
|---|---|---|
|Personal Competence|Self-Awareness|Self-Management|
|Social Competence|Social Awareness|Relationship Management|

Submitters and reviewers of Pull Requests can use the Emotional Intelligence framework to actively change their perspectives to view feedback through the lens of the other party. This ,in addition to keeping in remembering that both parties are working toward a common goal and have each others best interests in mind, can ensure that the Pull Request process is a productive exercise.

### Giving Feedback

##### **DO** adjust your expectations of a submission based on the perceived skill level of the submitter
{: .text-green-100 }
As a Pull Request reviewer make sure to consider the perspective and skill level of the submitter.  What would you expect differently from a Principal, Senior, or Junior level submitter?

##### **DO** assume that the submitter is aiming for the success of the product
{: .text-green-100 }
A reviewer cannot assume that the submitter does not want to achieve a beneficial outcome for the product.  It is highly improbable that the person on your team who spent a day working on a feature is trying to make the project fail. (It’s not impossible, you may actually have a toxic team member that needs to be removed, but that situation is atypical and outside the scope of pull request reviews).

##### **CONSIDER** finding a mediator when the response to feedback isn’t “kind”
{: .text-yellow-300 }
Be kind when you write your feedback.  If the other side of your exchange is not being kind, then you may have to go into a [Crucial Conversation](https://www.amazon.com/dp/0071771328?tag=duckduckgo-brave-20&linkCode=osi&th=1&psc=1) or find someone else to mediate the conversation.

### Receiving Feedback

##### **DO** treat receiving feedback as a coaching moment and prepare to learn something
{: .text-green-100 }
When you are receiving feedback, the same principles apply.  Know your own skill level and be prepared to consider areas you need to improve.  Even a Senior developer can learn a few things, and a Junior developer can certainly teach you something new.  Treat feedback as a coaching moment and extract lessons learned from the exchange.

##### **DO** assume that the reviewer is aiming for the success of the product
{: .text-green-100 }
Your pull request reviewer most likely wants to achieve a positive outcome for the product and is trying to voice concerns and corrections.  They might be the best technical person, but are terrible at crafting empathetic or constructive feedback. Be patient and ask questions, get to the root of the issue so you can resolve it more confidently.

##### **CONSIDER** finding a mediator when the feedback isn’t “kind”
{: .text-yellow-300 }
Be kind when you receive feedback.  If the other side of your exchange if not being kind, then you may have to go into a [Crucial Conversation](https://www.amazon.com/dp/0071771328?tag=duckduckgo-brave-20&linkCode=osi&th=1&psc=1) or find someone else to mediate the conversation.

## Directive Two: Be Constructive

The Pull Request feedback conversation should focus on constructive and objective points. The reviewer must keep in mind that the presented solution could be a perfectly good solution even if it isn’t the solution the reviewer had in mind initially. The submitter must keep in mind that feedback is meant to be constructive and isn’t personal. To help organize constructive feedback into actionable, prioritized items, you can use the [MoSCoW](https://en.wikipedia.org/wiki/MoSCoW_method) method.

### Giving Feedback

It is inevitable that at some point a reviewer will receive a “perfect storm” Pull Request which contains an abundance of issues. Using the MoSCoW method, the reviewer can categorize feedback objectively according to that which is highest priority — critical for acceptance — and that which is more focused on future work and the development of the submitting engineer.

##### **CONSIDER** use the MUST, SHOULD, COULD, and WON’T keywords in your pull request feedback and explain clearly
{: .text-yellow-300 }
Each feedback comment should be tagged with “MUST”, “SHOULD”, “COULD”, and “WON’T”.  

|**MUST**| for this pull request to be accepted, these changes MUST be made|
|**SHOULD**| these changes SHOULD be made, however, they are not required for this specific pull request to be accepted.  But, they SHOULD be made in a future pull request|
|**COULD**| describe an alternative approach as a coaching moment.  Would you have done it differently based on different concerns?  Acknowledge the proposed approach works, but there are other possible options|
|**WON’T**| for this pull request to be accepted, these changes must be REMOVED|

In each case, you should objectively explain your rationale for the feedback. Justify why an item is tagged as MUST, SHOULD, COULD, or WON’T by discussing the risk (impact x probability) or referencing best practices or standards.

### Receiving Feedback

##### **DO** write your response to feedback in the same context as the original comment
{: .text-green-100 }
When reading through feedback, even if it hasn’t been tagged using the MoSCoW Method, you MUST look for the keywords to put the feedback into context and provide an objective response.  Add your comments next to each statement in the pull request response.

|**MUST**| highlight how you have resolved the “MUST” concerns|
|**SHOULD**| if there’s a good reason why it was done a certain way, write it down.  Otherwise, leave it alone|
|**COULD**| make a personal note of the alternative approach, but DO NOT implement it in this pull request|
|**WON’T**| remove items from your pull request to reduce the risk of the change.  For a reviewer to give you this feedback, they will feel quite strongly it is the wrong approach or inappropriate for this pull request|

##### **AVOID** immediately resolving feedback marked as “COULD” with changes
{: .text-red-300 }
##### **DO** ask questions when the feedback isn’t clear or objective
{: .text-green-100 }
A Pull Request submitter should feel comfortable questioning the reviewers rationale to better understand the intent of their feedback.

Give and receive feedback constructively.   The pull request is a coaching moment and should be treated as an objective conversation which will produce a better outcome for the product and the team.

## Directive Three: Be Concise

Pull request feedback can be extremely detailed, or can be simple and broad, or can be something in between.  The key for reviewers is to better understand the submitter, and consider using the Situational Leadership framework to decide how much detail is needed in pull request feedback.  Likewise, the submitters can consider their own situation and discuss how the reviewer can construct feedback that is more likely to get a better response.

[Situational Leadership](https://en.wikipedia.org/wiki/Situational_leadership_theory) is all about adjusting your leadership style based on the ability (skill level) and the commitment or willingness level of the submitter.

What is Situational Leadership?

The Situational Leadership model is based on the work done by Blanchard and Hersey. Their theory is based on two concepts: leadership itself, and the developmental level of the follower. Blanchard and Hersey developed a matrix consisting of four styles:

|**Telling**|S1 (specific guidance and close supervision): These leaders make decisions and communicate them to others. They create the roles and objectives and expect others to accept them. Communication is usually one way. This style is most effective in a disaster or when repetitive results are required.|
|**Selling**|S2 (explaining and persuading): These leaders may create the roles and objectives for others, but they are also open to suggestions and opinions. They “sell” their ideas to others in order to gain cooperation.|
|**Participating**|S3 (sharing and facilitating): These leaders leave decisions to their followers. Although they may participate in the decision-making process, the ultimate choice is left to employees.|
|**Delegating**|S4 (letting others do it): These leaders are responsible for their teams, but provide minimum guidance to workers or help to solve problems. They may be asked from time to time to help with decision-making.|

**Stages of employee development in situational leadership**

Along with leadership qualities, Blanchard and Hersey defined four types of development for followers or employees:

|Low Competence|High Commitment|
|Some Competence|Low Commitment|
|High Competence|Variable Commitment|
|High Competence|High Commitment|

This model requires the reviewer to consider the competency and commitment or willingness level of the employee, and adjust the feedback to the appropriate style.

**Low Competence / High Commitment = Telling**

Be specific about all the changes you requested, and tag each change as you see it in the code.  This type of submitter will most likely only fix the things you point out, so you better point them all out.  This will take longer on the part of the reviewer, but it will send a clear message to the submitter that all the work is being looked at deeply and all the changes are important.  Don’t worry about explaining all the “why” parts, just explain what needs to be done.

**Some Competence / Low Commitment = Selling**

Be specific about all the changes you requested, but include more explanation on “why” a change is requested. The submitter wants to know more about why changes are done because they now have enough skill to ask questions.  

**High Competence / Variable Commitment = Participating**

Be specific about the major changes you requested, but you can be more general about any lower level issues you see.  For example, if you see whitespace issues, just mark one instance and mention there are more you saw. Discuss why changes are being requested, but only for the major changes.  Be sure to highlight areas that are done particularly well, even if they aren’t the way you would’ve done them.

**High Competence / High Commitment = Delegating**

Be general about the major changes you requested, and be more general about any lower level issues.  You can summarize these pull request reviews in the general comment block rather than needing to highlight certain lines.  Be sure to highlight design ideas that are done particularly well, even if they aren’t the way you would’ve done them.

### Giving Feedback

##### **CONSIDER** the appropriate situational response after evaluating the submitter’s competency and commitment level
{: .text-yellow-300 }
Matching the feedback style to the review will usually get a better response from the submitter.  It signals to the submitter that the reviewer is paying attention and stepping back where it makes sense.  As the submitter moves along in their path to autonomy, it is the reviewer’s role to reduce the amount of hand-holding and trust the submitter to do the right thing.

Ascertaining competency and commitment is challenging and takes practice.  As a reviewer, keep in mind that competency is measured for the task at hand specifically, not generally.  If this is the first time the submitter is working in a messaging system, but has otherwise been building web applications for a long time, then you’ll need to adjust your approach.  Observing the level of commitment and the attitude of team members is important to give you non-verbal feedback on how your level of leadership was achieved. Not enough feedback for a beginner makes them feel you don’t care enough to help them out. Too much feedback for an expert conjures up the dreaded “micro-manager” label.

### Receiving Feedback

##### **CONSIDER** being mindful of the reviewer’s available time to provide tailored feedback
{: .text-yellow-300 }
##### **DO** be honest and open in your communication and expectations to the reviewer
{: .text-green-100 }
As a submitter, keep in mind that the reviewer is busy and is trying to optimize the time spent on providing feedback.  If you need more detailed feedback or really don’t want to see the line-by-line approach, then tell them. The reviewer may still be learning how to read the situation and could use some help from you to get it right.  If there’s any doubt, say what you want and clear it up.
