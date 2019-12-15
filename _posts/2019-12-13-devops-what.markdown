---
layout: post
title:  "DevOps is a fancy word"
description: "And yet you, your dev colleagues and your business need it now more than ever"
date:   2019-12-13 13:30:00 +01
categories: devops
author: nevarsin
published: true
---
|![]({{site.baseurl}}/images/Devops-toolchain.svg)|
|:--:| 
| "*And thaaat's the way the news goes!*" |

In the last 4/5 years, and a lot more frequently after containerization kicked off, the only chance you never heard of DevOps is if you have been **not**  working in IT at all.

I first encountered the term in late 2017, duckduckwent* it and was personally blazed away by it.

> *to duckduckgo (past tense: duckduckwent): the act of a wise person to search for content via the <a target="_blank" href="https://www.duckduckgo.com"> DuckDuckGo search engine </a> 

# Origins
As you can find in the specific <a target="_blank" href="https://en.wikipedia.org/wiki/DevOps#History"> Wikipedia page</a> DevOps started in 2009 from a Belgian agile developer and it spread very quickly. The general idea was that one of historical bottlenecks of the software industry is disconnection between Development depts and everything else. And that missing relationship had **a lot** of consequences.

# Bottlenecks


|![]({{site.baseurl}}/images/bottleneck.jpg)|
|:--:| 
| *This is a driving metaphor* |

## The traditional approach

Old schools development methodologies usually follow this steps:
1. A developer copies (or pulls) code to hook to a specific project feature, on his workstation
2. He adds his own code and builds this portion only
3. Other devs, in the same or a different team, do the same
4. Eventually, when the release window gets close, all code gets merged
5. Project is compiled as a whole
6. Project is tested manually by Quality Assurance (QA) 
7. Build is deployed in production


|![]({{site.baseurl}}/images/works_on_my_machine.png)|
|:--:| 
| *I'm a millenial and I communicate through memes* |

## Issues

While it looks streamlined the steps reported above will encounter, among others, the following issues:

1. Each dev environment will take a lot to setup and it will be inherently incomplete
2. Builds and (manual) tests will be performed on a specific portion of code
3. No way to forecast the impact before the final merge
4. Bugs from the whole project build will be extremely hard to track down
5. Dev, staging and production environments will be different and problems will rise also with Operations
6. Feedback happens seldomly thus improvement is slower and harder
7. Everything will be even messier if no documentation is in place

Releases will be big, slow and risky.

Above all technical issues: this approach is also doomed to create friction between developers, sysadmins and, ultimately, customers.

# Development+Operations

DevOps is a set of best practices that, both from a technical and methodological point of view, aims at a quicker, simpler and effective product development and release cycle (have a look again at the first pic of this article). 

Some of the benefits are:
- Smaller release batches
- Shorter release cycles
- Frequent feedback (from all stakeholders)
- Faster reaction to issues (during development or even after deploy)
- Business value eventually perceived by all people involved
- Less company drama :)

## Tools
I will now describe some categories of tools I employed in the past. I won't spend much time on documenting them as there is plenty of documentation around if you like to get more in depth   

|![]({{site.baseurl}}/images/tools.jpg)|
|:--:| 
| *This is again a metaphor, I like metaphors* |

### Containers (mostly Docker)

This has been the biggest revolution in the field: containerization took away all the complexity of creating a proper run environment for an application by segmenting filesystem and network thus providing an evoluted chroot jail for a specific process or service.

Each container becomes an independent piece of the whole project and its content can be deployed in the same exact way on a single workstation dev environment, on a staging local server or in a production cloud infrastructure.

Some refs: 
- <a target="_blank" href="https://docs.docker.com/get-started/">Docker "Get Started" documentation</a>
- <a target="_blank" href="https://labs.play-with-docker.com/">Play with Docker</a>

You can also have a look at two docker images I created:
- <a target="_blank" href="https://www.cmdbuild.org"> CMDBuild Asset Manager: </a><a target="_blank" href="https://hub.docker.com/repository/docker/trepz/cmdbuild" >Docker Image</a> - <a target="_blank" href="https://github.com/nevarsin/docker-cmdbuild">Github repo</a>
- <a target="_blank" href="https://www.resiprocate.org/ReTurn_Overview">reSIProcate STUN/TURN Server: <a target="_blank" href="https://hub.docker.com/repository/docker/trepz/stunturn">Docker Image</a> - <a target="_blank" href="https://github.com/nevarsin/docker-resiprocate"> Github repo</a>

### CI/CD applications

Continuous Integration and Continuous Deployment are often found together and featured at the same time in a few open source projects.
Some refers to them as "glorified scriptx". They're basically a whole lot of pre made automation that does the following:
1. gets triggered by commits of a source control management (Git, Mercurial)
2. compiles the source
3. executes automated tests
4. if both build and tests are successful, deploys the application in staging/production
5. reports on all of the above

