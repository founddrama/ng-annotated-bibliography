# AngularJS: an annotated bibliography

Wherein I gather up notes about AngularJS philosophy, mechanics, quirks, and more, and cite each item. The _if I were writing a book on this_ version (which I'm not (yet?)). More a journal or personal wiki than anything else. YMMV.

#### Contents

1. [The Angular Way](#ng-way)
2. [Core Concepts & Conventions](#core-concepts)
3. [A Trivial Example](#trivial-example)
4. [Controllers & `$scope`](#controllers-scope)
5. [Services](#services)
6. [Filters](#filters)
7. [Directives](#directives)
8. [Routes](#routes)
9. [Other Bits](#misc-other)
10. [AngularJS Conventions](#ng-conventions)
11. [Testing AngularJS Applications](#testing)
12. [Deploying an Application](#deploying)
13. [AngularJS Quirks](#quirks)
14. [Going Further](#going-further)
15. [Appendix A: Comparisons](#appendix-a)
16. [Appendix B: Further Reading](#appendix-b)

## Part I: Introduction & Philosophy

### [1. The Angular Way](id:ng-way)

Introductory material:

- what is Angular? (tell the story)
- what are its advantages and strengths?
- what is the underlying philosophy?

#### What is Angular?
- MV*? [MVW][38]!
- c/w Polymer
  - "like preview of the future" (web components?)
  - [Mosher, 2013][7]; [Rauschmayer, 2013][8]
- declarative data bindings FTW
  - [Smith, 2012][12]
- it's not just a new-fangled `onclick`; it's a DSL for proto-web-components...
  - see also: [that Hacker News thread (2013)][10]
- ng is "lethally allergic to global state" (from [docs on Services][18])
  - enforcement of module patterns
  
##### "opinionated in the middle"
- opinionated about everything **but** the model?
  - sidebar: re: Backbone not being opinionated about the views ([Murphey, 2012][20])
    > My biggest complaint about Backbone is probably how unopinionated it is about
    > the view layer. Its focus seems to be entirely on the data layer, but the view
    > is still where we spend the vast majority of our time.
  - Knockout: not opinionated about controllers/view models? (Anyone say this?)
- and/but/maybe/also unopinionated about the view
  - rather: opinion is **don't do string templates**
- major points from my talk here:
  - model is a server concern (UI deals with it incidentally)
  - "bring your own views"
    - (declarative bindings are not enough to have opinions about views)
    - (am I mostly comparing these to things like ExtJS and Dojo?)
    - no special template syntax to learn (just HTML)
    - use all the tools you already have

#### Why Angular?
- separation of concerns
  - ...leads to idiomaticity
    - ...leads to clarity
      - ...leads to simplicity
- `$scope` : helps keep things out of the global scope
- great testing story

##### Angular vs...
- anything you can do w/ Angular, you can do with any other fwk
- so when you would you use it?
- ...as opposed to [fill in the blank library]?

#### The Angular Philosophy

- "lethally allergic to global state" (from [docs on Services][18])
  - _why is this important?_
- "hands off the DOM"
- extend HTML vocabulary
- (what else?)

### [2. Core Concepts & Conventions](id:core-concepts)

What are the cross-cutting concerns? What is the "meta-knowledge" within Angular that will help orient you to the rest of the framework?

#### Dependency Injection
- in services, controllers, directives
- "just function args"
- the magic!
  - _too much magic?_ - the `Function.toString` stuff ([Rothenberg, 2013][14])
  - e.g., services don't exist unless dependency-injected
- arguments that it's unnecessary ([Pretorius, 2013][15])
  - and/but : it matches the philosophy
- deep dive on how DI works w/r/t/ services ([Hooks, 2013][30])

#### Scope
- `$rootScope` and others
- prototypical inheritance
- events: `$emit` (up) and `$broadcast` (down)
  - see also: [Stack Overflow: How to unsubscribe to a broadcast event in angularJS. How to remove function registered via $on][19]
- in controllers, directives

##### the zen of $scope
- AngularJS docs: [Understanding Scopes][29]
- lifecycle ([Dorby, 2013][1])
- `$scope.$watch`
  - should be fast and idempotent ([Dobry, 2013][2])
- `$scope` as model and not-model ([Dobry, 2013][2])
  - `$scope` !== model; `$scope` === glue b/w Controller and View
- `$scope` is prototypal
- when to use `$apply`
- when to force calls to `$digest`
- `$on` (and events...)
  - whither `$off` - de-registering listeners "the Angular way" ([uh... this][19])
- "the surface area b/w your controller and your view"

### [3. A Trivial Example](id:trivial-example)

**The Gentle Introduction to AngularJS**

Quick-and-dirty "`ng-hello-world`" type app to introduce and highlight: _controllers_, _services_, _directives_, _filters_, and _routes_. "Just enough to be dangerous."

#### "`ng-hello-world`""
- code

#### AngularJS in a Nutshell
- re-stating philosophical position
- (real sound-bite stuff here)

#### AngularJS in 15 Minutes

##### the controller
- explanation of code

##### the routes
- explanation of code

#### AngularJS in an Hour

##### the service
- explanation of code

##### the filter
- explanation of code

#### AngularJS in an Afternoon

##### the directive
- explanation of code

## Part II: Angular Mechanics

### [4. Controllers & `$scope`](id:controllers-scope)
- philosophy of "thin" controllers
- more of a "ViewModel" ([Osmani, 2012][39])
- `$scope` is the "surface area" b/w your controller and your view

#### on models
- in the AngularJS world: models are a server concern
  - the client-side only incidentally deals with models
  - re: `$scope` just provides the "membrane" b/w view and controller
  - re: use filters to transform model data for display (_don't mutate it!_)
  - re: no "model infrastructure" built in to the fwk
    - b/w Ember.js, Backbone.js, Knockout.js (to a lesser extent)
    - i.e., no special utilities to update the model
- AngularJS is _not_ opinionated about models
  - but should be?
  - and/but: _none_ of the MV* frameworks are?
  - (some would hand-raise re: Ember.js here?)
  - ["... Angular has virtually no opinion when it comes to your Model layer."][1]
- `$scope` and `$rootScope` as model?
  - [Dobry][1]: "No" - "services own your data ... controllers and directives consume your data"
    - and/but - feel like I saw a thread somewhere that said _don't_ just use Services in this way
  - (who else...?)
- take a stand here? (offer an opinionated view on models)
- [Rob Conery on models][21]
  - "services are _not_ the model"
  - emphasizes keeping model concerns on the server
  - makes the case that ng-controllers are really view-models (heard that before?)  
    > if it's not presentation-related, it doesn't belong here.
  - gives the nod to filters here (let the model just be data; "this is what filters are _for_")
- re: using your back-end validation to power your front-end validation
  - AngularJS forms + ajax ([Dudulski, 2013][35])
  - (although dealing w/ `$`-prefixed properties is probably an AngularJS code smell)

### [5. Services](id:services)
- for ~~...?~~ _any code-sharing b/w components_
- philosophy:
  - for code sharing across other components
  - idempotence is key here
- singletons
- "just global objects" ([DeJong, 2013][23])
- "Combine broadcast messaging with services" ([DeJong, 2013][23])
- different ways of creating services ([Hooks, 2013][30])
  - `provider` vs.
  - `factory` vs.
  - `service`
- see also:
  - [AngularJS Docs: Creating Services][18]
  - ["Models and Services in Angular" (Conery, 2013)][21]


### [6. Filters](id:filters)
- "don't manipulate your data!" (filters as "strictly a view concern")
  - i.e., don't go changing your model data in services, controllers, directives
  - need to turn your gnarly *nix date string into something someone would read?
    - filters!
  - see also: [Conery, 2013][21]
- and/but *not* producing HTML

### [7. Directives](id:directives)
- spend time on each of the "constructor args" in the object you produce
- attrs as constructor/scope args
- the power is in the data binding; forget what you think you know about `onChange` events ([DeJong, 2013][23])
- [Hooks, 2013][26]: digging in to the built-in `A` directives in Angular (illuminating...)
- [Jan Varwig, 2013][28]:  
  > Directives are the core building block of an Angular app. Use them to insert whatever structure or behavior you need in your app. Views are merely a shortcut for very simple use-cases.
- "directives are never done" (and the ... problems this can cause) ([Mak, 2013][22])
  - short version (and I've ran into this myself) - if it's constantly doing shit b/c of `$digest`
  - ...then how do you ever know when to do something "non-Angular" with your Angular code?
  - (he talks about using `$timeout` and `scope.$apply` - and though the latter seems _OK_, the former decidedly _does not_, and neither seems _ideal_)
- references / see also:
  - ["Build Custom Directives with AngularJS"][4]
- ...vs. views ([Varwig, 2013][28])

#### restrict
- on sticking to `restrict:'A'`
  - when to use the others

#### scope
- AngularJS docs on scopes include _a lot_ of info re: scopes + directives ([source][29])
- `scope:false` (default) vs. `scope:true`
- `scope:true` vs. `scope:{}` (isolate scope)
- understanding isolate scope
- attributes as constructor args (attributes as scope args) (_vide supra_)
  - `@` vs. `=` vs. `&`

#### priority
- !?!?!?

#### templates
- `template` vs. `templateUrl`
- on `transclude`
- on `replace`

#### compile vs. link
- `compile` fn
  - return `postLink` _or_
  - return `{pre, post}`
- `link` fn
  - _as_ `postLink` _or_
  - return `{pre, post}`

#### controllers inside of directives
- !?!?

#### require
- !?!?

### [8. Routes](id:routes)
- !?!?

### [9. Other Bits](id:misc-other)

"Other Bits" may be a big enough topic to split up -- haven't spent enough time "down in there" to know for sure... And but includes:

- bootstrap and ng lifecycle
- understanding ng expressions
- modules
- views
- resources (???)
- promises and `$q`
- the ng wrappers (`$window`, `$document`, `$location`, `$timeout`, `$log`)
- errors

#### angular.element
- "jqlite"
  - ... vs. jQuery
  - ... *w/* jQuery
  - Zepto?
- ng's philosophy about touching the DOM

#### animation
- rolling your own?
- just don't?
- "edge" versions on ng?
- see also:
  - [Animation in AngularJS][11]
  - [Animating with AngularJS][24]
  - [Get Moving with Angular 1.2 Animation and Animate.css][32]

## Part III: Sophisticated Angular

### [10. AngularJS Conventions](id:ng-conventions)
Naming and organizing things; the conventions that you'd expect _would_ be part of the opinionated AngularJS eco-system.

- are there any?
  - ["AngularJS Best Practices" (Jaco Pretorius, 2013)][15]
    -  suggests that there is
  - and/but, it's at odds w/ what (_what???_)
- ["Building large apps with AngularJS" (Dobry, 2013)][1]
  - and its [accompanying presentation][2]

#### naming things
- !?!?!?

#### organizing things
- [angular-seed][16] suggests one and/but... that won't scale to large apps
- what about:
  - `controllers/`
  - `directives/`
  - `services/`
  - etc.
  - ...a variation on the same untenable problem?

### [11. Testing AngularJS Applications](id:testing)
- !?!?!?
- Jasmine
- Karma
  - as in: "they assume you're using it"
- unit
  - case study... ([Rothenberg, 2013][36])
- E2E
- need to use `angular-mocks.js`
  - to get it to work in the browser
- [Angular Test Patterns][37]

### [12. Deploying an Application](id:deploying)
- !?!?

### [13. AngularJS Quirks](id:quirks)
> "...difficulties, hitches, pitfalls, problems, and other realities."

**"Pain Points"**

- documentation
  - feels incomplete in many places
- testing
  - docs around unit testing practically non-existent
  - test-runner? (Karma, but...)
- complexity! & "where to begin?"
- references:
  - ["AngularJS Pain Points" (Pretorius, 2013)][3]
- rendering performance
- FOUC
- Elephant in the Room: Progressive Enhancement

### [14. Going Further](id:going-further)
"Advanced AngularJS"; working w/ advanced features; working with 3rd party libraries; and beyond.

#### Advanced AngularJS
- AngularJS + SVGs ([Rockwood, 2013][5])
- AOP in Angular ([Gechev, 2013][34])

#### working w/ 3rd party libs
- "It's just JS!" (the Tao of Globals)
- Dorby (again!) says to become one with:
  - `$scope.$apply()`
  - `$scope.$evalAsync`
  - `$q.when()`
  - `$timeout`
- jQuery (see "`angular.element`")
  - Zepto?
- Underscore?
  - Lo-Dash
- RequireJS?
  - AMD more generally?
- hand-rolled libs/utils?
- on `$watch` and `$digest` and `$apply`
  - Angular can call out but don't have 3rd call in
  - (best practices here?)
  - ([Dobry, 2013][2])
  - ([Rodriguez, 2013][33] - good intro)

#### lazy-loading
Expands on advice from "deploying"?

- not officially supported ([Dobry, 2013][1])
- would be great if it did!
  - Dorby article includes a (short!) summary of an existing proposal
- re: lazy loading images? ([Nadel, 2013][25])
  - (they're talking about 2 diff things I think)
  - maybe a separate section for "advanced shit?"

## Appendices

### [Appendix A: Comparisons](id:appendix-a)
(note: re-org into something earlier on? and/or is it not even worth discussing?)

- c/w: Knockout, Backbone; others?
- see also:
  - [Sanderson, 2012][27]
  - [Hartikainen, 2013][13]
  - [McKeachie, 2013][40]

### [Appendix B: Further Reading](id:appendix-b)

#### _AngularJS_
(Green, 2013)

  1. Introduction to...  
  2. Anatomy of...  
  3. Developing in...  
  4. Analyzing an...  
  5. Communicating with Servers  
  6. Directives  
  7. Other Concerns  
  8. Cheatsheet and Recipes  

#### _AngularJS in Action_
(Ford, 2013)

  1.  Introduction to...  
  2.  Hello Angular  
  3.  MVW: The Foundation  
  4.  Integrating with the Server  
  5.  Directives  
  6.  Animating with...  
  7.  Routes  
  8.  Forms and Validation  
  9.  Polishing  
  10. Testing  
  11. Deploying  
  12. Philosophies Behind...  
  13. Best Practices

#### AngularJS: From Beginner to Expert in ~~7~~ _6_ Steps
(Lerner, 2013)

  1. [How to Get Started](http://www.ng-newsletter.com/posts/beginner2expert-how_to_start.html)
  2. [Scopes](http://www.ng-newsletter.com/posts/beginner2expert-scopes.html)
  3. [Data Binding and Ajax](http://www.ng-newsletter.com/posts/beginner2expert-data-binding.html)
  4. [Directives](http://www.ng-newsletter.com/posts/beginner2expert-directives.html)
  5. [Services](http://www.ng-newsletter.com/posts/beginner2expert-services.html)
  6. [Next Steps (Routing and Filters)](http://www.ng-newsletter.com/posts/beginner2expert-config.html)

#### My Own Work

Blog posts, presentations, etc.

1. [Angular zen: directives and scope](http://blog.founddrama.net/2013/07/angular-zen-directives-and-scope/)
2. [early thoughts on AngularJS](http://blog.founddrama.net/2013/08/early-thoughts-on-angularjs/)
3. [Beyond `ng-hello-world`](http://founddrama.github.io/vt-code-camp-2013/)

----

[1]: http://pseudobry.com/building-large-apps-with-angularjs.html "Building large apps with AngularJS"
[2]: http://slid.es/jdobry/building-large-apps-with-angularjs "Building large apps with AngularJS (presentation)"
[3]: http://www.jacopretorius.net/2013/07/angularjs-pain-points.html "AngularJS Pain Points"
[4]: http://www.ng-newsletter.com/posts/directives.html "Build custom directives with AngularJS"
[5]: http://gaslight.co/blog/angular-backed-svgs "Angular Backed SVGs"
[6]: http://keminglabs.com/blog/angular-cljs-weather-app/ "Building an iOS weather app with Angular and ClojureScript"
[7]: http://blog.testdouble.com/posts/2013-06-26-what-polymer-and-angular-tell-us-about-the-future-success-of-the-web-platform-and-javascript-frameworks.html "what polymer and angular tell us about the future success of the web platform and javascript frameworks"
[8]: http://www.2ality.com/2013/05/google-polymer.html "Google’s Polymer and the future of web UI frameworks"
[9]: http://oscarvillarreal.com/2013/05/07/5-reasons-to-use-angularjs-in-the-corporate-app-world/ "5 reasons to use AngularJS in the corporate app world"
[10]: https://news.ycombinator.com/item?id=5526058 "We used to write things like... (HN)"
[11]: http://www.yearofmoo.com/2013/04/animation-in-angularjs.html "Animation in AngularJS"
[12]: http://thesmithfam.org/blog/2012/12/02/angularjs-is-too-humble-to-say-youre-doing-it-wrong/ "AngularJS is too humble to say you’re doing it wrong"
[13]: http://codeutopia.net/blog/2013/03/16/knockout-vs-backbone-vs-angular/ "Knockout vs Backbone vs Angular"
[14]: http://www.alexrothenberg.com/2013/02/11/the-magic-behind-angularjs-dependency-injection.html "The \"Magic\" behind AngularJS Dependency Injection"
[15]: http://www.jacopretorius.net/2013/07/angularjs-best-practices.html "AngularJS Best Practices"
[16]: https://github.com/angular/angular-seed "angular-seed"
[17]: https://coderwall.com/p/y0zkiw "Building large apps with AngularJS (on Coderwall)"
[18]: http://docs.angularjs.org/guide/dev_guide.services.creating_services "AngularJS Docs: Creating Services"
[19]: http://stackoverflow.com/questions/14898296/how-to-unsubscribe-to-a-broadcast-event-in-angularjs-how-to-remove-function-reg "How to unsubscribe to a broadcast event in angularJS. How to remove function registered via $on"
[20]: http://rmurphey.com/blog/2012/03/11/thoughts-on-a-very-small-project-with-backbone-and-backbone-boilerplate/ "Thoughts on a (Very) Small Project With Backbone and Backbone Boilerplate"
[21]: http://wekeroad.com/2013/04/25/models-and-services-in-angular "Models and Services in Angular"
[22]: http://branchandbound.net/blog/web/2013/08/some-angularjs-pitfalls/ "Some AngularJS pitfalls"
[23]: http://www.objectpartners.com/2013/08/09/i-wish-i-knew-then-what-i-know-now-life-with-angularjs/ "I Wish I Knew Then What I Know Now — Life With AngularJS"
[24]: http://flippinawesome.org/2013/08/05/animating-with-angularjs/ "Animating with AngularJS"
[25]: http://www.bennadel.com/blog/2498-Lazy-Loading-Image-With-AngularJS.htm "Lazy Loading Image With AngularJS"
[26]: http://joelhooks.com/blog/2013/07/15/a-look-at-angularjs-internal-directives-that-override-standard-html-tags/ "AngularJS Directives That Override Standard HTML Tags"
[27]: http://blog.stevensanderson.com/2012/08/01/rich-javascript-applications-the-seven-frameworks-throne-of-js-2012/ "Rich JavaScript Applications – the Seven Frameworks (Throne of JS, 2012)"
[28]: http://jan.varwig.org/archive/angularjs-views-vs-directives "AngularJS: Views vs. Directives"
[29]: https://github.com/angular/angular.js/wiki/Understanding-Scopes "Understanding Scopes"
[30]: http://joelhooks.com/blog/2013/08/18/configuring-dependency-injection-in-angularjs/ "Configuring Dependency Injection in AngularJS"
[31]: http://www.yearofmoo.com/2012/10/more-angularjs-magic-to-supercharge-your-webapp.html#internationalization-and-localization "More AngularJS Magic to Supercharge Your Webapp (i18n)"
[32]: http://www.divshot.com/blog/tips-and-tricks/angular-1-2-and-animate-css/ "Get Moving with Angular 1.2 Animation and Animate.css"
[33]: http://angular-tips.com/blog/2013/08/watch-how-the-apply-runs-a-digest/ "$watch How the $apply Runs a $digest"
[34]: http://blog.mgechev.com/2013/08/07/aspect-oriented-programming-with-javascript-angularjs/ "Aspect-Oriented Programming with AngularJS"
[35]: http://codetunes.com/2013/server-form-validation-with-angular/ "Server form validation with Angular.js"
[36]: http://www.alexrothenberg.com/2013/08/06/how-to-unit-test-an-angular-app.html "How To Unit Test An Angular App."
[37]: https://github.com/daniellmb/angular-test-patterns "Angular Test Patterns"
[38]: https://plus.google.com/+AngularJS/posts/aZNVhj355G2 "Igor Minar re: MVW"
[39]: http://addyosmani.com/resources/essentialjsdesignpatterns/book/#detailmvvm "Learning JavaScript Design Patterns: MVVM"
[40]: http://www.funnyant.com/choosing-javascript-mvc-framework/ "Size Does Matter: Choosing a Javascript MVC Framework"