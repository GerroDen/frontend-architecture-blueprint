# Disclaimer
The whole architecture is highly opinionated to my personal knowledge and experience.
And thus this is only written from my point of view.
Because languages, frameworks and my opinion changes over time, this architecture is work in progress.
I do **not** claim that this works for everyone and every project, but it works for all projects that i encountered so far.
I will not go in depth with all aspects but want to provide a top level architecture which allows to be extended by other architecture patterns for different needs.

# Motivation
Before I discuss my blueprint, I have to introduce myself, and why I made up this architecture blueprint.
I see myself as a fullstack developer, especially because I like to simplify and visualize complex processes which help people to do what they can and want to do efficiently.
As fullstack developer I have my comfortzone in back- and frontend equally.
With backend development I learned a lot from clean coding, clean code developers, architecture, evolvability, reusability and domain driven (software) design (DDD).
In frontend I had to deal with a lot of frustration as I often saw that almost nothing what we learned from backend architecture is applied to frontend client architectures especially to UI elements.
Modern frontend frameworks provide a hugh step forward to build efficient UI clients and I think let us apply all known aspects.
During work within both worlds I made myself a mental model, how the frontend works best for myself by combining all the aspects that I learned software development.
This is my bleprint for an architecture that I think works with all modern frontend frameworks to build a full functional fat or slim UI client for web and mobile.

# Requirements to my Architecture
I have some requirements to my architecture which explains my intent of the structure:
* Reusability of UI elements,
* simplicity by using known architecture paterns,
* seperation of concerns in UI, behaviour, state management and features, and
* definition of domains.

These requirements do not look new to software developers, but I see that they are often missing in frontends.

# My Architecture
> TODO

## Separation of Domains
DDD is not only applicable on a feature level, but also on a non functional level.
Like it is possible to define a domain for security or tracking, I see it is also possible for UI elements.
> TODO

### Separation of Pure UI and Feature Integrating UI
> TODO

### Separation of Views from UI Components
> TODO

### Separation of Feature State and UI State
> TODO

### Separation of Global State and Component State
> TODO

### Separation of UI and Behaviour
> TODO

## Routing
> TODO

## Client API
> TODO

## Pure UI
> TODO

## Feature integrating UI
> TODO

# References
* https://flutter.dev
* https://vuejs.org
* https://reactjs.org
* https://svelte.dev
* https://angular.io

