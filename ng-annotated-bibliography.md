# AngularJS: an annotated bibliography

### Angular JS Google Group: "service to manage the configuration of the app"
[[46][46]] \#service \#remote-configuration

The nutshell version of the question: **If my server-side app provides constants
and/or other data to configure aspects of my Angular application, how can I
bring those in at runtime?** Though it isn't explicitly said, the original
question ("Can I use `$http` during `factory` or in `$provider.get` to get these
data?") turns out to be more/less impossible; instantiation of the service has
no mechanism for being deferred. Instead, one user recommends: 

> You would prefer a json file when it is data that is shared between different
> sides like validation rules for server and client or data that is generated at
> build time. Then I would try to integrate the json file in the build process
> so I get a constant definition in angular.

Example:

```
angular.module('config').constant('generic', <%= json %>);
```

That one of Angular's service creator functions does _not_ take a promise (i.e.,
as opposed to "only" a function) seems like a missed opportunity, although there
are also some significant challenges there as well (e.g., what if the promise is
rejected?)

### Angular Team: `angular-seed`
[[16][16]] \#boilerplate

> This project is an application skeleton for a typical AngularJS web app. You
> can use it to quickly bootstrap your angular webapp projects and dev
> environment for these projects.

Arguments cane be made that this will work for small projects but maybe not for
large ones. Good source material though.

### Angular Team: "Dev Guide: Creating Services"
[[18][18]] \#service

Get familiar with the source material, esp. w/r/t/ `$inject`.

