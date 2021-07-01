---
summary: Learn the best practices to share a component in OutSystems Forge.
en_title: Forge components best practices
---

# Forge components best practices

This article describes the best practices for sharing OutSystems components in the [Forge](https://www.outsystems.com/forge/).

## Introduction

OutSystems components enable reusability of code and are an important piece to accelerate application delivery. The [OutSystems Forge](https://www.outsystems.com/forge/) is the place where you can share your components with the OutSystems community.

## Forge best practices

Before [publishing your OutSystems component to Forge](https://success.outsystems.com/Support/Forge_Components/Forge_FAQs/Sharing_a_Project#How_do_I_publish_a_component.3F), make sure it follows the best practices below.

### Follow the development best practices { #best-practices }

To create high-quality components that can serve several apps and developers, make sure you know and follow the OutSystems best practices:

* [Best practices for OutSystems development](https://success.outsystems.com/Documentation/Best_Practices/Development)
* [The complete guide to creating components](The-Complete-Guide-to-Creating-Components.md)

### Start small { #small }

Your component will be used by several people in different context, so keep in mind the developer experience and their skill set.

Start by a simple small component that focus on the most common use cases. This promotes its usage among the community. Other options can come through an extensibility mechanism.

### Use icons for visual reference { #icons }

Create a visual reference by using icons that identify your component pieces. This way, developers can actually see them in the development environment. Icons apply to the Application, Modules, Blocks, and Actions.

See more details in the [complete guide to creating components](The-Complete-Guide-to-Creating-Components.md#icons).

### Configure a good preview inside Service Studio { #preview }

There are certain styles in UI elements that may difficult the preview of your component inside Service Studio. To enhance the developer experience, there are some UI level improvements that you can apply. Some examples are:

* Use false conditions to present a friendly UI
* Prefix CSS properties to adapt the layout during development time
* Use sample content inside placeholders

See more details in the [complete guide to creating components](The-Complete-Guide-to-Creating-Components.md#good-preview).

### Have a meaningful name and description { #name-desc }

A clear name and description allow community members to quickly understand the use case the component solves and if it fits their needs.

Make sure the component's **name**:

* Hints at what the component does
* Doesn't contain file extensions or abundance of special characters

| Do      | Don't   |
|---------|---------|
| Simple CRUD Example | Create,read,update,delete app |
| Environment Dashboard | EnvDashboard_v4.oml |
| Google Calendar Connector | Calendar |

Make sure the component's **description**:

* Isn't an one-liner description
* Includes why it was built
* Explains the issue the component is trying to solve
* Showcases features worth of notice
* Includes used libraries URL, API version, and references to external resources.

| Do      | Don't   |
|---------|---------|
| If you are working with a lot of services it can be hard to know which applications are consuming those web services methods. This component aims to improve visibility over what WebServices are being consumed and exposed in your OutSystems environment. Works by inspecting the OutSystems metamodel and presenting that information in a more readable format. | Know which web services are being consumed and exposed in our OutSystems environment. |
| This application aims to showcase best practices when integrating with external services. In this app you can learn how to consume and expose a REST API, with a focus on how-to authenticate. | How-to make a wrapper for a simple external service. |

### Provide clear and concise documentation { #docs }

Independently of complexity, users always need help, even if the code is exemplary. Demos are awesome and of great help but it's not always possible to have one. Documentation is the next best thing that can help a fellow community member.

Make sure the component's **documentation** meet the following:

* Include any steps needed for the component to install and work correctly: create developer accounts in other services, configure authentication, run timers, change site property values, or other settings.

* One of the things developers look for is public methods and their signature to understand if the component fits their needs, use documentation to give an overview of what public methods/web blocks are available.

### Provide a demo application { #demo }

Whenever possible, make sure you create a separate application that consumes your component logic and **demonstrates the most common use cases**. By doing this, you enable better testing from your side and better understanding of what the component does without community members having to install it.

Consider the following best practices:

* The demo application name should include the component's name and "Demo", or "Example", since users will search for the component's name when they need help.

* The demo should be **guided** and **straightforward**, with one single or central screen if possible, and without login.

### Distribute your component in OAP format { #oap }

The recommended format to distribute a component in Forge is as an OutSystems Application (OAP). This format enables easier installation and upgrade of the component, and keeps your environment better organized.

### Keep the component relevant { #relevant }

An important part of having a component available to the community is making sure that you can support a fellow developer when they're using your component. A few things that can help you on that:

* Follow the [OutSystems development best practices](https://success.outsystems.com/Documentation/Best_Practices/Development) and the [components best practices](The-Complete-Guide-to-Creating-Components.md) to ensure better guidance and maintenance.
* Use the latest OutSystems version to avoid issues with legacy code and make use of the new capabilities.
* Detail the changes whenever you upload a new version. This helps other community members know if an issue they're experiencing is already solved.

As the owner of the component you are responsible for it, and you'll probably receive queries on its functionality or implementation. Do your best to be helpful. If you don't have time to respond or iterate on the component, consider inviting other community members to join the team.

## Have your component trusted by OutSystems { #trusted }

The OutSystems Curation Team is entitled to grant the Trust curation stamp to components that deliver the promised functionality and comply with a defined set of quality standards.

See [all the requirements](https://success.outsystems.com/Support/Forge_Components/Forge_FAQs/Trusted_components_requirements) your component must follow to be considered a Trusted component.
