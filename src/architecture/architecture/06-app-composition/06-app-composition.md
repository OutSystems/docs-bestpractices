---
summary: An Enterprise ecosystem is mostly likely composed by a set of OutSystems applications. It is crucial that dependencies between them are correctly addressed.
tags: support-application_architecture-overview; support-application_development; support-development; support-Front_end_Development; support-Infrastuture_Architecture
---

# Application composition

An OutSystems application is an assembly of modules (*eSpaces *and *Extensions*), defined in Service Studio. An application constitutes the minimum deployment unit among environments in [LifeTime](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle).

Understanding the dependencies among applications is also key to design a correct architecture. Even if you have a correct module architecture, the wrong application composition can result in:

* Applications not being able to evolve independently (different life cycles).

* Applications that impact each other unnecessarily. Deploying one application may result in having to take all other applications with it to Production.

<pre class="script">
template('MindTouch/IDF3/Views/Guide')</pre>

Check the complete guide on how to design your application architecture in [Designing the architecture of your OutSystems applications](../intro.md).
