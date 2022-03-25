# Frontend Architecture Blueprint

**Disclaimer**

The whole architecture is highly opinionated to my knowledge and experience.
And thus this is only written from my point of view.
Because languages, frameworks and my opinion change over time, this architecture is a work in progress.
I *do not* claim that this works for everyone and every project, but it works for all projects that I encountered so far.
I will not go in-depth with all aspects but I want to provide a top-level architecture that allows being extended by other architecture patterns for different needs.
The approach might have a lot in common with other approaches.
I *do not* claim to reinvent the frontend architecture.
I merely define my blueprint.

# Motivation
Before I discuss my blueprint, I have to introduce myself, and why I made up this architecture blueprint.
I see myself as a full-stack developer, especially because I like to simplify and visualize complex processes which help people to do what they can and want to do efficiently.
As a full-stack developer I have my comfort zone in backend and frontend equally.
With backend development, I learned a lot from clean coding, clean code developers, architecture, evolvability, reusability and domain-driven (software) design (DDD).
In frontend, I had to deal with a lot of frustration as I often saw that almost nothing that we learned from backend architecture is applied to frontend client architectures especially to UI elements.
Modern frontend frameworks provide a huge step forward to build efficient UI clients and I think let us apply all known aspects.
During work within both worlds, I made myself a mental model, how the frontend works best for myself by combining all the aspects that I learned software development.
This is my blueprint for an architecture that I think works with all modern frontend frameworks to build a fully functional fat or slim UI client for web and mobile.

# TL;DR Show me a Diagram!
![Architecture Blueprint Diagram](frontend-architecture-blueprint.svg)

# Requirements to my Architecture
I have some requirements for my architecture that explains my intent of the structure:
* Reusability of UI elements,
* Simplicity by using known architecture patterns,
* Separation of concerns in UI, behaviour, state management and features, and
* Definition of domains.

These requirements do not look new to software developers, but I see that they are often missing in frontends.

DDD is not only applicable on a feature level but also on a non-functional level.
Like it is possible to define a domain for security or tracking, I see it is also possible for UI elements.

## What about Model View Control (MVC), Model View Presenter (MVP) or Model View ViewModel (MVVM)?
Actually, in my opinion, it does not matter.
Choose one and stick to it and make it visible to future developers.
It might sometimes be useful to choose other approaches for different features in an app because it saves a lot of boilerplate or it would simply is too confusing.
Some frameworks dictate or prefer a certain kind of approach like Angular strictly prefers MVC.
In web component with React, Vue or Svelte it is your choice how much you want to break down your implementation.

## Separation of Pure UI and Feature Integrating UI
When using a UI library, all UI elements are independent and free of app features.
This is pretty obvious because they were built for anything and it's unable to know all appliances.
When building your app it is very useful to build your own little UI library which i call "Pure UI" from now on.
This might sound like a great deal.
But actually, it is already achieved by a shift of domains.
While the feature implementation focuses on the app features, the UI library focuses on the UI itself without knowing anything of the app.
Each little UI element might have its own little feature-set that is completely unaware of its use in the app while the feature integrating UI places those independent UI elements within a context of the app feature.
The naming of variables, classes and parameters are focused on the UI element in pure UI and on the application context on the feature UI.

As a little example, imagine viewing the `Avatar` of a user.
With the little trick of building a pure UI component that displays any image in the style of the `Avatar` you want to show, you might also want to implement a component that loads that picture, maybe caches it and displays it as that same `Avatar`.
Instead of only building up the integrated version of that `Avatar`, there are 2 components that give a lot of advantages.

The highest advantage of building a pure UI is, that anything can be context-free developed, well documented, bug-fixed and visually tested for itself with a style guide.
Style guides can be automatically generated with each build in the CI pipeline and are called living style guides because their styling is from productive code instead of separate documentation.
The documentation helps keep an overview of what already exists and how it works during the lifetime of the project.
Well-known style guides for web frontend are Styleguidist or StoryBook.
Similar style guides are available for ReactNative, Flutter, Ionic and other mobile libraries.
And for future projects, the little library allows to carry over some components which proved themselves.