> [Read more](http://docs.angularjs.org/guide/di) about dependency injection
> (DI) in Angular and the use of array notation and the $inject property to make
> DI annotation minification-proof.

### Angular Team: "Wiki: Understanding Scopes"
[[29][29]] \#scope

In-depth discussion of prototypal inheritance in `$scope`. (More information in
the wiki than in the developer guide and/or API docs?)

### Bleigh: "Get Moving with Angular 1.2 Animation and Animate.css" (2013)
[[32][32]] \#animation

Michael Bleigh with introductory coverage of animation in AngularJS 1.2.x.

### Burleson: "Dependency Injection using RequireJS & AngularJS"
[[48][48]] \#dependency-injection

An overview of dependency injection, a comparison of what that means in
RequireJS vs. what it means in AngularJS, and how to use RequireJS underneath
AngularJS to handle AMD. It's worth noting that an AMD style loader seems like
something that will eventually ship as part of AngularJS.

Consider it background reading for [Burleson's post on decorators][45].

### Burleson: "Enhancing AngularJS Logging using Decorators"
[[45][45]] \#service \#decorator \#aop

Though he doesn't cite something like [Gechev's methods](http://blog.mgechev.com/2013/08/07/aspect-oriented-programming-with-javascript-angularjs/),
Thomas describes using `$provide.decorator()` as the built-into-Angular solution
for AOP. It's a thorough example of how using the `decorator` function to modify
services in Angular. That being said, I have mixed feelings about the slightly
more complicated (`$log.getInstance`?) version of `$log` that he ends up with.
(As an aside: his use of RequireJS makes this not a "pure Angular" post and
diminishes some of the value; although I can see _why_ he went this route, I
think it adds some unnecessary overhead w/r/t/ illustrating the specific points
about `decorator`.)

### Conery: "Models and Services in Angular" (2013)
[[21][21]] \#model \#service

Rob Conery:

> ...if it's not presentation-related, it doesn't belong here.

He makes the point that services in Angular are not models, though they should
provide the API that your controllers etc. use to deal with the model data. He
also points out that you may be tempted to overload your models with a bunch of
behaviors. And in a nutshell he says: "Don't. That's what filters are for."

### DeJong: "I Wish I Knew Then What I Know Now — Life With AngularJS" (2013)
[[23][23]] \#quirks

Jon DeJong re: data binding; services as singletones; `$broadcast`; `ng-view`;
and external data manipulation.

### Dobry: "Building large apps with AngularJS" (2013)
[[1][1], [2][2], [17][17]] \#architecture \#single-page-apps

Outstanding post by Jason Dobry about using AngularJS to do non-trivial UIs and
about some of the latent challenges that you'll encounter when doing so. He's
got quite a few interesting things to say, in particular about the common
misperception that `$scope` is the model, some points about how to organize your
files, and about integrating Angular with other libraries. It's one of the best
posts I've encountered on Angular so far, and he does an excellent job
explaining some of the more ambiguous things.

Also: the accompanying slide deck is pretty good, too.

### Dudulski: "Server form validation with Angular.js" (2013)
[[35][35]]

Jan Dudulski, writing at the Codetunes blog. By and large, I think if you're
dealing with `$`-prefixed properties directly, it's probably an AngularJS code
smell, but this technique looks pretty smart, and I think we can trust `$dirty`
and `$invalid` to be pretty stable features.

### Fletcher: "Decoupling from the DOM with Angular" (2013)
[[44][44]] \#controller \#directive \#separation-of-concerns

Rob Fletcher:

> Managing that kind of separation of concerns is the promise of Angular in a
> nutshell.

A good case study in best practices with controllers and directives and how to
think about separation of concerns in AngularJS. This is a particularly
interesting post because there's a strong temptation when developing directives
to "put it all in there" -- that once you've gotten to the point of creating the
directive, you're not really thinking about controllers anymore. But Fletcher
puts it well:

> Remember controllers are for managing view _state_ and directives are for
> managing the view _implementation_. If you find yourself mixing those concerns
> step back and think about how you can separate them.

### Fox: "Server-side HTML vs. JS Widgets vs. Single-Page Web Apps" (2013)
[[42][42]] \#architecture \#single-page-apps

Pamela Fox gives an overview of some of the server- vs. client-side UI decisions
(putting it in context at Coursera). She gets in to some of the trade-offs and
makes cases for each one having its place.

Not specific to AngularJS, but germane w/r/t/ architecture decisions.

### Gechev: "Aspect-Oriented Programming with AngularJS" (2013)
[[34][34]] \#aop

Minko Gechev presents a technique for AOP in AngularJS.

### Hacker News: "We used to write things like..." (2013)
[[10][10]] \#template-as-dsl \#templates \#criticism \#progressive-enhancement

**tl;dr:** It looks like `onclick` all over again, and/but ng templates are
really just a DSL for producing single-page apps? And/but: progressive
enhancement is the elephant in the room here.

I promise I won't make a habit of linking to Hacker News, but this was timely
because this was something that I found myself thinking about while hacking
together the [Project Management Dev Dice](http://dev-dice.herokuapp.com/).
"What makes `ng-click` any different from `onclick`?" There's the usual HN
signal/noise to deal with here, but most of the discussion is worthwhile. For my
money, where I landed on this was as follows: what makes `ng-click` and its
brethren different than the old-fashioned tight-coupling of `onclick` etc. is
that the the Angular views are basically hybridizing mark-up and JavaScript
anyway — (a DSL?) — the lines are blurry. (And isn't Angular's philosophy to be
declarative? to add something to the mark-up that it "should" have?) Now if I
can just reconcile the whole "not progressively enhanced" bit that goes
hand-in-hand with Angular...

### Hartikainen: "Knockout vs Backbone vs Angular" (2013)
[[13][13]] \#comparison

Jani Hartikainen (CodeUtopia) doing a side-by-side-by-side comparison of these
three popular libraries. In his opinion, none of the three is a clear winner
over the others, but he does a good job of illustrating the pros and cons of
each.

### Hooks: "Configuring Dependency Injection in AngularJS" (2013)
[[30][30]] \#dependency-injection \#service

Overview of services. Defining via `module.service` vs. `module.factory` vs.
`module.provider`. (Mentions but does not cover `value` and `constant`
dependencies.)

- `service` calls `factory`
  -  and/but uses `new` on constructor fn via `$injector.instantiate`
- `factory` calls `provider`
  - _but_ no `new`, and expects your "constructor" to return an object
- `provider` makes internal use of `$get`
  - maximum flexibility for maximum complexity

```
function provider(name, provider_) {
    if (isFunction(provider_) || isArray(provider_)) {
      provider_ = providerInjector.instantiate(provider_);
    }
    if (!provider_.$get) {
      throw $injectorMinErr('pget', "Provider '{0}' must define $get factory method.", name);
    }
    return providerCache[name + providerSuffix] = provider_;
  }

  function factory(name, factoryFn) { return provider(name, { $get: factoryFn }); }

  function service(name, constructor) {
    return factory(name, ['$injector', function($injector) {
      return $injector.instantiate(constructor);
    }]);
  }
```

### Hooks: "Why I Built an AngularJS Training Site on Rails" (2013)
[[41][41]]

Joel Hooks:

> Aside from basic nerd compulsion to explore cool technology, it became rapidly
> apparent that this was the right tool for the job.

A good reminder that, for as much as you may love a particular technology or
framework, use the rights ones in the right places for the right things. (A
variation on "don't put all your eggs in one basket"?)

### Hooks: "AngularJS Directives That Override Standard HTML Tags" (2013)
[[26][26]] \#directive

Joel Hooks: deep dive into some of the built-in AngularJS directives, focusing
on how they supplement (override) the native elements' behaviors.

### Lamb: "Angular Test Patterns"
[[37][37]] \#testing

Daniel Lamb's Git repository of useful AngularJS test patterns. A good place to
start some research into the testing, but still seemed to be missing some
good/clear foundational ("simple") examples.

### Lerner: "Build custom directives with AngularJS" (2013)
[[4][4]] \#directive

Ari Lerner. Long-ish expository post in the ng-newsletter about creating and
working with Angular directives. Directives are a really powerful part of
Angular but can be a bit daunting to newcomers ("Scopes? Restrictions? Linking
functions!?") but do yourself a favor: have the patience to learn the
fundamentals of directives. That will pay dividends.

### Lynagh: "Building an iOS weather app with Angular and ClojureScript" (2013)
[[6][6]] \#clojurescript \#ios

Kevin J. Lynagh on building the Weathertron app:

> ...do the semantics of the library complement your application design? Or
> will build a Dr. Jekel / Mr. Hyde abomination of ClojureScript code doomed to
> emit painful, prototype-twiddling, mutation-happy JavaScript?

Some cool solutions they came up with for some interesting problems.

### Mak: "Some AngularJS pitfalls" (2013)
[[22][22]] \#quirks

Sander Mak: FOUC; jQuery + AngularJS; Minification; and directives.

### McKeachie "Size Does Matter: Choosing a Javascript MVC" (2013)
[[40][40]]

Craig McKeachie doing some side-by-side, feature-for-feature comparisons of a
couple JavaScript MV* frameworks. His emphasis is mostly on AngularJS,
Backbone.js, Ember, and Knockout, but a couple of other libraries make cameos
as well. The title is a little misleading because he's talking less about the
size of each framework and more about how "rich" each one is. He lays out some
good points w/r/t/ things you should consider when choosing such a framework
(and/or whether you should choose one at all).

### Minar: "re: MVW"
[[38][38]]

Igor Minar re: "Model-View-Whatever"; **tl;dr:** "Don't argue over the
nomenclature; go forth and build cool apps."

### Mosher: "what polymer and angular tell us about the future success of the web platform and javascript frameworks" (2013)
[[7][7], [8][8]] \#web-components

David Mosher, writing about Polymer and Angular, and Web Components in a more
general way. In a nutshell he argues (as Yehuda Katz does) that
library/framework designers serve the community better by focusing on the
low-level APIs and letting the best ideas percolate up until they become part of
the standards. I agree with this point — you can look at how this has worked out
with Backbone (as he mentions) or go further back to jQuery and it becomes clear
how these feedback loops are tremendous success stories. On the other hand: I
wish people would stop dumping on Ember.js. There's a place for higher-level
frameworks like it, too.

### Murphey: "Thoughts on a (Very) Small Project With Backbone and Backbone Boilerplate" (2012)
[[20][20]] \#backbone \#comparison

On views, templates, and being opinionated.

> My biggest complaint about Backbone is probably how unopinionated it is about
> the view layer.

Not specific to AngularJS, but very good.

### Nadel: "Lazy Loading Image With AngularJS" (2013)
[[25][25]] \#lazy-loading

Case study is some advanced AngularJS application work around lazy loading with
a custom directive.

### Osmani: _Learning JavaScript Design Patterns_ (MVVM)
[[39][39]] \#MVVM

Addy Osmani's detailed description of what separates MVVM from MVC.

### Pretorius: "AngularJS Pain Points" (2013)
[[3][3]]

A write-up by Jaco Pretorius. I agree with him on the documentation, and I
(admittedly, sadly) don't have enough experience with the testing aspect to have
an opinion there, but I'd be willing to split hairs on the "overall complexity"
complaint.

Given my experience so far, I would say that the trivial things are quite
trivial, and though the non-trivial things take a bit of work, none of them have
blocked me for more than an hour or two. There's a bit of push/pull, but I've
also only been doing non-trivial stuff with it for a short time, and I feel like
each time I get stumped, I come out the other side with a much better
understanding of the framework. It's opinionated, and there's "an Angular way"
of doing things — but it's not always clear (at first) what that way is. And I
blame the documentation for that.

### Pretorius: "AngularJS Best Practices" (2013)
[[15][15]] \#best-practice

Notes/advice on directory structure and project organization; dependency
injection (skepticism); advice on hiding FOUC; separation of concerns.

### Rauschmayer: "Google’s Polymer and the future of web UI frameworks" (2013)
[[8][8]] \#web-components

Dr. Axel Rauschmayer on Google's Polymer:

> Nobody actually wants to use frameworks. We only want to build web user
> interfaces efficiently and frameworks help.

I largely agree with his points, especially the thrust of the above pull-quote.
When discussing frameworks and libraries like this, it's easy to lose sight of
the fact that they're around because of some gaping hole in the language's
native/standard libraries. Google's recent efforts (e.g., AngularJS and now
Polymer) point to more declarative or component-oriented approaches; put in the
context of HTML5 and the advancements there, that makes a lot of sense. I'm
interested in Polymer, but I feel like I need some more time to digest it. Like
when Twitter open-sourced Flight, there's this sense of: "Cool! That's a novel
approach..." Which also feels a bit like: "Whoa, that's really different." Now
to dig in...

### Rockwood: "Angular Backed SVGs" (2013)
[[5][5]] \#advanced \#svg

Kevin Rockwood, writing at the Gaslight.co blog -- with a rather nice epiphany:

> SVGs are just HTML!

i.e., Angular hooked into SVG "for free".

### Rodriguez: "$watch How the $apply Runs a $digest" (2013)
[[33][33]] \#$apply \#$digest

Jesus Rodriguez with a deep dive into the `$digest` cycle and how to master
`$watch` and `$apply`.

### Rodriguez: "Tip: Directives With the Same Name" (2013)
[[43][43]] \#directive

Jesus Rodriguez points out that AngularJS basically overloads directives,
effectively stacking them as an array of callables. This can make it easy to
"magically" add functionality (e.g., logging on top of the usual `ng-click`
behavior, as in his example), and though it may be "very intentional" in their
code, this kind of overloading seems potentially dangerous. Use with caution;
consider AOP instead?

### Rodriguez: "Understanding Service Types"
[[47][47]] \#service

Along the same lines as the overview of service types that [Hooks][30] gave; but
where Hooks went deeper on `factory` vs. `service` vs. `provider`, Rodriguez
goes broader and includes `constant` and `value` and `decorator`.

### Rothenberg: "The 'Magic' behind AngularJS Dependency Injection" (2013)
[[14][14]] \#dependency-injection \#minification \#quirks

Fascinating post by Alex Rothenberg wherein he digs deep into Angular.js to
determine exactly why minification broke his application.

### Rothenberg: "How To Unit Test An Angular App." (2013)
[[36][36]] \#testing

Alex Rothenberg with a TDD app dev case study and a look into how you can test
your AngularJS apps. Like many of the other AngularJS testing articles though,
it is a decent introduction but leaves out some information.

### Sanderson: "Rich JavaScript Applications – the Seven Frameworks (Throne of JS, 2012)" (2012)
[[27][27]] \#comparison

Steve Sanderson’s round-up of Throne of JS. Amazing post--I'm going to need to
read it two or three more times to take it all in. Many thanks to Steve for
putting the time and effort into a post like this.

### Schinsky: "Animating with AngularJS" (2013)
[[24][24]] \#animation

Holly Schinsky, writing at Flippin' Awesome with a good overview of the
animation directive that is new in Angular 1.1.4 and revised in version 1.1.5.
She covers animating with CSS3 transitions and introduces the naming conventions
that the directive expects but doesn't get into more advanced JavaScript-based
animations.

### Smith: "AngularJS is too humble to say you’re doing it wrong" (2013)
[[12][12]] \#introducing

Dave Smith's post on AngularJS is one of the best I've seen so far -- and mostly
because he is able to demonstrate so much of the library's power in such a short
and digestible post. (And/but if you want a post that's comparing several of
these MV* frameworks, you might want to check out [Steve Sanderson's "the Seven Frameworks" post][27].

### Varwig: "AngularJS: Views vs. Directives" (2013)
[[28][28]] \#view \#directive

Jan Varwig:

> Directives are the core building block of an Angular app. Use them to insert
> whatever structure or behavior you need in your app. Views are merely a
> shortcut for very simple use-cases.

The more I work with Angular, the more I agree with this statement. The
framework's greatest power is in the directives. If you want to know where to
invest your energy: directives. (Or, as Varwig puts it: "Embrace directives,
they're cool.")

### Villarreal: "5 reasons to use AngularJS in the corporate app world" (2013)
[[9][9]] \#introducing

Oscar Villarreal:

> You can start by sprinkling it in some places of your app that you can slowly
> start to rewrite. Believe me I’ve done this before and it actually WORKS. Not
> only does it work but it allows you to put the seed of a soon to become
> amazing application.

### Year of Moo: "Animation in AngularJS" (2013)
[[11][11]] \#animation

At yearofmoo.com -- looks like first-class support for animations is well under
way as part of the core AngularJS library. Thorough and pretty comprehensive.
(Current through version 1.1.5.)

### Year of Moo: "More AngularJS Magic to Supercharge Your Webapp" (2012)
[[31][31]] \#i18n \#$apply \#$digest

Comprehensive coverage at Year of Moo; highlights include: detailed information
about `$apply`, `$digest`, and `$$phase`; internationalization and localization.

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
[41]: http://joelhooks.com/blog/2013/09/15/why-i-built-an-angularjs-training-site-on-rails/ "Why I Built an AngularJS Training Site on Rails"
[42]: http://blog.pamelafox.org/2013/05/frontend-architectures-server-side-html.html "Server-side HTML vs. JS Widgets vs. Single-Page Web Apps"
[43]: http://angular-tips.com/blog/2013/08/tip-directives-with-the-same-name/ "Tip: Directives With the Same Name"
[44]: http://blog.freeside.co/post/60977491011/decoupling-from-the-dom-with-angular "Decoupling from the DOM with Angular"
[45]: http://solutionoptimist.com/2013/10/07/enhance-angularjs-logging-using-decorators/ "Enhancing AngularJS Logging using Decorators"
[46]: https://groups.google.com/d/msg/angular/nipAiBQ_lro/BCKYuQ6MN8EJ "service to manage the configuration of the app"
[47]: http://angular-tips.com/blog/2013/08/understanding-service-types/ "Understanding Service Types"
[48]: http://solutionoptimist.com/2013/09/30/requirejs-angularjs-dependency-injection/ "Dependency Injection using RequireJS & AngularJS"