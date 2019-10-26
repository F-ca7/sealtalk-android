# SealTalk

**By ChuYu Xu, YanZhi Fang, YunQing Liu, YuXin Duan, ZuXin Li.**

*Wuhan University*

## Table of contents

- [Introduction](#Introduction)
- [Stakeholder Analysis](#Stakeholder)
- [Context View](#Context-view)
- [Development View](#Development-View) 

## Introduction
Instant messaging is the most popular way for people to communicate on Internet,
and various kinds of instant messaging software are emerging one after another.
Service providers also provide more and more abundant communication services.
SealTalk is an open source instant messaging App based on the RongCloud Instant
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
SealTalk-android as a demo application for their cloud services. They provide cloud service as a part of SealTalk support, and also assigned developers to develop and maintain the system.
Acquirers wants to sold their cloud services, so they want to make it simple, 
and capable to demonstrate the ability of cloud server. 

The property that Acquirers concerned is *cost to build, testability* and *usability*  

**User**

As a real-time communication software, SealTalk has a very broad user base. The
user could use it as a tool for personal communication, or to send message in a
company. Notice that SealTalk's basic function are built for general usage,
enterprise user may need to modified their part. User provides feedbacks to further 
improve the system. For business user,they want SealTalk
to be easily extended for their purpose, while user prefers it to be as easy as possible in using.

The property that User concerned is *Reliability, Usability, Portability* and *Performance* .

**Suppliers**

SealTalk-android is built upon Java, and deployed on Android. They use Android
Studio as development environment. Suppliers provide basic framework and runtime environment for SealTalk.

**Development & Testing**

In GitHub record, most of the early commit are
given by jenkinsrc and Jianli Zhou. They are the employees of RongCloud Inc. 
RongCloud has assigned a group of nearly 5 people to develop SealTalk-android.
Developers implements nearly all the main function of the system.

Developers will focus on *Functional Correctness, Functional Completeness*.

**Competitors**

QQ and WeChat are strong competitors to SealTalk who provides similar services
and have a much wider user.

**Maintainers**

Commits that submit later are mainly given by rc-huangxiujun, an employee of
RongCloud. SealTalk is quite a young project ,there is not so much fork record on GitHub, most of the maintainer are RongCloud's worker. They add extended functions as well as fixing known bugs to SealTalk. They need the system easy to understand, but they will mostly holding the company's interest. Individual developers may customize SealTalk-android for their own purpose and own interests.

Maintainers will mainly focus on *Maintainability, Modifiability* and *Testability*.

![](https://s2.ax1x.com/2019/10/26/KDJ6bD.png)

Figure 1: Quality Requirements that Stakeholders are Concerned with

### Power-interest

![](https://s2.ax1x.com/2019/10/23/KYV6wn.jpg)

*Figure 2: Power-Interest Grid*

In the stakeholder analysis, many different actors with distinct roles, power
and interest have been analyzed. The predominant stakeholder of SealTalk is
RongCloud Inc., because it's the company which it belongs to,which has the most power and interest in this project. Besides, developers of this project, two Github contributors -- jenkinsrc(22commits) and rc-huangxiujun(8commits), have the highest power and interest eithier.

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
competitors, while competitors have the highest interest, like QQ, Wechat.These applications also offer in-time communication service. Apparently, they want to figure out how does the Seal Talk perform, what technologies are included and its advantages and shortcomings. Companies who use SealTalk are considered to have more power than individual users,for example WPS, SEEYON, since the cooperation among companies will force RongCloud to optimize their products and implement more functions, thus providing higher services. 
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

*Figure 3: External Entities*

As we can see,  SealTalk runs on multiple platforms. In fact, versions of different platforms are implemented in different languages. For instance, the Android version is implemented in Java, and the IOS version is implemented in Swift, while the server end is implemented in Node.js. 

## Development View

A development view describes the architecture that supports the software development process. 

### Folder Structure

A description of the top-level folders of the SealTalk-android repository is shown in Table 1 below.

| Folder         | Description                                     |
| -------------- | ----------------------------------------------- |
| contactcard    | an encapsulation of contact card module         |
| gradle         | managing the library dependencies               |
| pushpermission | check push permissions of Android               |
| recognizer     | third party dependencies for speech recognition |
| script         | scripts for building, releasing, etc.           |
| SealTalk       | source code of the Android application          |

*Table 1: description of the top-level folders in SealTalk-android*

Since the source code of the app is in the SealTalk directory, we now take a look inside to figure out its detailed structure from the package perspective, which is shown in Table 2.

| Package Name | Function                                                     |
| ------------ | ------------------------------------------------------------ |
| common       | encapsulate urls, error codes and other constants            |
| contact      | phone contacts management                                    |
| db           | database management, using the **Room**  framework           |
| im           | instant messaging implementation                             |
| model        | data layer, providing various data services for the upper layer |
| net          | network request operations, using the **Retrofit** framework |
| qrcode       | operations for qrcode scanning and recognizing               |
| sp           | managing cache in the application                            |
| utils        | utils for file, network, image, logging, etc.                |
| ota          | online updating tools                                        |
| viewmodel    | bridging View and Model, data returned via LiveData          |
| sms          | short message service                                        |
| task         | different tasks are encapsulated according to different interfaces or data attributes |
| ui           | view layer, responsible for drawing UI elements and interactions with users |

*Table 2: description of the packages in SealTalk*

### Module Structure

A module structure model shows the organisation of the source files into modules that contain related code. Such a structure provides an overview of the source code which guides developers to understand and navigate the codebase.

### MVVM

SealTalk-android actually adopted an **MVVM**(Model-View-ViewModel) architecture, which is shown in Figure 4. The most obvious difference between MVVM architecture and traditional MVC architecture is that MVVM adopts data binding, that is, view changes are automatically reflected in ViewModel. 

<img src="https://s2.ax1x.com/2019/10/26/KDDofU.png" style="zoom:50%;" />

**Figure 4: MVVM architecture**

MVC allows changing the response mode of the view to the user's input without changing the view. The user's operation on the view is handed over to the controller for processing. In response to the view event in the controller, the model interface is called to operate the data. Once the model changes, the relevant view is notified to update. However, in MVVM, the UI is driven by data. Once the data changes, the corresponding UI will be refreshed accordingly. If the UI changes, the corresponding data will also be changed. In this way, we can only care about the data flow in business processing without dealing with the page directly. This greatly achieves the understanding coupling through the **separation of duties**, so as to meet the **modifiability** in quality requirements.

The specific implementation of MMVM is shown in the Figure 5. As a view layer, Activity is responsible for interface display and event interaction. Userinfoviewmodel is a ViewModel layer. It connects view and model, and data is returned through LiveData. ViewModel can obtain different data  sources by calling different tasks.

The task layer is the repository layer. It shields the data source and internal implementation details from the upper layer. The upper layer don't need to know the source, just get the data and process them. In this way, the function of  modules is clear and highly reusable. All data requests only need to be written once, so as to achieve the **modularity** and **reusability** of **maintainability** in quality requirements.

![](https://s2.ax1x.com/2019/10/26/KD7Dzj.png)

Figure 5: MVVM Implementation in SealTalk