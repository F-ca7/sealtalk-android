# SealTalk

**By ChuYu Xu, YanZhi Fang, YunQing Liu, YuXin Duan, ZuXin Li.**

*Wuhan University*

## Table of contents

- [Introduction](#Introduction)
- [Stakeholder Analysis](#Stakeholder)
- [Context View](#Context-view)

## Introduction
Instant messaging is the most popular way for people to communicate on Internet,
and various kinds of instant messaging software are emerging one after another.
Service providers also provide more and more abundant communication services.
SealTalk is an open source instant messaging App based on the Rongyun Instant
Messaging SDK (IMKIt).

It aims to provide developers with IM development needs with reference
development practices, thereby reducing the time from development to online
application, and devoting more energy to the application's own business
implementation. IMKit includes two UIs: session list and session page. SealTalk
adds login UI, contact UI, discovery UI and "I" UI to provide a complete set of
instant messaging App DEMO instances.

SealTalk contains session scenarios including single chat, group chat, chat
room, discussion group and so on. Conversations between users can be
accomplished through the delivery of multiple message types. It includes
built-in message types such as text, voice, picture, location, and custom
message types such as red packets, business cards, etc. Some information about
the message, such as whether the message is sent successfully, whether the
message is read or not, will be immediately fed back to the user.

In the following section, we will take the reader through the workings and
development of SealTalk, showing our results of analysis on it. The analysis
will start by explaining about stakeholders and context view. Then in order to
have a deeper insight on SealTalk's technical side, the analysis is continued by
describing on functional view and development view. To understand SealTalk even
further, evolution perspective, which shows the process of how SealTalk develops
into what it is today, is also presented. At the end, we will have a conclusion.

## <span name="Stakeholder">Stakeholder Analysis</span>
To start off our analysis, we will look at the stakeholders involved with
SealTalk. A stakeholder is an entity of a system architecture that consists of an individual
or an organization that has importance and interest to realize a system.

### Stakeholders

**Acquirers**

RongCloud provides real-time communication cloud services. They release
SealTalk-android as a demo application for their cloud services. Acquirers wants to sold their
cloud services, so they want to make it simple, and capable to demonstrate the ability of cloud server. 
They provide cloud service as a part of SealTalk support.

**User**

As a real-time communication software, SealTalk has a very broad user base. The
user could use it as a tool for personal communication, or to send message in a
company. Notice that SealTalk\`s basic function are built for general usage,
enterprise user may need to modified their part. For business user,they want SealTalk
to be easily extended for their purpose, while user prefers it to be as easy as possible in using.
User provides feedbacks to further improve the system.


**Suppliers**

SealTalk-android is built upon Java, and deployed on Android. They use Android
Studio as development environment.

**Development & Testing**

In GitHub record, most of the early commit are
given by jenkinsrc and Jianli Zhou. They are the employees of RongCloud Inc. 
RongCloud has assigned a group of nearly 5 people to develop SealTalk-android.
Developers implements nearly all the main function of the system.

**Competitors**

QQ and WeChat are strong competitors to SealTalk who provides similar services
and have a much wider user.

**Maintainers**

Commits that submit later are mainly given by rc-huangxiujun, an employee of
RongCloud. SealTalk is quite a young project ,there is not so much fork record on GitHub, most of the maintainer are RongCloud's worker. They are more interesting in adding extended function as well as fixing known bugs. Also, they need the system easily to understand, for 
individual developers may customize SealTalk-android for their purpose, which enlarge the protential usage
of SealTalk.

### Power-interest

In the stakeholder analysis, many different actors with distinct roles, power
and interest have been analyzed. The predominant stakeholder of SealTalk is
RongCloud Inc., because it's the company which it belongs to,which has the most power and interest in this project. Besides, developers of this project, two Github contributers--jenkinsrc(22commits) and rc-huangxiujun(8commits), have the highest power and interest ethier.

RongCloud has already completed two rounds of financing:

Round A: RongCloud announced the completion of round A financing of 50 million
yuan, which was led by ZTE.

Round B: RongCloud announced the completion of the B round financing of 100
million yuan, which was invested by JinPu investment, TianXing capital and
Shanghai JiaCun investment.

Because these firms also invest in other various products, we can consider that
the investment means these companies have high power but moderate interest in
RongCloud’s product, as well as seal talk, to be specific.

Since Seal Talk is built upon Java and Android Studio (based on IntelliJ IDEA), and the iOS version is built on Swift.
The programming language and the developing environment can be regarded as more
powerful actors than other suppliers like Github, but having little interest. They also control and manage the code and version through Github and Gradle, and there are few alternatives.
The other developers, testers, maintainers(we may not know them) can be places in the middle of the graph, a little bit over the moderate grade.

The two less powerful stakeholders are the users and the
competitors, while competitors have the highest interest, like QQ, Wechat.These applications also offer in-time communication service. Apparently, they want
to figure out how does the Seal Talk perform, what technologies are included and
its advantages and shortcomings. Companies who use SealTalk are considered to
have more power than individual users,for example WPS, SEEYON, since the cooperation among companies will force RongCloud to optimize their products and implement more functions,
thus providing higher services. 
Individual developers using Seal Talk may be considered have least power and interest less then company users.

## Context View
### System Scope

SealTalk is an open source instant messaging Android app based on the RongCloud
Instant Messaging SDK (IMKIt), designed to provide developers with development
needs to develop development practices that reduce the time it takes for
applications to go from development to launch. Invest in the application's own
business implementation.

### External Entities

The context model of SealTalk is illustrated in Figure 1. This modeling figure
depicts SealTalk in the center, surrounded by the external entities it interacts
with.

![](https://s2.ax1x.com/2019/10/14/KS0EjS.png)

*Figure 1: External Entities*
