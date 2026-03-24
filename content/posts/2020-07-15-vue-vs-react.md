---
title: "Rewriting the Bus App in Vue"
date: 2020-07-15T15:55:00+00:00
slug: "vue-vs-react"
draft: false
description: "A comparison of a VueJS and React implementation of the same web app. Visually similar, fundamentally different? Let's find out. "
cover:
  image: "/images/2020/06/react_vue-1.JPG"
  relative: false
  hiddenInList: false
tag: ["React", "VueJS", "javascript", "real time app", "vs", "Projects", "bus app"]
---

I felt it was a little [unfair to compare React and Vue](https://westsideelectronics.com/building-real-time-web-app-v2) because I had only just learned React though a course whereas for Vue, it was something that learned a few years ago and didn't have any real understanding of what Vue could offer. So I decided to take this opportunity to learn Vue in greater detail and do a comparison between the two frameworks.

Welcome to Bus App V3.

  
    @import url('https://fonts.googleapis.com/css2?family=Dosis&display=swap');
 

    
    
React
    
    VueJS
    
I decided to use my[ bus arrival web app](https://bustime.neocities.org/) as a basis of comparison and rebuilt it using the Vue framework. The CSS and styling all remain the same. Visually, it is the same app. The only difference is how it is written. Would you even know if it is React or Vue behind the scenes? What if it is changed every other week? I can already hear the cries of *You monster.*

Enough jokes! Here is the latest implementation of the web app, which is basically a reused screenshot from the last time. Just kidding! This is a fresh screenshot. Or is it?

{{< figure src="/images/2020/06/image-14.png" alt="" caption="https://bustime.neocities.org/" >}}

What struck me about the difference between the [first version](https://westsideelectronics.com/vue-vs-react/timetable.neocities.org) and the [second version](https://bustime.neocities.org) was how short the code was. The `HTML` code for the first version was only 51 lines long, and the Javascript code, not counting the Firebase code, was only 8 lines long (44 if you count the Firebase initialization code). This is very short compared to the React implementation, which was 104 lines (about twice as long).

Would Vue really reduce the number of lines I had to write? Was it *better*? I had to find out.

# Getting started

As part of the core tools, Vue has a wonderful tool called `vue ui` that creates a graphical interface to not only set up, but also to configure and run your app. You can configure the starting components for your app, add plugins written by the community for Vue, and start debugging without ever having to touch the command line. This is wonderful for a complete beginner as all the options are laid out in front of you.

For React, you have the `start-react-app`, which generates a generic React template and is used in the command line. This is a fast way to scaffold your app and to get started, although I prefer using a simpler template based on `parcel` rather than `webpack`.

I was a bit skeptical of the `vue ui` tool at first, but then I quickly came around to the benefits of being able to see and set all the configuration options. You can definitely do the same with a decent text editor, but I found it easier to understand the project as a whole through the graphical configuration window. It also provides useful statistics on how fast the page loads under different networks. However, I found the experience a little buggy, and it sometimes errored out without any visual indication on the UI.

# Data binding

Data binding is the task of linking an element in your `HTML` code, which can be a number, a word, or element, to a variable in your `Javascript` code. This is how a static webpage becomes dynamic, displaying content based on how you interact with it.

## React

React does data binding in a very explicit fashion. If there is any data that you want to store and update, you will need to store it as a stateful value with `useState`, which is a function that returns the stateful value, and a function to update it. You cannot update the value directly.

{{< figure src="/images/2020/06/image-12.png" alt="" caption="React update loop" >}}

Therefore, in order to update an element on the page, you'll always need to call a function that updates the stateful value. If you change the element on the page without calling the update function, the value (Data) will not change.

## Vue

Vue does two-way data binding. That means that a change in the variable in the `Javascript` code will trigger a change in the element on the page, and changing the element on the page will also change the variable's value in the `Javascript` code.

{{< figure src="/images/2020/06/image-13.png" alt="" caption="Vue two-way data binding" >}}

I find two-way data binding to be a bit more intuitive, but it is also more prone to error as you are not changing the data explicitly. It is also harder to debug because the state of the data can be changed by the Javascript code or the user on the website. For bigger apps I found myself implementing React's update structure in Vue.

With regards to the bus app, I found Vue was slightly easier to write than React.

# Components

Both React and Vue allow you to create components and add it in similar ways to an existing HTML template.

```HTML
<div>
    <MyComponent />
</div>
```

This is great because it helps to encapsulate code that belong together, making the application easier to manage and allowing the creation of general-purpose, reusable components.

React and Vue differs in component design. In React, components are `Javascript` functions that return `HTML` elements. However, for Vue, components are split up into `HTML`, `CSS`, and `Javascript` parts. The really nice thing is that the `CSS` can also be scoped to just that element, so the design of the component can be readily compartmentalized instead of being spread across multiple files.

# Conclusion

Which do I think is better? I think both React and Vue have their own charm. In terms of code complexity, within the context of my app, it is roughly the same, with Vue having a small edge in terms of clarity of the overall structure. The number of lines written is about the same in both technologies. However, I do feel that React has an edge over Vue if you consider the mass of tutorials, articles, and StackExchange questions that have been written, simply because it has been around for longer.

These two technologies are roughly equal, with small technical benefits over the other. However, my advice is that once you pick one, **do not spend time learning the other.** Because the functionality is approximately the same, you shouldn't waste time relearning the same concepts (unless you are paid to do so). Instead the time can be better invested in **gaining mastery of the framework **that you have chosen.

Ultimately for me I found Vue to be quite enjoyable to work with and is something I look forward to investing more time in the future.
