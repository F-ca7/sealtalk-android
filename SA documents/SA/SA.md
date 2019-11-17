# SealTalk

**By ChuYu Xu, YanZhi Fang, YunQing Liu, YuXin Duan, ZuXin Li.**

*Wuhan University*

## Abstract

SealTalk is an open source instant messaging App based on the RongCloud Instant
Messaging SDK (IMKIt). It meets the social communication needs, providing single group chat, super group and other chat modes, support picture, voice and small video, real-time message push, highly customized interface, high-definition audio and video call, effectively improve user stickiness and activeness. It aims to provide developers with IM development needs with reference
development practices, thereby reducing the time from development to online application, and devoting more energy to the application's own business implementation.

## Table of contents

- [Introduction](#Introduction)
- [Stakeholder Analysis](#Stakeholder)
- [Context View](#Context-view)
- [Development View](#Development-View) 
- [Functional View](#Functional View)
- [Usability Perspective](#Usability-Perspective) 
- [Tactics](#Tactics) 
- [Deployment View](#Deployment View)
- [Evolutionary Perspective](#Evolutionary-Perspective)
- [Technical Debt](#Technical-Debt)
- [Conclusion](#Conclusion)
- [References](#References)

## Introduction
Instant messaging is the most popular way for people to communicate on Internet,
and various kinds of instant messaging software are emerging one after another.
Service providers also provide more and more abundant communication services.
SealTalk is an open source instant messaging App based on the RongCloud Instant
Messaging SDK (IMKIt).

IMKit includes two UIs: session list and session page. SealTalk
adds login UI, contact UI, discovery UI and "I" UI to provide a complete set of
instant messaging App demo instances. It has five advanced functions: server real-time message routing, single group chat message cloud storage, online status subscription, broadcast message and push, multi device message synchronization[1], which greatly improve the IM capability and user experience.

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

*Figure 1: Quality Requirements that Stakeholders are Concerned with*

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

Since the source code of the app is in the SealTalk directory, we now take a look inside to figure out its detailed structure from the package perspective[2], which is shown in Table 2.

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

A module structure model shows the organization of the source files into modules that contain related code. Such a structure provides an overview of the source code which guides developers to understand and navigate the codebase.

### MVVM

SealTalk-android actually adopted an **MVVM**(Model-View-ViewModel) architecture, which is shown in Figure 4. The most obvious difference between MVVM architecture and traditional MVC architecture is that MVVM adopts data binding, that is, view changes are automatically reflected in ViewModel. 

<img src="https://s2.ax1x.com/2019/10/26/KDDofU.png" style="zoom:50%;" />

*Figure 4: MVVM architecture*

MVC allows changing the response mode of the view to the user's input without changing the view. The user's operation on the view is handed over to the controller for processing. In response to the view event in the controller, the model interface is called to operate the data. Once the model changes, the relevant view is notified to update. However, in MVVM, the UI is driven by data. Once the data changes, the corresponding UI will be refreshed accordingly. If the UI changes, the corresponding data will also be changed. In this way, we can only care about the data flow in business processing without dealing with the page directly. This greatly achieves the understanding coupling through the **separation of duties**, so as to meet the **modifiability** in quality requirements.

The specific implementation of MMVM is shown in the Figure 5. As a view layer, Activity is responsible for interface display and event interaction. Userinfoviewmodel is a ViewModel layer. It connects view and model, and data is returned through LiveData. ViewModel can obtain different data  sources by calling different tasks.

The task layer is the repository layer. It shields the data source and internal implementation details from the upper layer. The upper layer don't need to know the source, just get the data and process them. In this way, the function of  modules is clear and highly reusable. All data requests only need to be written once, so as to achieve the **modularity** and **reusability** of **maintainability** in quality requirements.

![](https://s2.ax1x.com/2019/10/26/KD7Dzj.png)

*Figure 5: MVVM Implementation in SealTalk*

## Functional View

The functional view defines the architectural elements that deliver the SealTalk’s functionality. The view will shows the key functional elements and the external interfaces of SealTalk in the following part of this section.

### External interface

The external interfaces provided by SealTalk mainly concern how to help users to build their own instant messenger or simply do some customization based on the demo app. They also include complete sample codes. For instance, the functionality concerns receiving and sending message, retrieving information, creating or removing the chat rooms and groups, as well as changing the user interface. Next to that it also provides utilities as net services, qrcode processing, file management and so on. There are too many interfaces to completely list them in this report, while they're quite complete for building a message application. On the other hand, others just need to revise few codes in some functions without changing the internal structure, if they want to add new modules or integrate it into their own application.

### Functional capabilities

Functional capabilities define what the system is required to do. The most important and unique functionalities will be described in this part. Since SealTalk is an instant messaging app, its functionalities mainly include four categories: registration and login, session, address book, personal information management. Each category will have a detailed introduction below.

#### Registration and login

Registering is a function that no user can avoid when they are using SealTalk for the first time. SealTalk's account needs to be bound with the mobile number. During the registration, users need to fill in a nickname, password and mobile number. After entering the mobile number, clicking the button on the right will make them receive a verification code sending by the system. Only the correct verification code can complete the registration. The registration process just needs to be carried out once. When using SealTalk for the second time, users can directly log in and enter their own account. The login only needs to fill in the user name and password.

Of course, it's common for many users to forget their password. If you encounter such a thing, don't worry. Just use your mobile number can easily set a new password.

#### Session

Session is the most basic and important functionality of SealTalk. Session types include single session, group session, chat room, discussion group, etc. T here is a plus sign in the upper right corner of the session interface. Click the plus sign to use three functions: create chat, add friends and create group chat. Users should first add friends by searching email or mobile number in the add friends interface. After adding friends, users can create single or group chat with friends freely. Session not only supports text, but also voice, picture, image, location, red packet, business card and other message types. At the same time, it can also display the sending status of messages in the session interface. 

Chat room is a very special way of session. Unlike group sessions, chat room sessions can be created among strangers. Users can find the chat rooms through the "discovery" function and join one that is interesting to them. There is no maximum number of members in the chat room session and the information in the chat room will not be received after exiting the interface. It is suitable for live chat rooms, roadshows and other temporary scenes.

#### Address book

The address book in SealTalk can be used not only to find friends, but also to search for group and public numbers. In addition to the above functions, if a stranger wants to add you as a friend, this information will also be displayed in the address book. The history of adding friends will also be kept in the address book for a period of time, and users can view it at any time.

#### Personal information management

In the personal information management interface,users can choose a favorite picture as their avatar and give themselves a nickname. This function helps to distinguish each user and make them more personalized. It also supports some account management transactions, such as changing account password, logging out, clearing chat records, etc. If users meet any problems when using the app, they can contact customer service in "feedback" and customer service will respond to users' comments as soon as possible. At the same time, some information about SealTalk can also be viewed through this interface. SealTalk provides a product introduction for new users to familiarize them with the basic functions of the software and explain some complex functions that are not easy to understand. The version of the product and the SDK version used are also displayed. If users want to know more information, they can click "about SealTalk" to link to the official website.

## Usability Perspective

Usability is a term that too abstract to be studied directly. Here we further divided it into three parts by following FCM model.

### Operability

For user, the chat view is designed as simple as possible, no redundant buttons or panels for end users. In main Activity(see figure 6), Group session and Friend session is placed on the center of the screen, with some helpful items on the top. For developer, the code is well-organized with sufficient folder, helping developer to review or customize the compartment he wants easily.


![](https://s2.ax1x.com/2019/11/02/Kqofit.jpg)

*Figure 6: SealTalk Main Activity*

### Training

SealTalk's UI is very similar to its competitor, like WeChat or QQ, which may due to the acquirer's concern to reduce the cost to build, especially the cost to redesign the UI. It is likely that user can switch to SealTalk from WeChat without training cost. It is easy to found that the design(see figures 8 9 10) is largely influenced by WeChat. SealTalk have packed all the code and sored by their utility, and leave Chinese notes for developers to quickly get started with Rongcloud's cloud service.(see figure 7)

<img src="https://s2.ax1x.com/2019/11/02/Kqo4Rf.jpg" style="zoom:67%;" />

*Figure 7: SealTalk's note for developers*

### Understandability

For User, the UI is simple to discover. SealTalk have reused all the icon from QQ, thus it is accomplishing understandability with the minimum cost.(see figures 8 9 10)

<img src="https://s2.ax1x.com/2019/11/02/Kqo5z8.jpg" style="zoom: 50%;" />

*Figure 8: SealTalk add new friend Activity*

<img src="https://s2.ax1x.com/2019/11/02/KqoRII.jpg" style="zoom:67%;" />

*Figure 9: SealTalk group chat Activity*

<img src="https://s2.ax1x.com/2019/11/02/KqohJP.jpg" style="zoom:67%;" />

*Figure 10: SealTalk friend Activity*

## Tactics

Tactics are a collection of primitive design techniques that an architect can use to achieve a quality attribute response.

### Push & Pull

As we have mentioned in the introduction part, SealTalk have integrated user social functions, the core function of which is Feeding[3]. After a user publishes some content, the backend server displays the acquired data to the user's fans in a certain way. The common tactics are **push and pull**. 

In **push**, the process of a user publishing content is as follows: (1) A user with id #1 published a message; (2) The backend wrote the content to the database; (3) Search in the *followers* table where user_id=1; (4) Write the content to the *received_content* table where the receive_id equals to the follower's id in step3; (5) When the followers check their feeds, the backend will query in the *received_content* table. Obviously, the **push** adopts the strategy of **space for time**.

In **pull**,  the process of a user publishing content is as follows: (1)A user with id #1 published a message; (2) The backend wrote the content to the *send_content* table;  (3) When another user with id #2 check his/her feeds, make a query in the *followings* table and find that it should acquire the content published by #1; (4) The backend again make query in the *send_content* table where author_id=1 to display the message content. t can be seen from the above that the **pull** adopts the strategy of **time for space**, since users can publish their contents with high efficiency, however when displaying the feeds, it costs lots of time in the aggregation operations.

The table 3 shows their advantages and side-effects.

|  | Push     | pull |
| -------------- | ------------ | -------------|
| publish contents    | push to all the followers | no pushing |
| display feeds         | done by a single SQL | needs lots of aggregation operations |
| modify contents | higher cost  | lower cost |
*Table 3: advantages and side-effects*

Since the SealTalk is served for companies or enterprises, not for the public, which means it is not likely to posses a large number of users like Weibo ,it adopts the **push** tactics to improve is **performance** quality attribute.

## Deployment View

SealTalk-Android runs on Android platform, so its developing and deploying relies on anything that a typical android application does.

SealTalk-Android is build via Android studio, an IDE for android application developed by google. Android studio uses gradle to manage build settings for applications. For SealTalk-android, the version of gradle is 3.3.2 by default, and it could be swapped by Android studio automatically.

SealTalk-Android, as its name, is designed to running on Android platform. Android platform has three layers on the bottom of application: Linux kernel- Dalvik Virtual Machine and Application FrameWork. Google packed them and marked as android SDK version. For SealTalk android, its target SDK version is 28, with minimum SDK version 19, which means it should be running normally on Android platform version Through Android 4.4 to 9.0 .  

Besides, it requires internet connection, and 23.5mb of memory required for the APK, while typically a real time communication software requires more spaces for the messages.

Last but not least, the source code itself is not completed. RongCloud's release SealTalk-android for their cloud services, the application cannot be started without a copy of permission license for RongCloud's IM realtime talk SDK.  The project is using RongCloud SDK2.9.19 for its communication part, so it is required to purchase a license from RongCloud, and hard code the ID before building the application.  

<img src="https://s2.ax1x.com/2019/11/17/MDb00O.jpg" style="zoom: 80%;" />

*Figure 11: Deployment View*

## Evolutionary Perspective

The evolutionary perspective deals with concerns related to evolution during the lifetime of a system. SealTalk has changed a lot since its initial release, so it is necessary to learn the evolution perspective of it. In the following of this section, the evolution of SealTalk will be shown in detail.

By investigating the changelog of the product, we can see all notable changes from the first release to now. The specific time when the first version SealTalk 1.0.0 was released has not been found. We speculate that it was released in 2016 because the first changelog was updated in July 2016. SealTalk 1.0.0 already has the basic functions of instant messaging software. All of the following updates are based on the original version, optimizing, modifying and adding some new functions. SealTalk 1.1.0 was the first version documented in the changelog. In this version, group function and message recall function were added. People could set up a group to chat with multiple people at the same time. Also the structure of code was improved in this version. SealTalk 1.1.1-1.1.4 had done a lot of function optimization and repair work. And then in SealTalk 1.1.5 which was released at the end of 2016, the function of sending red packets was added. In 2017 and 2018, SealTalk has experienced dozens of releases, but the changes are very small. In addition to continuous optimization of the interface and existing functions, only a few small functions have been added, such as business card added in April 2017, dynamic expression added in September 2018 and local video sending added in November 2018. SealTalk 1.3.11 released in February 2019 was a more meaningful version. From this version, SealTalk supported the use of international mobile number to register accounts. And people can switch SealTalk in the UI interface. This means that SealTalk has begun to move towards internationalization. People from different countries could use it to communicate with each other. Since SealTalk 1.3.14, the audio and video engine referenced by calllib module has been replaced with RTC 3.0, which is not interoperable with the previous version. In May 2019, SealTalk experienced a relatively large update, SealTalk 2.0.0 came out. The new version adds three functions: identification QR code, sending group announcement and message forwarding. The latest release is SealTalk 2.2.1. At present, it has many functions that it didn't have when it was first released and many of the original functions have been optimized. But it still has a lot to improve. In the future, many new versions will be released to improve it step by step.

Since SealTalk is an open source instant messaging App based on the Rongyun Instant Messaging SDK. The update of SealTalk is closely connected with the update of the version of Rongyun SDK. From 2016 to now, the version of SDK has also been updated dozens of times. The latest release updates the SDK to 2.10.0.

![](https://s2.ax1x.com/2019/10/30/K4KVJO.png)

*Figure 12: The Evolution of SealTalk*

## Technical Debt

Technical debt is a concept in programming that reflects the extra development work that arises when code that is easy to implement in the short run is used instead of applying the best overall solution[5]. 

> According to Ward Cunningham, who coined the "technical debt", it is used to describe the obligation that a software organization incurs when it chooses a design or construction approach that's expedient in the short term but that increases complexity and is more costly in the long term.

In the next few parts,  the debt will be analyzed in various aspects by comparing different versions of seal-talk, including architectural debt, code debt, testing debt, documentation debt. Three released version will be chosen  for comparative analysis, the latest version(SealTalk _2.1.0), last stable version(SealTalk _1.3.21), the first version(2.9.9_dev).

### Architectural Debt

Since the SealTalk _1.3.14, the audio and video engine referenced by the CallLib module has been replaced with RTC 3.0, which is not compatible with previous versions. And in the version 2.0+, SealTalk reconstructs the internal logic implementation to make the overall code cleaner and easier to read. Based on MVVM pattern, LiveData + ViewModel + Retrofit 2.0 + Room and other frameworks were used for development. 

> Because DataBinding was difficult to debug, and had to be written in XML, the decision was made to deactivate DataBinding. 

These are the two biggest progress in the project, each version is incompatible with the old version, which makes the old version directly obsolete and not maintained. As shown in the Github, all issues about the old versions are closed and the only suggestion is to try the latest version, which may make some developers annoyed. Even SealTalk 2.0 hasn't completely solve the Android X compatibility problem. 

After looking in to the project structure in the previous version and the latest one. It's obvious that the file organization structure is becoming terser, and large part of codes split into small ones.  For instance, `Friend.java` and `FriendDao.java`(which are relative to manipulating database), are divided into `FriendBlackInfo.java`, `FriendDescription.java`, `FriendInfo.java`, `FriendShipInfo.java`, and `FriendStatus.java` in 2.1. Therefore, the overall code becomes easier to maintain while more functions are implemented. Other progresses are achieved like sources are merged into one package, redundant libs are removed and so on. It is clear that they are paying down their debts.

### Code Debt

Android-studio offers code inspection function. After running it on the master branch, the result is:

| Type     | Errors | Warnings   | Description                                                  |
| -------- | :----- | :--------- | ------------------------------------------------------------ |
| Android  | 46     | 927        | Accessibility, Compliance, Correctness, Security, Usability... |
| General  | 6      | -          | Annotator & Syntax error                                     |
| HTML     | -      | 4          | Unknown HTML tag                                             |
| Java     | 7      | 4403       | Code style, Declaration redundancy, Javadoc issues...        |
| RegExp   | -      | 1          | Redundant character escape                                   |
| Spelling | -      | 7762 typos | Typo                                                         |
| XML      | -      | 24         | Unused XML schema declaration, empty body                    |

*table 3: Code Inspection*

The issues in Java part is extremely serious, with 1917 warnings of declaration redundancy, including 36 empty method :

```java
AddFriendActivity()
SearchFriendActivity()
SwitchButton()
ThreadManager()
...
```

(maybe haven't been implemented yet), unused return value, unused declaration, less strict access modifiers, and loss of final modifier. And the SealTalk seems not to follow the regulations of Javadoc. There are 7 errors and 1063 warnings found in javadoc part.

This inspection points out the following javadoc comment flaws:  

- *no javadoc where it is required*
- *required tag is missing* 
- *invalid or incomplete tag*
- *javadoc description is missing or incomplete*

On the other hand, another way of identifying code debt is  searching for "TODO" and "FIXME" comments in the codebase. In the latest version 2.1.0, there are **19 TODO** and **2 FIXME** items in 16 Files. They are either written in Chinese or English. It seems that some of them were proposed by previous developer and haven't finished yet, being stuck for a long time.

### Testing Debt

Testing code coverage of SealTalk is had to do, because it's an Android-studio project and the developers do  not write some unit test. Even running the demo, the service address where you deploy SealTalk need to be provided to replace the default value in source code, as well as RongCloud AppKey. 

At present, we intend to abandon the assessment of this part of technical debt

### Documentation Debt

Developers need to write documentation of their source code, which may be of help for other developers to understand their code. More importantly, when they would not work anymore for a project, people coming after and new contributors should also be able to understand the system and its setup. In addition, documentation for the end user of the product is needed in order to understand how to download, install and use the product. 

SealTalk 2.0+ gives the detailed architecture diagram to help the developers and users understand the structure easily, while they just gave a simple UML diagram in previous version. 

Besides, on the GitHub page of SealTalk, there is a link to [SealTalk Server](https://github.com/SealTalk/SealTalk-server). This project is a back-end service for SealTalk series of applications, providing user, friend, group, blacklist related interfaces and data management services. In the project home directory, Readme file gives detailed installation and usage guide. Until now, few issues have been opened about this aspect, and quite part of them are just related to Android X compatibility problems([#20](https://github.com/SealTalk/SealTalk-android/issues/20), [#35](https://github.com/SealTalk/SealTalk-android/issues/35))[4], which have been already solved or due to using the old version SealTalk.

From these perspectives, SealTalk seems to be out of debt in the documentation section. They provide a complete set of documentation and flowcharts for the user to understand. This includes how to deploy the front and back end, API list documentation, SDK integration instructions and so on.

## Conclusion

## References

1. RongCloud.  https://www.rongcloud.cn/product/im, 2019.
2. GitHub.com. SealTalk-android.  https://github.com/SealTalk/SealTalk-android/tree/master/SealTalk/src/main/java/cn/rongcloud/im, 2019.
3. Wikipedia.  https://en.wikipedia.org/wiki/Facebook_News_Feed, 2019.
4. GitHub.com. https://github.com/SealTalk/SealTalk-android/issues, 2019.
5. Technopedia. Technical debt. https://www.techopedia.com/definition/27913/technicaldebt,
   2017.