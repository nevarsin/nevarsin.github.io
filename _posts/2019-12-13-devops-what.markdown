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
| *And thaaat's the way the news goes!* |

In the last 4/5 years, and a lot more frequently after containerization kicked off, the only chance you never heard of DevOps is if you have been **not**  working in IT at all.

I first encountered the term in late 2017, duckduckwent* it and was personally blazed away by it.

> *to duckduckgo (past tense: duckduckwent): the act of a wise person to search for content via the <a target="_blank" href="https://www.duckduckgo.com"> DuckDuckGo search engine </a> 

# Origins
As you can find in the specific <a target="_blank" href="https://en.wikipedia.org/wiki/DevOps#History"> Wikipedia page</a> DevOps started in 2009 from a Belgian agile developer and it spread very quickly. The general idea was that one of historical bottlenecks of the software industry was the disconnection between Development depts and everything else. And that missing relationship had **a lot** of consequences.

# Bottlenecks


|![]({{site.baseurl}}/images/bottleneck.jpg)|
|:--:| 
| *This is a driving metaphore* |

## The traditional approach

Old schools development metodologies usually follow this steps:
1. A developer copies (or pulls) code to hook of a specific feature on his workstation
2. He adds his own code and builds this portion only
3. Other devs, in the same or a different team, do the same
4. Eventually, when the release window gets close, all code get merged
5. The project is then compiled as a whole
6. The build is then deployed in production


|![]({{site.baseurl}}/images/works_on_my_machine.png)|
|:--:| 
| *I'm a millenial and I communicate through memes* |

## Issues

While it looks streamlined, the steps reported above will encounter, among others, the following issues:

1. Each dev environment will take a lot to setup and it will be inherently incomplete
2. Builds and (manual) tests will be performed on a specific portion of code
3. No way to understand the impact before the final merge
4. All bugs from the whole project build will be extremely hard to track back
5. Dev, staging and production environments will be different and problems will rise also with Operations
6. Feedback happens seldomly thus making improvement slower and harder
7. Everything above will be even messier if no documentation is in place

Releases will be big, slow and riskier.

Above all technical issues: this approach is also doomed to create friction between developers, sysadmins and, ultimately, customers.

# Development+Operations

DevOps is a metodology that, both from a technical and methodological point of view, aims at a quicker, simpler and effective product development and release cycle (have a look again at the first pic of this article). 

Some of the benefits are:
- Smaller release batches
- Shorter release cycles
- Frequent feedback (from all stakeholders)
- Faster reaction to issues (during development or even after deploy)
- Business value is eventually perceived by all people involved
- Less company drama :)

## Tools
I will now describe some of the tools I employed in the will enable companies. I won't spend much time on those as there is plenty of documentation around if you like to get more in depth   

### Containers (mostly Docker)

This has been the biggest revolution in the field: containerization took away all the complexity of creating a proper run environment for an application by segmenting filesystem/network thus providing an evoluted chroot jail for a specific process or service.

Each container becomes an indipendent piece of the whole project and its content will be deployed in the same way on a single workstation dev environment, on a staging local server or in a production cloud infrastructure.

LOREM IPSUM