#HTML5 - Beyond the markup - @johnallsopp

##HTML5 core concepts and syntax

###Relax! We can use HTML or XHTML syntax.

With HTML5, a lot of things are now optional including
*	The `<html>` element
*	`<body>`
*	closing `li` and `p` tags
*	quoting attribute values
*	but `<title>` is required element

Google 404 page is a good example of optional markup and stripping out tags to save file size: http://www.google.com/fake

Today we are focssing on the aspects of HTML5 hich add new or improved functionality, rather than simply new semantics.

###What happens in the DOM when element are omitted?

The browser fills in all the gaps in the DOM. Browsers have always done this but I like the way that it's more standardised now.

That's not to say you shouldn't be using new elements like header, nav and footer.

## HTML5 Forms

Inputs and other form elements no longer need to be the descendants of a form.

We bind them to forms with the forum attributes.

The value of this attributes is the id of the form the elements is associated with.

----
Example of a detached form:

	<form id="contact-form">
		...
	</form>
	
	<input form="contact-form" name="name" type="text" required/>
----

### new input type attributes and other form elements

*	`search`
*	`tel`
*	`url `
*	`email` 
*	`number` 
*	`range`

These are especially great on mobile devices with virtual keyboards.

New inputs types fallback to text which is great for backwards compatibility.

### New input types

Avoid getting in the way of the standards of the users device/environment. For example input `type="search"`.

Some odd ones are range and `date`/`time`/`month`/`datetime`:

*	`range` is an "imprecise number-input control". it's a slider where you get to choose numbers between a range.
*	`date` pickers are perhaps the tricks to use, rubbish on desktop but great on tablets/phones.
*	file/camera will be awesome once it's supported.
*	built-in browser validation for the new input types.

####Number and range elemets

*	Constraining input `type="number"` or `type="range"` with `min="1"` `max="11"` `step="0.5"`
*	The leading 0 in `step` < `1` is REQUIRED.

#### datalist element

*	like a combo-box in windows
*	lets users pick an option or add their own option.
*	the id needs to match a list attribute on another input element.
*	a great example of a data list would be a state picker.

Other new form elemets, things that will be supported soon.

*	`color`
*	`meter`
*	`progress`
*	`output`

####meter element

*	a scalar measurement within a known range or a fractional value.
*	example disk usuage.
*	visualising numerical values within a range

####progress element

*	like a meter but for progress bards
*	animated with a candy stripe on webkit but looks like meter on gecko
*	for a task or progress

####output element
*	result of a calculation in a form.
*	for example a shopping cart where the output could be using for displaying the price.
*	output can be used across browsers safely

###New form attributes

####	placeholder
*	default content placeholder that disappears when you edit the input.
*	something extra for the user but if it's not supported it doesn't matter. 

####	required attribute
*	a boolean attributes indicating that the element must be completed by the user.
*	Can be user for **styling** and for form **valudation**.

####autofocus
-	The first part of the forum to get `focus`.

----
Example of a boolean attribute:

	<input type="text" autofocus>

or this if you want strict XHTML style content.

	<input type="text" autofocus="autofocus">
----

####Autocomplete attribute

	autocomplete="off"
----

In iOS the attribute is autocorrect. another one is autocapitalise.


####Spellcheck attributes
	spellcheck="false"
Note: It's super annoying how some are are on/off and some are true/false.

----


####contenteditable

	<p contenteditable="true"></p>
----

Something else to think about is the CSS `user-modify` property but it's had inconsistent browser prefixes and syntax.

### constraining and validating input

* Well designed forms ensure their user complete all required content and make it simple to input valid content.
* Always validate on the server side as well. Client side validation is about creating a better user experience.

#### Auto validating elements

* Some new inputs we've just looked at will auto-validate, browser support is growing.
* `:invalid` and `:valid`  are great new selectors for styling inputs. These are CSS level 4. ZOMG CSS4.
* **regex** can be used for validating form inputs using the *pattern* attribute.
* you could use input `type="tel` with `pattern="xxx"`.

#####List of css selectors relating to input validation.

* `:valid`
* `:invalid`
* `:required`
* `:optional`
* `:in-range`
* `:out-of-range`
* `:enabled`
* `:disabled`
* `:checked`

"Just because you can, doesn't mean you should." Don't go changing default device styles just because you feel like it.

For example you might stick a background on an input that is `:invalid`.

----
Gotcha alert, not a good idea.
	
	input:required:before{content: '*'} //don't use this in real life.
----

####DOM/JS for validation

`oninvalid` is a new attribute that corresponds to a js event.

###styling with CSS3

The shadow dom is the sub-objects for things like video, date pickers etc. Not yet standardised. You can style these but be careful.

Users often expect things like scrollbars and dropdowns the way they are used to.

### selectors API
----
Using the concept of jQuery without actually using jQuery.

	querySelector('css style selector string'); // returns the first matching element.
	
	querySelectorAll('css style selector string'); // returns all elements matching the selector.
----

##Offline Web Apps

How we can make our sites work even when the user is offline.

