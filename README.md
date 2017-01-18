# dna-router

`Note: ` `on-auth` and `on-no-auth` attributes changed to `with-auth` and `with-no-auth` due to `on-*` conflicts with current update of Polymer. Thanks to <a href='https://github.com/jom'> jom</a>

`ui-router` is an advance Angular's ui-router style all html router provider for Polymer. It has `auth` status which enables user to show different views for same state.
Inspired by ui-router: https://github.com/angular-ui/ui-router

Install using bower:
```script
bower install dna-router
```
`Update:` Added `Vulcanize` support.
`Use:` Add 'manual-load' attribute in 'dna-config' tag and then import the vulcanized templates in your project.

Download starter kit at : <a href='https://github.com/Saquib764/starter-kit-dna-router'>https://github.com/Saquib764/starter-kit-dna-router</a>


Import all element:
```html
<link rel="import" href="./bower_components/dna-router/dna.html">
```

1. Define states and routes:

	```html
	<dna-new-state state='home' route='/home'></dna-new-state>
	<dna-new-state state='user' route='/user/:id/'></dna-new-state>
	```
2. Defining views. You can have multiple views for a single state.
	```html
	<dna-view
		state='home'
		element='home-template'></dna-view>
	```
	By default, `dna-view` converts `element` name into camel case and imports file named so in base directory. This file must contain `home-template` element. Example, above imports `\homeTemplate.html`.

	All params is available in `home-template` polymer properties `params`.

	To import file from different directory:
	```html
	<dna-view
		state='home'
		template='\templates'
		element='home-template'></dna-view>
	```
	`NOTE:` Set global template path using `dna-config`.

	If states requires authentication.
	```html
	<dna-view
		state='home'
		with-auth
		element='auth-home-template'
		else-element='home-template'></dna-view>
	```
	If user is not authenticated, `home-template` will be shown. To redirect to a different state. Example, `login`.
	```html
	<dna-view
		state='home'
		with-auth
		element='auth-home-template'
		else-state='login'></dna-view>
	```
	Show some page only to 'unauthenticated' users
	```html
	<dna-view
		state='home'
		with-no-auth
		element='login-template'
		else-state='dashboard'></dna-view>
	```

3. Configure `dna-router`:
	```html
	<dna-config
		id='conf'
		home='some state'
		auth  // authorise
		template='\templates'> </dna-config>
	```
	By default `home` is state named `home` and `auth` is false.

	To authosrise on fly using javascript:
	```js
	var conf = document.querySelector('#conf');
	// Sending login context to views.
	// Access it using '$state' global variable.
	conf.context = {
		author: 'Saquib Alam',
		message: 'Star me :)'
	};
	conf.auth = true
	```
4. `S-ref` element:
	- With goto and params property
		```html
		<a is='s-ref' goto='["users"]' params='{"user_id":"56"}'>To state users</a>
		```
	You can use a Polymer variable in your params :
		```html
		<a is='s-ref' goto='["users"]' params='{"user_id":"{{userId}}"}'>To state users</a>
		```

	- With goto property only *(backward compatibility)*

		`goto` takes an array as input. First is state name and second item is object with params. Its similar to `ui-router` `s-ref`.

		```html
		<a is='s-ref' goto='["users",{"user_id":"56"}]'>To state users</a>
		```
		However you can't use Polymer variables that way.

	Use `$state.params.user_id` to access params.

5. `dna-many-view` element.
	This element is visible only if any `dna-view` inside this element or any of it's `state` is active.
	```html
	<dna-many-view state='abc xyz'>
		This Example
		<dna-view state='home' ....></dna-view>
	</dna-many-view>
	```
	In above example, many view is visible for states `abc, xyz and home`. For any other state none of its content is visible. `"This Example"` is not visible for some state, i.e `login`.

6. Define not found page:

	To define a route to page not found use the state `notfound`, set the route to page not found in `url` and create a `dna-view` with state `notfound` adding an element to be displayed if the page is not found.

	Define the route
	```html
	<dna-new-state state='notfound' route='/notfound'></dna-new-state>
	```
	Create a `dna-view` with state `notfound` and set an element that will be shown when no page is found
	```html
	<dna-view
		state='notfound'
		element='notfound-template'></dna-view>
	```

# Executing a function on page load
`dna-router` provides a `DNA` object.
```js
DNA.run = function(){
	// Do your stuff
}
```



Enjoy :)