Visual testing is considered the most valuable testing of UI.
UX is almost impossible to test automatically by code alone, which leads to no way around visual testing in the end.
And instead of the need to test all edge-cases in the integrated application UI, it is easier to mock all thinkable edge cases within a style guide and test them that way.
This is similar to building unit tests for the backend, which also cover all edge-cases for that unit of code, but this time this is for the UI.
It also gives the opportunity to test out new behaviour of certain UI elements for itself like dropdowns or autocomplete, or the opportunity to fix layout issues on multiple platforms.
It might even be useful to do A/B tests only on single component elements before integrating them into the application or website.

A pure UI might help with debugging because the state is small and clear for the UI only.
Each component defines a unit in the UI which narrows down possible errors because the global state and its business logic are left out.

## Separation of Views from UI Components
In every app, there might be UI elements that are definitely only visible once on the screen like a page and some that are definitely visible multiple times on the same screen.
For development, it always helps to identify and know what the element is made for.
Documentation might help, but it is quite quicker to distinguish between them by separate naming or module conventions like different suffix or a subdirectory.
Each one of them also needs a different documentation style: While UI components stand for themselves, view components often might describe a specific layout with placeholders for UI elements.

## Separation of Feature State and UI State
There is a difference between feature and UI states.
This difference should be visible in the code.
During development, it helps a lot to know what is the business data from the backend and what is only intermediate during a view in the UI.

As a quick example, imagine loading a list of entries from a search API that you want to filter dynamically in the view instead of using an API.
My approach is to keep the original list in one state and compute the filtered list with another state or as the name suggests a computed value.
Any state management library nowadays has computed values based on their state which help.
As to outline, an error-prone approach only stores the filtered list in the state.
But the downside here is, that any filtering must call the API and it might be confusing what the content of that list in the state is if errors occur.
This makes debugging quite harder while separation kinda makes it a no-brainer to follow up what was loaded and what is filtered.

The separation can be simply by using different containers or naming.
In the pure UI, any component is free of any business domain and consists only of its own scoped UI state.
The feature integration components map any business data into UI state.

## Separation of Global State and Component State
When working with states, every developer soon faces various state management libraries.
A major problem of most of those libraries is, that their tutorials are for very small data sets which in itself are quite self-explaining.
It sure is obvious that every state can contain a different domain and a state can compose numerous states in any library.
But the hard time will come when most of the state has shifted to be global and is might become harder to manage.

So my approach is, to think about if a state is necessary to be global or only scoped for one or a set of composed components.
The global state is available to the whole app or website at any time.
This might become an issue if there is a global state which actually may only exist during certain views and requires kind of resets during navigation in the app.
Resetting the global state may harvest errors in future contained subviews and that reset is forgotten.
I think it is possible to handle this also within the routing, but I will not cover this topic now.
More easily it is, to keep a state as scoped as possible.
If it is necessary to have nested components to share a state, it might be useful to consider another way of injecting states in multiple components and still stay reactive.

## Separation of UI and Behaviour
In complex UI components, the code for its behaviour might become quite larger than the styling.
Considering the separation of look and feel can help to clarify what code belongs to each of those aspects.
A major benefit becomes obvious if a component has the exact same look but a completely different behaviour in other cases.
This separation allows composing each behaviour separately in two UI components with the same look.
I compare this to a composition in an object-oriented domain where UI and UX are implemented in objects.

# My Architecture
Back to the diagram.

![Architecture Blueprint Diagram](frontend-architecture-blueprint.svg)

## Routing
The routing defines the global navigation to various screens of an application.
Additional handlers might be necessary to map external URLs in the browser or deep links (universal links in iOS) to the inner routing.
The routing may access the state to change the target route, i.e. to `login` when there is no authenticated user in the state.
But no state may access the routing to prevent dependency cycles and clarify responsibilities.

## Client API
The client API takes care of mapping an inner API to the external API.
An external API might be HTTP-Rest-API, so this part will map inner types to HTTP calls.

Sometimes these parts can be generated if the other API has a self-describing nature, like an Open API doc, swagger or old fashioned WHDL.
Even if there is nothing like this, it still is useful to implement this layer.
The inner API classes help with typing, completeness and clarifies its use.

## Pure UI
A pure UI only takes care of itself.
All components in the pure UI lack any business logic, their state is strictly scoped to itself and its context is always unknown.
It consists of several styling elements or components and composes them as needed.

## Feature integrating UI
This is the glue to combine UI elements with the global or feature state and their business logic.
It initializes new scopes and set up the pure UI to behave as needed for the feature inside the application or website.
