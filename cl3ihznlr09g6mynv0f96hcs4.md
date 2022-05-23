## Why Svelte is Easy to Learn as a React Developer

Originally published on the Offerzen Blog [here](https://www.offerzen.com/blog/why-svelte-is-easy-to-learn-as-a-react-developer). 

Trying new frameworks is always a great learning experience. As a React developer, I used Svelte for a project and was really impressed by its out of the box features and the developer experience it provided. Here’s why Svelte is a great framework to play with.

![Screen with search bar and Svelte and React logos](https://offerzen.ghost.io/content/images/2022/05/OfferZen_-Bhekani-Khumalo_How-I-built-pro-search-with-SvelteKit_Inner-article.png)

I started noticing a lot of tweets about using [Google’s advanced search parameters](https://ahrefs.com/blog/google-advanced-search-operators/) for better results. I realised that most people don’t know about them and if they do, it’s not that intuitive to use them with minimal friction.

That’s why I decided to make [Pro-Search](https://www.pro-search.io), a UX-focussed, advanced search engine query builder. Pro-Search helps you find exactly what you’re looking for by making it easy to add advanced search filters to different search engines.

![Pro-Search front page with search options](https://offerzen.ghost.io/content/images/2022/05/Screenshot-2022-05-20-at-09.53.44.png)

Choosing the right technology for Pro-Search
--------------------------------------------

To create Pro-Search, I ended up choosing [SvelteKit](https://kit.svelte.dev/), a framework that is to Svelte what NextJS is to React. It’s an open-source web development framework that enables functionalities such as server-side rendering and static site generation.

At the time, I had never built anything with Svelte and I barely knew anything about it, so this was a bit of an odd choice for me as a React developer. Why did I use Svelte?

**When choosing technologies, my top priorities were for it to:**

*   **Load fast:** It had to load as fast as, or faster, than the most popular search engines (Google, DuckDuckGo, Bing, Yahoo).
*   **Be simple:** A search should be as simple and easy as on the search engine. Additionally, the building of those simple and intuitive UIs should be simple and frictionless as well.
*   **Interactivity:** The tech I chose should handle heavy interactivity easily.
*   **Responsiveness:** It was important that I’d be able to create a progressive web app, and optimise for both web and mobile easily.

My goal was to create a great user interface while having an enjoyable developer experience, and Svelte delivers on both of these.

> I chose Svelte because it’s fast, ships small bundles and provides many great features while staying very close to vanilla HTML, CSS and JS.

All these are possible because you don’t ship Svelte with a Svelte app: it’s a “[framework without the framework](https://svelte.dev/blog/frameworks-without-the-framework)”. By statically analysing the component template ahead of time, Svelte ensures that the browser does as little work as possible.

_Disclaimer: I will make many comparisons with React. I am not picking on React or saying Svelte is fundamentally better than React. React is the framework I know best so it’s easiest for me to draw comparisons from there._

What is Svelte?
---------------

Svelte is a frontend compiler for building interactive user interfaces similar to React and Vue. With most current popular frameworks like React and Vue, the browser must do extra work to convert declarative component structures into DOM operations at run time. This eats into the frame budget and taxes the garbage collector.

Svelte compiles your code into imperative client-side vanilla JavaScript that manipulates the DOM directly. This is done during a compile step that happens at build time. This means it can strip out the parts of the framework that your app doesn’t use, resulting in a smaller bundle, fast first load and improved performance. Importantly, one of Svelte’s selling points is that it avoids the overhead associated with runtime intermediate representations, such as the virtual DOM.

As a tool, Svelte is powerful, flexible and beautifully designed. Its compiler approach unlocks a broad range of capabilities making it ripe for further expansion and growth. Making things with Svelte feels fun and streamlined.

Let’s walk through the developer experience and tooling and features.

Developer experience
--------------------

### Easy to learn

Svelte feels natural if you’re used to HTML, CSS and JavaScript. This makes it easy to collaborate with people who have a basic understanding of CSS and HTML. If you know HTML and CSS, you can write Svelte immediately. You can then learn more as you go, layering new concepts on top of that.

Sure there are still many things that aren’t vanilla HTML, but they’re quite easily [guessable](https://johno.com/guessable) once you see it done a few times. Being close to regular ‘ole HTML also means it is a much easier onramp into using UI frameworks compared to React.

When it comes to docs, Svelte sets a very high bar for an unfunded project without people working full-time on it. Here’s some of what Svelte provides in terms of docs:

*   [API docs](https://svelte.dev/docs)
*   [Tutorial](https://svelte.dev/tutorial/basics)
*   [Examples](https://svelte.dev/examples/hello-world)

I went through the Svelte tutorial in less than a day. It takes you through the concepts, incrementally increasing complexity and at each step and explaining why each thing exists. I found that this structure made it easier to stick with it. It also helps that there’s a [REPL](https://svelte.dev/repl/2c06d586d890466e8dc7da6fb111efb7?version=3.47.0) that’s structured to encourage you to keep trying what you’re learning. By the time you finish reading the tutorial, you’ve already written quite a significant amount of Svelte, and you’re familiar with the concepts and how it looks.

### Everything out of the box

One of the first things I noticed with Svelte, especially coming from React, is that it comes with many essentials included out of the box. While React has tons of external libraries to extend it, it’s nice to work with a framework where I don’t have to be making decisions at every turn about what to use.

To be fair to React, here are some things React has first-party that Svelte relies on external libraries for:

*   [DevTools](https://github.com/RedHatter/svelte-devtools)
*   [Portals](https://github.com/romkor/svelte-portal)
*   [Suspense](https://github.com/sveltejs/svelte/issues/3203)
*   [Mobile/Native](https://github.com/halfnelson/svelte-native)

### Abstracting the pain

Svelte has some nice syntactic sugar that makes the developer experience more pleasant. It clearly learnt a lot from its predecessors by abstracting away some of the pain.

For example, forms are a pain in React. That’s why libraries like [Formik](https://formik.org/) and [React Hook Form](https://react-hook-form.com/) exist. A form in React would look something like this:

    import { useState } from 'react'
      const Form = () => {
      const [name, setName] = useState('')
      const handleChange = e => setName(e.target.value)
      const handleSubmit = e => {
        e.preventDefault()
        // some stuff here
      }
      return (
        <form onSubmit={handleSubmit}>
          <input type='text' value={name} onChange={handleChange} />
        </form>
      )
    }
    

The same component in Svelte is:

    <script>
      let name = ''
      const handleSubmit = () => {
        // some stuff here
      }
    </script>
    <form on:submit|preventDefault={handleSubmit}>
      <input type="text" bind:value />
    </form>
    

The Svelte helpers and two-way binding are really handy because they make the code more terse and easy to follow.

### State management

Pro-Search is basically many different UI elements that are used to manipulate the global state, and the Svelte stores made implementing this very easy. Given how much global state management I needed, using Svelte stores was much simpler compared to having to use React’s context, redux and mobx.

Stores are JavaScript objects that adhere to a simple interface. With Svelte, a store is basically anything that has a subscribe method that returns a function, thereby allowing you to unsubscribe. Stores allow you to access data or functionality from multiple components. They’ll update your components whenever their data changes - hence that handy subscribe method.

> Svelte stores allow for separation of the state management from the component hierarchy. This is important when the same piece of data is needed in more than one place.

This is how you create a Svelte store:

    Import { writable } from 'svelte/store'
    export const store = writable(0) 
    

This is how you read and write to a Svelte store:

    <script>
      import { store } from './store'
    </script>
    <button on:click={() => $store + 1}>{$store}</button>
    

#### **Reactivity in the language**

Svelte uses mutable variables for local states, and they’re reactive out of the box. This makes for leaner code. Here’s a Svelte counter component:

    <script>let i = 0</script>
    <button on:click={() => i += 1}>count: {i}</button>
    

For comparison, here’s the same thing in React:

    import { useState } from 'react'
    const Counter = () => {
      const [i, setI] = useState(0)
      return <button onClick={() => set(i + 1)}>count: {i}</button>
    }
    

Svelte moves reactivity into the language by just using the language. Updating a value, and all the things that depend on it, is achieved simply by using the assignment operator (=).

    <script>
      let count = 0;
      $: double = count * 2;
    </script>
    <button on:click={() => count += 1}>Double: {double}</button>
    

Since Svelte is a compiler, reactivity is achieved by just instrumenting the code behind the scenes:

    count += 1; $$invalidate('count', count)
    

As you can see, this looks a lot like a regular HTML file. This, however, is also clever enough to know that whenever the value of the count variable changes, all the values that reference it will change as well.

### Bad experience with testing

On the other hand, I haven’t yet found a good way to test Pro-Search. Given my history with React, I first reached for [Jest](https://jestjs.io/) and [Svelte Testing Library](https://testing-library.com/docs/svelte-testing-library/intro/). I spent several hours trying to debug with no success.

Then, I remembered that Cypress recently launched their component testing library. Again, I spent several hours trying to set it up and got many different errors until I gave up. After that, I was too frustrated but didn’t want to kill my momentum so I parked the testing work.

However, I recently [tweeted](https://twitter.com/bhekanik/status/1511442051841331205?s=20&t=tmO84P7vi0i4ddDk8y7byA) to ask the Svelte community what people are using and within minutes I had several responses with multiple resources. I’m excited to try these out and see if I have better luck.

Svelte Tooling and features
---------------------------

### Static Scoped Styling

Svelte comes with scoped CSS without using CSS-in-JS solutions: It’s just CSS, no bells and whistles. Include a style tag in the component, write CSS and it will be scoped to the component by default: No configuration, just normal CSS. Of course, you can add the bells and whistles if you want to.

I have particularly loved working with Tailwind CSS, so this was the natural choice for me. Also: the configuration was a breeze.

### Transitions and Animations

Before Svelte, I had never really worked with animations and transitions. It’s always been something on my to-do list, but I’ve never dedicated the time for it because it seems like a complex topic. Fortunately with Svelte, I didn’t have to think about much: Straight out of the box, I just slapped on some pre-baked functions and it worked.

### Small bundle, faster apps

One of Svelte’s biggest strengths is its minimal JavaScript bundle sizes. This makes it ideal for any code that will be executed by low-power devices that can’t parse JavaScript quickly: For example, smart TVs, watches, and other IoT devices, or lower spec smartphones. It also means Svelte shines where bandwidth is limited, which again makes it a perfect fit for older devices and users who might have poor internet connections.

Because Svelte is a compiler, you always get the minimum subset of features required by your app. For example, the [Svelte implementation of the TodoMVC](https://svelte-todomvc.surge.sh/) weighs 3.6kb zipped. For comparison, React plus ReactDOM without any app code weighs about 45kb zipped. Even if your app is only made of one component, you still have to serve the 45kb that is React itself.

### Interoperability

When you want to use a specific npm library, you can only do so if you’re already using the framework that the library was designed for. If the library was built with React and you’re using Angular, then tough luck.

However, if the library was written with Svelte, apps that use it can be built using whatever JavaScript technology you like. This is because the Svelte app is just JavaScript at the end of the day.

This means that while the ecosystem of dedicated Svelte packages and plugins is comparatively small, you can still easily use any framework-agnostic package. I personally haven’t struggled to find what I’m looking for in terms of external libraries.

### Young library ecosystem

While on the subject of external libraries, Svelte options are fewer in number and generally less mature. Depending on your project and team, this could be irrelevant, inconvenient, or a blocker. I have personally struggled with getting a component library that gives me everything I need.

If I was using React, I’d be spoilt for choice because there are big players like [Material UI](https://mui.com/) or [Chakra UI](https://chakra-ui.com/). Sure, there’s [Svelte MUI](https://sveltematerialui.com/) but it’s nothing close to its React counterpart. That said, [daisyUI](https://daisyui.com/) has been really great and provided most of the things I need.

Apart from the UI components, which I have the escape hatch of building myself, this is not a big issue as I’ve been building Pro-Search to be as light as I can possibly make it. When you do need to build something yourself, Svelte makes it comparatively simple to do so.

Best practices are still evolving
---------------------------------

Svelte conventions and cultural knowledge like patterns, anti-patterns, best practices and consensus libraries are sparse and under-developed. People who adopt it today will have to think through problems that other communities have common solutions for, making the learning curve temporarily steeper.

While React forms are a nuisance, there are some established patterns to ease the pain even without using libraries. We all know of Kent C. Dodds’ popular approach on [How to use React Context effectively](https://kentcdodds.com/blog/how-to-use-react-context-effectively), and you see it popping up in many libraries.

These patterns and practices don’t exist with Svelte. For example, I couldn’t find a best practice to use a Svelte store, so I created my own way. There’s also no recommended way to structure components, so I used what I’m used to in React.

This, however, is obviously a temporary problem and will disappear as adoption increases.

Reflections on using Svelte
---------------------------

After using Svelte for more than six months on Pro-Search, I can say it’s an enjoyable developer experience that doesn’t compromise on user experience. I was able to pick it up and get going in building a relatively complex application in no time. Sure, there are things that are still in the works, but the API is very intuitive and the knowledge I already had from React was quite transferable.

When using Svelte, I always feel like I’m just writing regular HTML, CSS and JavaScript, and whatever powers are given to it by the framework are done under the hood. I really think this is how writing for the modern frontend web should feel. I’m excited for the future of Svelte, and I’m excited to be joining the community so early. Hopefully this is the start of many years of blissful coding!

Should you use Svelte right now?
--------------------------------

Well, it depends.

Are you learning your first framework? If so, what’s your goal? If you’re looking for an easy tool to learn, for something that will empower you to build interactive UIs, or just for some additional knowledge and experience, I’d recommend Svelte wholeheartedly. There’s no wrong choice when it comes to developing your own skills, and Svelte is easy for beginners to learn quickly.

On the other hand, if you’re learning in the hopes of getting a job with your newfound skills, it would be hard to recommend anything other than React because it’s the largest framework by far in terms of market share and jobs available. The industry went in heavily on React, and I don’t see that changing any time soon. There aren’t many employment opportunities for Svelte right now. Because it’s new, it’s not easy to get developers who already know it or are interested in learning it. Though, if you already know React, I think it’s worth learning Svelte for the perspective if nothing else.

Alternately, are you choosing a technology for a relatively new startup or project like me? Svelte will likely enable you to move quicker and build something that’s more performant, but hiring or collaborating may be a challenge given the relatively small pool of Svelte developers. Where React, Vue and Angular all have large communities full of resources for you to take advantage of, Svelte can seem comparatively small. The fear, then, is that when or if you need to reach for additional packages to handle things beyond the main framework’s capabilities, you may be left in the rut.

That said, knowing one JavaScript framework tends to make learning a new one easier and Svelte is, in my opinion, the easiest to start with. Any developer who’s already familiar with another frontend framework should be able to pick up Svelte pretty quickly, and vice versa.

The future of Svelte
--------------------

In the State of JS 2021 survey, [Svelte had a 90% satisfaction rating,](https://2021.stateofjs.com/en-US/libraries/front-end-frameworks) coming a close second to Solid. It also took the top spot as the [most loved framework](https://insights.stackoverflow.com/survey/2021#section-most-loved-dreaded-and-wanted-web-frameworks) in the 2021 Stackoverflow Developer Survey. All public survey data is going to have biases, but roughly speaking these are great numbers for a small time framework.

> Compared to modern mainstream frameworks like React and Vue and their communities, Svelte is smaller and less mature, but its community is active and established. As a result, Svelte is now commonly mentioned in the same breath as React, Vue, Angular, Preact, and Ember.

Unlike React, which has benefited from nearly a decade of optimisation already and may not have a lot left to squeeze out of it at this point, Svelte is comparably young. If I’m betting on which framework will improve more in the future relative to its current position, I’m backing Svelte no question. And, the bonus to being an early-adopter is that you become one of the initial groups of community contributors.

It’s worth mentioning that since Svelte started becoming popular, other frameworks have learned from it — leading to some synergies in the communities.

The arguments against adopting Svelte grow fewer everyday.

The end of 2021 came with some exciting news: Svelte and [Vercel](https://vercel.com/) announced that Svelte creator [Rich Harris will join Vercel to work on Svelte full-time](https://vercel.com/blog/vercel-welcomes-rich-harris-creator-of-svelte). This should accelerate the development of features, and hopefully push Svelte further into the mainstream.

I hope I’ve made you as excited about Svelte as I am! I encourage you to try it out on your own, even if it’s just playing around in the Svelte tutorial.

If you have any questions or thoughts, [reach out](https://twitter.com/bhekanik)!

### Resources

If you’re new to Svelte, these three presentations by Rich Harris, the creator of Svelte, are a popular way to get a pretty comprehensive introduction of what it is, why it exists, and what it wants to achieve.

*   [Computer, build me an app](https://www.youtube.com/watch?v=qqt6YxAZoOc)
*   [Rethinking Reactivity](https://www.youtube.com/watch?v=AdNJ3fydeao)
*   [The Return of Write Less, Do More](https://www.youtube.com/watch?v=BzX4aTRPzno)
