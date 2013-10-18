##Webshaped 2013 — A digital design conference
###Helsinki, May 23rd
- - -
###Vitaly Friedmann — Responsive Web Design (Extended), Opening Keynote

>We used to be drawers of rectangles, now the web has changed.

- We have interconnected parts, the web is chaotic in a way, a **nice** way
- We should be drawing systems instead of boxes

>It’s a new mindset that requires us to **rethink** and **extend** our practices.

- RWD means we have to change everything
- RWD comes with a lot of challenges
	- **Main issue**: most HTML pages are written under **an assumption about screen size**
		- Details get lost on small screens
		- Priorities, too, get lost on small screens
		- The content hierarchy **shouldn’t depend on the resolution**
- From the strategical standpoint we face two issues in maintaining the content hierarchy
	- Source order
	- Intermixing content

#####Resolution Independence

- **SVG**
	- Even better: [SVG Stacks](http://simurai.com/post/20251013889/svg-stacks)
	- [SVG Support](http://caniuse.com/#search=svg)
- **Icon Fonts**
	- Even better: Combine icon font glyphs to create complex multi-layered icons
	- [Support](http://caniuse.com/#search=font)
	- [IcoMoon - Icon Fonts Done Right](http://icomoon.io/)
- **Compressive Images**
	- To display photos properly on high pixel density displays **we don’t need hi-res images**
	- [“Compressive Images”](http://filamentgroup.com/lab/rwd_img_compression/)
	- [Retina Revolution](http://blog.netvlies.nl/design-interactie/retina-revolution/)
	- [Progressive jpegs: a new best practice](http://calendar.perfplanet.com/2012/progressive-jpegs-a-new-best-practice/) (considered to be a new best practice)
	- [JPEGmini - Your Photos on a Diet!](http://www.jpegmini.com/)
	- Responsive Images: [Picturefill](http://scottjehl.github.io/picturefill/), [Responsive Images: Clown Car Technique](http://www.standardista.com/responsive-images-clown-car-technique/)
- **Conditional Loading**
	- [Conditional CSS](http://adactio.com/journal/5429/) (super hacky)
	- The Guardians modular load
		- Consider **at least three types** of page content
			- Core content
			- Enhancement
			- Leftovers
		- Load JS asynchronously with `<script async defer>`
			- **Edit**: Avoid `defer` (via [Jake Archibald](https://twitter.com/jaffathecake))
		- Enhance only if a “modern browser” is present
			- BBC’s `isModernBrowser()`
				
					if (
						'addEventListener' in document && 
						'localStorage' in window && 
						'querySelector' in document
					) {
						// http://bukk.it/party.gif
					}
			
	> Performance isn’t solely a technical concern for developers. It’s time to treat performance as an essential design feature. — Brad Frost
		

- **RWD Design Patterns**
	- RWD design affects **all** design assets
		- Layout, images, type, navigation, tables… etc.
	- Offline access and **mobile UX enhancements** complement RWD very well (e.g. HTML5 localStorage, Geolocation, etc.)
- **There are still many unsolved problems**	- Web forms, Flows, Images, Performance, Debugging, moving a really complex legacy site to a responsive site
- **Resources**
	- [Flexy Boxes](http://the-echoplex.net/flexyboxes/)
	- [How To Maintain Hierarchy Through Content Choreography](http://www.smashingmagazine.com/2013/04/25/maintain-hierarchy-content-choreography/)	- [Content Choreography](http://trentwalton.com/2011/07/14/content-choreography/)
	- [An Ajax-Include Pattern for Modular Content](http://filamentgroup.com/lab/ajax_includes_modular_content/)
	- [Gmail for Mobile HTML5 Series: Reducing Startup Latency](http://googlecode.blogspot.fi/2009/09/gmail-for-mobile-html5-series-reducing.html) (from 2009, uses `eval`)
	- [Responsive Design on a Budget](http://clearleft.com/thinks/responsivedesignonabudget/)	- [How to lose weight (in the browser)](http://browserdiet.com/)
	- [Slowy app | Real-world connection simulator and bandwidth limiter](http://slowyapp.com/)
	- [Responsive Data Tables](http://css-tricks.com/responsive-data-tables/)
	- [Responsive Tables (and a calendar demo)](http://dbushell.com/2012/01/04/responsive-calendar-demo/), [Responsive Nav](http://responsive-nav.com/)
	- [TypeForm - Making forms & surveys awesome](http://www.typeform.com/)
	- [This Is Responsive — Patterns, resources and news for creating responsive web experiences.](http://bradfrost.github.io/this-is-responsive/)

###Darrell Stephenson — Backbone.js in production at SoundCloud

- **Quick Assumptions**
	- You know what Backbone.js is
	- You know what a single page app is
- **Soundcloud current stats**
	- The platform reaches 200.000.000 people **every month**
	- **10 hours** of audio uploaded **every minute**
- **Four lessons**
	1. App anatomy
	2. Sharing between modular views
	3. Efficient data respresentation
	4. Cheating
1. **App anatomy**
	- An app in **five parts**
		1. URLs route to layouts
		2. Layouts insert high level views
		3. Views can load and unload other views
		4. Views know what data they need to render
		5. Data is shared between views
		
> Views are modular when they are composable, configureable and self-contained.

- **SoundCloud**
	- **Balance**: models, collections, view
	- 33 Models
	- 48 Collections
	- ~200 Views
- **Lesson #1**
	- Modular views are pretty powerful
	- Most-used, but least built by Backbone
	- Will save you bytes in the long run as your codebase scales
	- Will establish the most convention in your project
		- **This** is how your project will feel to work in
			- **This** drags people into working on your project
- **Lesson #2**
	- Sharing between modular views
		- **Approach 1** — View event bubbling
			- Communicate between views in a hierarchy
			- Send an event **up the chain**, it could come back down, then respond to that
			- Useful for simple messaging, **hard** with more complex things
			- There is **no event namespacing soup** (you are managing your events through strings)
			- Works a bit like DOM events, but not attached to the DOM
			- What if the views are not in the same hierarchy?
		- **Approach 2** — [State Machines](http://en.wikipedia.org/wiki/Abstract_state_machines)
			- A way to encapsulate changing between known conditions
			- Light and added to views as mixin
			- Calls mixed-in methods directly
			- Automatically uses the resource of the view its being mixed into

> Event bubbling and state machines can help to share data between views.

- **Lesson #3**
	- Efficient data representation
	- [“Dogfooding”](http://en.wikipedia.org/wiki/Eating_your_own_dog_food) i.e. **using your product the same way the user does**
	- SoundCloud decided to “eat their own dog food” by serving the **public API**
		- **Good** things about serving the public API:
			- The data is there, let’s go
			- Concentrates common business logic
			- If the API can support our app, it can support anything
		- **Bad** things about serving the public API:
			- Too generic to be fast at a scale
			- Inflexible because of external obligations (**an update may break somebody’s app**)
			- Changing the dog food isn’t really possible any more
				- Client code gets the brunt of it
				- More complexity on the client
				- More expensive operations on the client
				- More requests to the API
		- **Lesson learned**: Using the public API doesn’t work well at scale
			- Use flexible services without external obligations
			- Serve up good food and feed your app efficiently
- **Lesson #4**
	- Example: [Skrillex](https://soundcloud.com/skrillex) gets a lot of comments + infinite scroll = **~270.000 DOM nodes** = [bukk.it/killmepls](http://bukk.it/killmepls.jpg)
	- Limit-enforced template approach
	- Switch to `canvas`, **equals 1 node per sound, handles any amount of comments**
	- Going even further: Limit to last **x** comments
	- You can’t use `canvas` for everything
		- **Not a magic bullet for performance**
	- If a view is compressed into a very small space, draw a fake (SoundCloud actually does this!)
	- **Lesson learned**: Break out of the standard view pattern where it makes sense
- **Example screens**: [Small](http://cl.ly/image/2p2C0a0S3j0B), [Big](http://cl.ly/image/0z3b352Y2Z2y)
- **Resources**
	- [Building The Next SoundCloud](http://backstage.soundcloud.com/2012/06/building-the-next-soundcloud/)
	- [Slides](http://futuredarrell.github.io/webshaped-2013/#/)


###Jake Archibald — Rendering without lumpy Bits

> Our responsibilities and boundaries change so quickly on the web.

- Start of the web: Text on screen
- Everything become **much more complicated so quickly** — “Firefoxes”, “Chromes”, etc. made all this happen
- Facebook dropped their graphical performance and witnessed that **user numbers went back**
- Things like adding a class, changing `.style`, etc. will call a **style calculation**
- DOM changes, resizing the browser will call a re-rendering, style changes (may)
- **Paints** are caused by visual changes, triggered by DOM, changes of style, highlighting text

> We want everything to be instant. **How fast is instant?**

- If you are going to read something from the page, **cache it inside variables**
- [Requires layout](https://dl.dropboxusercontent.com/u/5928175/Requires%20Layout.jpg)
- “Enable continuous repaint” in Chrome dev tools = HOT
- “Enable paint rectangle” in Chrome dev tools = HOT
- **Heavy styles**:
	- **X** may be faster than **Y** today, but tomorrow?
	- Use dev tools to see what’s going on
- **Browser applications**:
	- When in Chrome, do as Chromans do (Jake himself, for example)
	- Firefox: “Toggle-paint-flashing” add-on, or Aurora dev tools
	- WebKit Nightly: Web Inspector
		- `defaults write com.apple.Safari IncludeInternalDebugMenu 1`

> IE used to be a dick. There’s no good word that comes after that.

> It’s simple to say which versions of IE are a dick: <=8

- **Mobiles**
	- Mobiles are small and can be slower. That is all mobiles do. 
	- **Nearly everything that used to be unique to mobile isn’t any more**
	- If you are doing anything on mobile, make sure to use [FastClick](https://github.com/ftlabs/fastclick)
		- Takes care of 300ms delay on mobile and does many other, neat things
- **100ms — Is it good enough?**
	- **100ms** is **NOT** good enough for animation
	- As yourself **WTFPS?** (What the frames per second?)
	- Audio and film: **24fps**, anything lower was too unnatural
		- 24fps is **not good enough**
	- A higher refresh rate isn’t very beneficial
	- If we want smooth motion, **we want to deliver a frame for every refresh of the screen**
- **16.67ms** is our deadline

> Animation is like orange juice, milk and farts.

- Porting `requestAnimationFrame` to `jQuery`: [https://github.com/gnarf37/jquery-requestAnimationFrame](https://github.com/gnarf37/jquery-requestAnimationFrame)

- **The GPU** — Make the hardware do the work
	- `translateZ(0);` to trigger hardware acceleration
	- Animate `translate` instead of `top` and `left` for smoother animations
	- `translate` doesn’t jitter when animating, it doesn’t “snap” into pixels
	- Firefox will still snap, adding a tiny, tiny `rotate()` will help here

> The web used to be a flat canvas. But less and less so.

- **What triggers layers in Safari and Chrome?**
	- 3D transforms
	- Animating/transitioning transform/opacity/filters
	- Flash/Silverlight
	- Canvas
	- Video
	- Fixed position elements, sometimes
- **Remember**:
	- WTFPS?
	- Don’t overdo layout
	- Be mindful of painting costs & when it happens
	- Don’t make users suffer the stupid 300ms tap delay (use FastClick)
	- Use `requestAnimationFrame`/CSS for smooth animations

> I get this question asked a lot: What is the difference between WebKit and Blink? Nothing.<br /> Of course you have to test in both, but it’s not that much of a deal, really.

- **Resources**
	- [Slides](https://speakerdeck.com/jaffathecake/rendering-without-lumps)
	- **Jake**: “Apologies, these slides mean very little without the talk to go with them. Will put a url to a recording when I have one.”

###Andrew Nesbitt — Turbo charging your workflow with Node.js

- What is a workflow? **Everything that you do while you are working**
- **Automate everything to save time**
	- Maybe if it’s just a second, if you are doing this second every minute of every hour, **it’s a waste**
- Automate boring bits
- Reduce errors — **Computers are very good at reducing errors, humans are not**
- Stay in the flow
- Focus on the stuff that matters

> A good developer is a lazy developer.

- Try automating everything that is **done on a regular basis**
- … goes on explaining [node](http://nodejs.org/) and [npm](https://npmjs.org/)
- Three `npm` modules that will help you supercharge your workflow
	- [Bower — A package manager for the web](http://bower.io/)
		- Allows you to install any front-end JS components by cloning the `git` repository
		- For example, installing `jQuery` is as simple as `bower install jquery`
	- [Grunt — The JavaScript Task Runner](http://gruntjs.com/)
		- Automates all sorts of things: **linting, testing, copying, compiling, concatenating, etc.**
- “Automating the automation”
	- [Yeoman — Modern workflows for modern webapps](http://yeoman.io/)
