###Appcache

Even if you develop web sites rather than 'applications' you can benefit from techniques outlined here.

App cache gives you more control over what items are being cached and stored locally. Use it to increase performance.

The appcache manifest

1. create and appcache manifest file
2. link to this from html documents
3. beware of the quite a few gotchas

The appcahce is a text file with .appcache extension, it trumps .htaccess and specifies everything.

----
Example of a cache manifest file:

	CACHE MANIFEST
	#version 1.0
	#the version number is important, it lets us know when there is a new version of the file.
	
	CACHE:
	#images
	/images/image.png	
	
	NETWORK:	
	signup.html
	payments/pay.html
	/payments/
	# you wouldnt use * wth the above because it would do everything anyway.
	*
	
	FALLBACK:
	#in the format of "pattern SPACE specific-url-to-fallback" for example:
	/images/ /images/missing.png
	#kind of like an offline 404 page:
	/ /sorry.html
----

#### The appcache gotchas ####

* will cache files indefinately, unles you change the manifest with version numbers.
* there may be things that we don't want the server to cache, signup pages, timely data

####The network sections
- when online shows new info
- when offline shows a fallback
- we can use patterns and wildcard(*) to tell it what to not cache.

#### How we use the apcache manifest

----
An example of pointing to a manifest file.

	<html manifest='manifest.appcache'>
----

The manifest file **must** be serves as **text/cache-manifest**

Make sure that you set the mime type in the .hataccess and is supported in H5B.

####The gotchas

-	Persistence. Caches DO NOT expire. Appcache trumps everything.
-	Never develop with Appcache turned on.
-	Resources must be explicitly cached, images in style sheets and scripts won't be cached automatically.
-	pay attention to `@import`
-	My tool ManifestR can help here
- 	The one exception is HTML documents with a manifest attribute included.

#### Caching cross domain resources

-	Cached resources don't have to be in the same domain
-	The exception is for resources served over https, which do
-	Chrome does not honour this restriction	
-	With CDN's it's argued that this policy is too restrictive for real world use of appcaching
-	basically appcache is over HTTPS needs to be all served from the same domain.

####Lazy caching

html documents with manifest attr will be cached even if not included in the manifest. EVEN IF INCLUDED IN NETWORK.

####cache failure

If one of the resources are not available the cache will not be **built** or will not be **updated**.

#### developing with appcache

-	use .htaccess to change mimetype for .appcache to disable appcache completely while in development.

###Web storage

Instead of saving data on the server we can store it on a local device.

Cookies are very insecure. Plain-text.

The two kinds of storage on the client side are:

- `localStorage`
- `sessionStorage`

sessionStorage is a property of the weindow object in the DOM.

Because it is not universally supported, well want to check that this property exists.

----
Example of how to test for session storage:

	if (window.sessionStorage) {
		//we use sessonStorage
	}
	else{
		//we do something else, perhaps use cookies, or another fallback.
	}
----
Example of setting an item in session storage.

	function displayDetails(){
		var name  = 'jonathan';
		window.sessionStorage.setItem('name',name);
	}
----
Example of accessing an item in session storage.

	function displayDetails(){
		var name  = window.sessionStorage.getItem('name');
	}
----
Example of removing an item session/local storage.

	localStorage.removeItem(key);
----
Clear the entire localStorage.

	window.localStorate.clear();
----

####Things to know…

- Everything stored is a string.
- This includes boolean values, integers, floating point numbers, dates, and so on
- Keep this in mind if you are storing boolean prefs, jason or more complex types of data.

Be aware that in private mode you won't know if they are using the session/local storage or not.

`localStorage` and `sessionStorage` are *synchronous*, this means that they could slow down the loading time of the webpage. javascript on the page stops execute while the storage function is performed.

----
Property that we can use to tell if the user is online or not.

	navigator.onLine
----
Adding an event listener in javascript to detect online/offline.

	window.addEventListener("offline", updateUI());
	window.addEventListener("online", updateUI());
----

###Web databases

Web databases are tricky. Don't use indexedDB until is widely supported.

The advantages are:
-	speed
-	complex data types

Some browsrs support webDB while others are using indexedDB. Bit of a nightmare.

## Geolocation

Don't panic.

Help the user to do things on the tablet or phone. Pre-fill form fields perhaps. Location, currency.

----
geolocation code examples:

	geolocation.getCurrentPosition();
	
	geolocation.watchPosition();
----

Use callbacks because js is single threaded and a geo request would block the rest of the web app.

A position is an object with 2 attributes.
	
- `timestamp`
- `coords` which is an object with properties: `latitude`,`longitude`,`altitude`

Example of a callback.

	function showLocation(pos){
		alert(pos.coords.latitude); //or whatever other property you want.
	}
	
	navigation.geolocation.getCurrentPosition(showLocation);

----

##Resources
http://caniuse.com/ What can I use?

http://westciv.com/tools/manifestR/ - ManifestR

http://westciv.com/tools2/ - Live tools

https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills - HTML5 Polyfills

