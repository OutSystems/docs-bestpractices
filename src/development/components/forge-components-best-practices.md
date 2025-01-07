---
summary: Learn best practices for sharing and developing Forge components in OutSystems 11 (O11) to enhance code reuse and application delivery.
guid: 6f1265f0-a96e-45cb-b813-8460434ba1ab
locale: en-us
app_type: traditional web apps, mobile apps, reactive web apps
platform-version: o11
figma:
tags: component development, code reuse, best practices, outsystems forge, developer experience
audience:
  - mobile developers
  - frontend developers
  - full stack developers
outsystems-tools:
  - forge
  - service studio
coverage-type:
  - evaluate
---

# Forge components best practices

This article describes best practices for sharing OutSystems components on [Forge](https://www.outsystems.com/forge/).

## Introduction

Components for OutSystems enable code reuse and help to accelerate application delivery.

If you've built a component that can help other developers, share it with the OutSystems community on [Forge](https://www.outsystems.com/forge/).

## Forge best practices

Before [publishing your OutSystems component to Forge](https://success.outsystems.com/Support/Forge_Components/Forge_FAQs/Sharing_a_Project#How_do_I_publish_a_component.3F), make sure it follows these best practices.

### Start small { #small }

People will use your component in different contexts; keep in mind the developer experience and skill set.

Start with a simple, small component that focuses on the most common use cases, to promote broad usage across the community. Other options can come through an extensibility mechanism. 

### Follow the development best practices { #best-practices }

To create high-quality components that serve multiple apps and developers, follow the OutSystems best practices:

* [Best practices for OutSystems development](https://success.outsystems.com/Documentation/Best_Practices/Development)
* [The complete guide to creating components](The-Complete-Guide-to-Creating-Components.md)

### Promote Reactive Web over Traditional Web

When building web components, promote Reactive Web over Traditional Web.

### Use icons for visual reference { #icons }

Create a visual reference by using icons that identify your component pieces. Developers can then see them in the development environment. Icons apply to the Application, Modules, Blocks, and Actions.

See [complete guide to creating components](The-Complete-Guide-to-Creating-Components.md#icons) for more information.

### Configure a good preview inside Service Studio { #preview }

Certain styles in UI elements make it difficult to preview your component inside Service Studio. To enhance the developer experience, apply these UI-level improvements:

* Use false conditions to present a friendly UI
* Prefix CSS properties to adapt the layout during development time
* Use sample content inside placeholders

See [complete guide to creating components](The-Complete-Guide-to-Creating-Components.md#good-preview) for more information.

### Distribute your component in OAP format { #oap }

The recommended format for a Forge component is an OutSystems Application (OAP). This format makes it easier to install and upgrade the component and it keeps your environment more organized.

### Have a meaningful name and description { #name-desc }

A clear name and description allow community members to quickly understand the component use case and to decide if it fits their needs.

Make sure the component **name**:

* Indicates what the component does
* Doesn't contain file extensions or a lot of special characters

| Do      | Don't   |
|---------|---------|
| Simple CRUD Example | Create,read,update,delete app |
| Environment Dashboard | EnvDashboard_v4.oml |
| Google Calendar Connector | Calendar |

Make sure the component **description**:

* Isn't a one-line description
* Describes why you build it
* Explains the problem the component is trying to solve
* Showcases note-worthy features
* Lists URLs for libraries included, API versions, and references to external resources

| Do      | Don't   |
|---------|---------|
| If you're working with a lot of services, it can be hard to know which applications are consuming web service methods. This component aims to improve visibility for web services consumed and exposed in your OutSystems environment. Works by inspecting the OutSystems metamodel and presenting that information in a more readable format. | Know which web services are being consumed and exposed in your OutSystems environment. |
| This application aims to showcase best practices when integrating with external services. In this app, you can learn how to consume and expose a REST API, with a focus on how to authenticate. | How to make a wrapper for a simple external service. |

### Provide clear and concise documentation { #docs }

Regardless of a component's complexity, users always need help, even if the code is exemplary. Demos are awesome and helpful but it's not always possible to have one. Documentation is the next best thing to help community members.

Make sure the component's **documentation**:

* Includes the required steps to install and implement it. For example, how do you create developer accounts in other services, configure authentication, run timers, change site property values, or configure other settings.

* One thing developers look for is public methods and their signature to understand if the component fits their needs. Use documentation to give an overview of what public methods/Web Blocks are available.

### Provide a demo application { #demo }

Whenever possible, make sure you create a **separate application** that consumes your component logic and **demonstrates the most common use cases**. By doing this, you enable better testing on your side and make it easier for community members to understand what the component does. 

Additionally, having an application for demo purposes only avoids the need to push demo code to production along with the component's core functionality, as developers can publish the demo only in the development environment.

Consider the following best practices:

* The demo application name should include the component's name and "Demo", or "Example", as users search by component name when they need help.

* The demo should be **guided** and **straightforward**, with one single or central screen if possible, and it shouldn't include a login.

### Support and maintain your component { #support }

Supporting fellow developers who use your component is important. A few things to help you achieve this goal are:

* Follow the [OutSystems development best practices](https://success.outsystems.com/Documentation/Best_Practices/Development) and the [components best practices](The-Complete-Guide-to-Creating-Components.md) to ensure better guidance and maintenance.
* Use the latest OutSystems version to avoid issues with legacy code and make use of new capabilities.
* Detail changes whenever you upload a new version. This helps other community members know if an issue they're experiencing is already solved.

As a component owner, you're responsible for it, and you'll likely  receive queries on its functionality or implementation. Do your best to be helpful. If you don't have time to respond or iterate on the component, consider inviting other community members to join the team.

## Have OutSystems certify your component { #trusted }

The OutSystems Curation Team can certify that a component delivers the promised functionality and complies with defined quality standards. A certified component appears in the community with a Trusted label.

See [all the requirements](https://success.outsystems.com/Support/Forge_Components/Forge_FAQs/Trusted_components_requirements) your component must follow to become a Trusted component.