Some of the prominent ones are <a target="_blank" href="https://travis-ci.org/" >Travis CI</a> (general oriented and with a native GitHub integration) and <a target="_blank" href="https://jenkins.io/" >Jenkins</a> (more Java oriented but extremely flexible nonetheless).
<a target="_blank" href="https://www.gitlab.com" >Gitlab </a>also includes CI/CD feature even on its Community Edition.  

### Orchestration

Once code (in form of containers) is in production, you'll need to rationalize how those containers will behave, be located, scale, etc. 
This is the duty of an orchestration application. It provides CLI and API to pick containers from a repository (called registry) and deploy them in a production environment, most of the times a cloud one. 

The complexity of orchestration can be overwhelming. A specialized role emerged subsequently in order to handle "only" this Ops part: <a target="_blank" href="https://landing.google.com/sre/books/" >the Site Reliability Engineer</a>.

There are multiple solutions to orchestrate with but I will be misdirecting you if I pointed you somewhere else than to <a target="_blank" href="https://kubernetes.io/"> Kubernetes</a>. You will find a lot of "distributions" of Kubernetes (aka K8S) provided by every major cloud player and also some learning/dev single-node versions like <a target="_blank" href="https://kubernetes.io/docs/tasks/tools/install-minikube/">MiniKube</a> or <a target="_blank" href="https://microk8s.io/">MicroK8s</a>.

## Humans
All of the goodness I listed above does not come free of charge. Well... yes but actually no. What I'm talking about is a change of paradigm from most of the people involved in the process. Here are some human requirements and benefits DevOps will bring to the table.

|![]({{site.baseurl}}/images/developers.jpg)|
|:--:| 
| *Yay vintage meme template!* |

### Automated tests
<a target="_blank" href="https://en.wikipedia.org/wiki/Test-driven_development">Test-driven development</a> is a methodology that turns the table when it comes to how to plan your code schedule. You don't start writing **and then** test it. Instead:
1. you start designing a test that will verify that your unit of code (e.g. a function) is working as exepected.
2. you write your code (no automation for that yet, sorry)
3. you run the test again and verify that conditions are met.

This way the test itself will be part of the build (run by the CI/CD tools) and it will basically replace the manual QA. Advantages are:
- Same person develops and tests
- Tests are run automatically at every build: errors are immediately reported
- A lot of manual, error-prone testing work can be avoided
- Code can be pushed directly to production

### Communication
DevOps also means collaboration among departments. When a dev can be empowered by the ability the push code in production, he becomes self aware of the value that the change will bring to the business. 

Given that work gets done and travels faster, people will be part of a big team working synchronously and empathically understand what are the issues that colleagues with totally different tasks may experience. 

Kanban boards (or their digital counterparts) are often use as a visual help to share tasks, statuses and teams in charge.

### More feedback

Releasing more frequently means also that you can get more feedback from users/customers and be able to adapt to those feedback quicker. 
In general this implies:
- higher customer satisfaction (as they see their opinion quickly valued) 
- precious data on specific changes that will drive the implementation road map from then on  

# Conclusion

TL;DR recapping, what you will have is:
1. Developers instantly getting a reliable dev environment
2. Test automation 
3. Production deploy triggered after a simple commit

I hope I was able to convey to you what groundbreaking change the adoption of the DevOps culture will be for you and for your company.
Even if your product is not a cloud SaaS application but an on-premise solution, you'll get competitive advantage by employing even some of this best practices and tools.

You will probably, as in everything in life, find resistance. It will come from status-quo loving people, colleagues that really don't want to change their day-by-day duties or simply that, until now, have managed to be low-output without being noticed.

Ultimately: from a geek point of view, it is a lot of fun :D

# References

I strongly recommend one book above all: <a target="_blank" href="https://www.amazon.com/Phoenix-Project-DevOps-Helping-Business/dp/0988262592/ref=tmm_hrd_swatch_0?_encoding=UTF8&qid=1576446507&sr=8-1">The Phoenix Project</a>: it's a novel (yes, seriously) that tells the story of a SysAdmin suddenly named VP of Operations after a major company failure. The book will get you through the challenges and the results from this journey.

I also recommend to follow <a target="_blank" href="https://www.youtube.com/channel/UC0NErq0RhP51iXx64ZmyVfg">Bret Fisher</a>, a "Docker Captain" that for many years helped to spread general competence on the topic. I found his Udemy courses extremely valuable. 

# Thanks for reading!
I hope you enjoyed this quick lecture and, most importantly, I hope I lit a spark on the topic. I will get more in depth of some of the tools listed in future articles.
Any feedback is appreciated. You can contact me <a href="/contact/">here</a> or via my <a target="_blank" href="https://www.linkedin.com/in/stefano-chittaro">LinkedIn</a> profile. 

Cheers,

Nevarsin