---
title: Reflections on Upgrading from Vue1 to Vue2
date: 2017-01-10
locale: en_US
---

I spent approximately one week upgrading the membership card module I was responsible for from Vue1 to Vue2 at my company. During this process, I encountered numerous challenges, most of which stemmed from the original code's significant deviation from Vue2's best practices (primarily regarding unidirectional versus bidirectional data flow), and partly from fundamental changes in Vue2's underlying architecture (notably the introduction of Virtual DOM).

## Motivations and Considerations for Upgrading

The considerations for upgrading essentially boil down to two aspects: whether the upgrade meets current and future requirements, and weighing the benefits against the costs.

### Requirements

* First Contentful Paint server-side rendering needs: Due to having two technology stacks—one with PHP templates and another with a decoupled Vue and Play for Scala—there was a noticeable blank loading time when transitioning between old and Vue-based pages
* Component validation requirements: At the time, the officially recommended vue-validator did not support this feature, but it was planned for version 3.0, which would only support Vue2. This was a significant factor, suggesting that Vue's ecosystem would likely develop and iterate based on Vue2
* Existing bugs that were fixed in Vue2, such as the v-model number modifier turning an empty input to 0 (though this bug was later fixed in Vue1 as well, but smaller bugs might not continue to be addressed)

### Benefits

Beyond solving the aforementioned requirements and problems, I couldn't find a definitively compelling advantage that would mandate upgrading from Vue1 to Vue2.

### Costs

* Migration time: Estimating this before actually modifying the code is challenging. Using vue-migrate-tool, I detected approximately hundreds of modification points (not including Vuex). I initially estimated two to three days, but it actually took a week. Moreover, during the upgrade process, the project could not run completely (by "completely", I mean that migrating module by module allowed partial runnable and testable sections). This also includes time needed to upgrade other projects developed or in development with Vue1
* Post-migration risks: Even with comprehensive unit testing, there's no guarantee of consistent behavior compared to the Vue1 version
* Learning costs: This was much easier compared to Angular. Most changes were API renamings at the syntax level, and most changes involved subtraction—removing rarely used or redundant APIs. However, Vue2 modified some underlying elements (primarily Virtual DOM), causing semantic style shifts (in my opinion, becoming more React-like)
* Server-side rendering considerations: This was no longer just a frontend issue but involved architectural changes across frontend and backend, potentially requiring adding a Node layer as a "frontend backend". Feasibility and project timeline needed careful evaluation. (Ultimately, this project used Play to directly render some common components like sidebars and top navigation, then loaded Vue JS files to render the remaining parts)

In summary, if no decisive upgrade factor is found, proceed cautiously. Consider applying Vue2 best practices in projects currently under development and those planned for the future, minimizing the use of deprecated Vue2 elements.

## Vue2 vs Vue1

Let's dive into the significant changes between Vue2 and Vue1.

### Unidirectional vs Bidirectional Data Flow

A common Zhihu discussion about Vue's "progressive framework" mostly interprets it as "advocating the minimum". In the unidirectional versus bidirectional debate, Vue took a stance. In Vue1, child components could modify props passed from parent components; in Vue2, this is no longer allowed. During my Vue1 development, I extensively used bidirectional data flow, and converting previous `sync` implementations to custom events consumed most of my migration time.

Where did I use `sync`?

#### Generic Base Components

Components like Popup and Sidepage require a variable to control visibility. In a bidirectional data flow, this variable could be opened by the parent component and closed by the child component. In a unidirectional flow, closing requires the child component to trigger a custom event, which the parent component listens to and then sets the variable to closed. (In Vue1, there was another approach where this variable could be local component data, with the parent component triggering a child component event to complete the operation, but in Vue2, events can only flow upward)

Comparing these scenarios, bidirectional implementation is simpler (and feels more convenient), while unidirectional is slightly more complex but still offers benefits. Expanding this scenario: if closing involves additional actions, bidirectional flow gives control to the child component, requiring passing a callback function. In a unidirectional flow, the parent component controls the process by adding logic to the original event callback. Ultimately, the consideration is about data control: does it belong to the parent, the child, or is it neutral?

#### Nested Multi-level Components

When data from an ancestor component is passed through multiple layers, with many components requiring it and final modifications happening deep within the component tree, the situation changes. As Vue's documentation suggests, bidirectional flow raises data safety concerns, with any child component potentially modifying shared data and causing unpredictable errors. Unidirectional flow becomes even more complicated in such scenarios. I ultimately converted all such cases to use Vuex. (Previously in Vue1, I only used Vuex for non-parent-child horizontal data sharing)

Note: Vue2 documentation mentions an exception—for reference-type data, preventing child components from modifying parent data is not possible.

### Rendering Functions and Virtual DOM

I consider this the most significant change in Vue2: introducing a process of calculating new Virtual DOM through rendering functions, situated between dependency collection and actual DOM modification. Templates are ultimately compiled into rendering functions, and these functions operate at the component level, necessitating a re-examination of templates.

Templates might contain:

- Standard data bindings
- Dynamically bound DOM attributes
- Props
- Event callbacks
- Filters
- Directives

Any of these template elements could introduce side effects that repeatedly occur during rendering function calculation. For instance, using reference-type literals in props causes object recreation in each render, involving memory and performance overhead and potential unknown errors from reference comparisons. Another example: performing output operations in filters might trigger filter calls even when the underlying data hasn't changed. (Since rendering functions are component-level, any component data modification responsively triggers the rendering function)

Failing to reconsider templates and modify existing coding styles could lead to unexpected errors.

These two significant changes—unidirectional data flow (data downward, events upward) and rendering functions (Virtual DOM)—make me feel Vue2 is semantically moving closer to React. Perhaps framework differences will continue to diminish as they mutually learn from each other. This is positive, as frameworks ultimately solve problems, not claim territories. Yet, I can't help but wonder: what will be the next front-end feature or design that leads the trend, following Virtual DOM?

