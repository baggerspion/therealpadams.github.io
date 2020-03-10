---
title: Lehman's Second Law
date: 2016-12-30 10:00
categories: [Process]
header:
  src: complexity.jpg
  desc: Complexity by Mark Skipper (https://flic.kr/p/cFM3cd)
---
*This is the third post in my weekly series on [Lehman's Laws of
 Software
 Evolution](https://en.wikipedia.org/wiki/Lehman's_laws_of_software_evolution)
 and how they intersect with agility. The previous posts can be found,
 here:*

- Post 2: [Lehman's First Law](/2016/12/lehmans-law-1/)
- Post 1: [Lehman's Laws of Software Evolution](/2016/12/evolution/)

---

## Law 2: Increasing Complexity

> As an E-type system evolves, its complexity increases unless work is
> done to maintain or reduce it

Law 1 reminds us that we *must* adapt to change if our software is
going to remain relevant. Lehman's Second Law warns that, as we adapt
our software, complexity will increase unless the code receives
further maintenance.

Notice how there is no specific mention to how we should define
"complexity"? I don't think it really matters. We have all experienced
*that feeling*. That niggling feeling that the code is getting out of
hand or that adding *that feature* will cause the code to get out of
hand. You can just feel that refactoring is required.

In my previous post I talked briefly about avoiding the temptation to
become a feature factory. One of the clear indicators that your agile
project is heading in that direction is the dropping of systematic
refactoring.

## Managing Technical Debt In Agile Projects

Engineers are very good a feeling their way through a codebase. They
can just tell that the code is getting gnarly (or will do). This
feeling that something is wrong is almost always caused (at least in
part) by the codebase’s technical debt. I always think that technical
debt is subjective and hard to pin down. However, in short: technical
debt is the corners that have been cut, the undelivered promises the
issues that have been swept under the carpet.

Come on, admit it, **we all do it**.

The trick to handling technical debt inside an agile environment is to
admit to ourselves that it is always there and always need to be
addressed. We do not ignore it, we do not avoid it, we get on with it.

The trick to handling technical debt inside an agile environment is to
admit to ourselves that it is always there and always need to be
addressed. We do not ignore it, we do not avoid it, we get on with it.

### Managing Technical Debt in Scrum

Scrum (and really all agility) is about the constant maintenance of
technical debt. It is far too easy to think about the technical debt
solely in terms of the user stories (Feature Factory alert) while
ignoring all the opportunity we are afforded to handle it.

I’m a firm believer that every Scrum meeting is really just a planning
meeting (just with different scopes). Similarly, almost every Scrum
meeting is an opportunity to handle technical debt

#### Estimation

The first time a Scrum time usually sees a *brand new* user story is
during an estimation meeting (or "Backlog Refinement", or whatever you
would like to call it.) You'll know you are in a Feature Factory if
this meeting is solely dedicated to the content of that post-it note
that the Product Owner has presented. Good Scrum teams will also talk
about what is **not** on that card.

The technical debt for the user story is a crucial consideration:

- Is there any to be addressed before the user story can be enacted?
- Is the user story going to leave any additional technical debt
  behind?

The latter question really is worth considering. You do not want to
really plan for leaving technical debt behind, but the simple fact is
there is always the opportunity that you might.

Please, please, please remember my golden rule for this meeting:

```Everyone should look at the code during estimations```

This makes it a lot easier to see/feel the technical debt coming. It
also helps with the quality of the effort estimation, too.

#### Planning 1

If you took the time and effort to discuss technical debt during
Estimation, you might as well do the same during Planning 1. Do you
really want to select all of those stories if you know that they all
have a high likelihood of increasing technical debt?

#### Planning 2

By now, hopefully, technical debt for each user story has been
discussed during the Estimation meeting. Now it’s time to plan how to
handle it. In the same way that the team needs to plan who is going to
code which aspects of the user story, they should also take time to
plan any technical debt maintenance required:

- Is there some refactoring to be done?
- TODO lying around?

#### Review

Want to build up an awesome relationship with your Product Owner? Be
brutally honest with them during Review.

A Feature Factory environment will focus this meeting on showing that
the completed user stories meet their definition of done and any
additional criteria. But what happened to the technical debt that was
being tracked? The Review meeting is the perfect opportunity to
flag-up any technical debt that has been introduced.

If the purpose of your Review meetings is simply to have your user
stories accepted, you’re kinda getting it wrong. Review is really
about gaining feedback from the Product Owner on your work. But Review
is also a planing meeting: has your work during the sprint flagged the
need for some additional, future user story for the backlog? Be honest
with the Product Owner about any increase in technical debt; it may
warrant a specific user story to refactor it out later.

### Little and Often

So how do we handle Lehman’s Second Law in agile projects? Little and
often. In the “good ol’ days” each release would end with a fairly
substantial set of QA phases.

Contrary to popular agile opinion, I can come up with some decent
reasons why having some dedicated QA at the end of a release cycle is
a good thing. What’s important however, is that the general approach
agile teams should be taking is to constantly be mindful of their
technical debt and constantly put in the effort to reduce it.