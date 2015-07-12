---
title: JavaScript is Terribad
subtitle: A Reflection on the Web-Development Community's Shared Case of Stockholm Syndrome
---

Remember when JavaScript was simple? When we served down HTML from the server
then displayed all that lovely markup to a user and then made it dance with
eloquently written lines of JavaScript?

**Yea.... Me either.**

JavaScript is shit - it's always been shit. To understand
why it is shit [you must first learn it's history](http://speakingjs.com/es5/ch04.html).

Here is a brief summary:

* [Brendan Eich](https://en.wikipedia.org/wiki/Brendan_Eich), a god amongst morals, was hired at Netscape.
* He proceeded to implement [Scheme](https://en.wikipedia.org/wiki/Scheme_(programming_language)) in the browser.
* Sun partnered with Netscape.
* Management decided "We need Java in the browser."
* Management wanted a prototype "Now."
* 10 days later, JavaScript (then LiveScript) was born.

Maybe my hatred stems from the thought that, had management been left out of the
decision, I *could* have been writing Scheme in the browser. I'm not sure. But what
I do know is that the JavaScript community has gone pants-on-head crazy with all its
libraries, build tools, and frameworks. What we are left with in the wake of
management's and unshakeable love of Java is a language that:

1. Won't run outside the browser.
2. Has *almost* no standard Library.
3. Is slower than Pharaoh's army [and getting slower](http://kpdecker.github.io/six-speed/)

##Angular vs Ember vs React vs ...

How is it that this happens so often in the JavaScript world? There are far more
framework than there should be.

* Ember.js
* Angular.js
* Backbone.js
* Bootstrap.js
* React.js
* SproutCore
* Meteor

That is just getting started. How is anyone supposed to choose the proper framework
for their tool? Perhaps this is why people are so fervently passionate about their
framework of choice being the ONLY framework you should use. Once you pick a framework
it takes weeks of research to even select with framework you will learn next and by
time you pick one a new framework will pop out of the woodwork.

##Package Managers

* NPM
* Bower
* Jam
* Ender
* Volo

Where does the list end? At least the majority of the
JavaScript community has agreed that [npm](https://www.npmjs.com/) is good but when
will people stop trying to roll their own package management solution? Half of these
things need to be put on a boat and sent out to sea.

##Transpiled Languages

"JavaScript is the byte-code of the internet" is something I have been hearing since 2013.
Maybe it is because we all truly do hate JavaScript or maybe it is because the simple task
of defining a class with a few methods in pure JS looks something like this.

```js
function Dog(name){
   this.name = name;
}

Dog.prototype.speak = function(){
    alert(this.name + " barks");
};

Dog.prototype.walk = function(){
    alert(this.name + " walks across the room");
};

Dog.prototype.walk = function(){
    alert(this.name + " goes to sleep");
};
```

Don't even get me started on what inheritance looks like. Why are we trying to do
OO in JavaScript anyway?

But, I don't think that the insane prototypal inheritance interface that JS has is the reason
that things like Coffeescript, ECMASCRIPT 2016, and ClojureScript exist. I think
it is because the JavaScript Standard Library is the weakest of any standard lib
out there. Trying to write a pure JavaScript application is like trying to dig
yourself to the center of the planet with a spoon.

##When will I get a true scripting language in the browser.

One day I hope I live to see this.

```html
<script type='python'>
...
</script>
```

Or

```html
<script type='ruby'>
...
</script>
```

I don't know how, and I don't know when but  I do know that the browser needs a proper scripting language.
Maybe the [ASM.js](https://en.wikipedia.org/wiki/Asm.js) team will pull this off. I have
high hopes for [Emscripten](http://kripken.github.io/emscripten-site/) and what it could
do to the frontend development world. In case you have not heard, Emscripten is an LLVM
to ASM.js compiler that *can* allow any C based language to be run natively in the browser.

##In Conclusion

There is no way on gods earth that JavaScript can be improved on. You would need to pull together
a consortium of all the browser development teams (and their bosses). To even begin to start
to have this conversation. Perhaps this has already been done but I cannot image that it 
would prove to be very production. Again, I think that projects like [Emscripten](http://kripken.github.io/emscripten-site/)
are giving us somewhat of a brighter future for frontend development that could result in more
readable, maintainable, and faster code that does not pull in 9000 modules that you have never
heard of before. Long story short, I wish I had to option to leave the JavaScript ecosystem in 
order to do frontend development. 
