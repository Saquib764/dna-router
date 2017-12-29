# dna-router
`Note: ` Update to Polymer 2.0

`Note: ` `with-auth` and `with-no-auth` attributes is now added on `dna-state` instead of `dna-view`.

`dna-router` is an advance Angular's ui-router style all html router provider for Polymer. With in built authentication management.

Inspired by ui-router: https://github.com/angular-ui/ui-router

Install using bower:
```script
bower install dna-router
```
`Update:` Added `Vulcanize` support.
`Use:` Add 'manual-load' attribute in 'dna-config' tag and then import the vulcanized templates in your project.

<!-- Download starter kit at : <a href='https://github.com/Saquib764/starter-kit-dna-router'>https://github.com/Saquib764/starter-kit-dna-router</a> -->


Import all element:
```html
<link rel="import" href="./bower_components/dna-router/dna.html">
```

1. Define states and routes:

	```html
	<dna-state state='home' route='/home'></dna-state>
	<dna-state state='user' route='/user/:id/'></dna-state>
	```
	If states requires authentication.
	```html
	<dna-state
		state='home'
		with-auth
		route='/home'
		else-redirect='login'></dna-view>
	```
	If user is not authenticated, it will be redirected to 'login' state. 

	Show some page only to 'unauthenticated' users
	```html
	<dna-state
		state='login'
		with-no-auth
		route='/login'
		else-redirect='home'></dna-state>
	```

	State without `auth` attribute are shown irrespective of auth status

2. Defining views. You can have multiple views for a single state.
	```html
	<dna-view
		state='home'
		element='home-template'></dna-view>

	<dna-view
		state='home'
		element='home-another-template'></dna-view>
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


3. Configure `dna-router`:
	```html
	<dna-config
		id='conf'
		home='some state'
		template='\templates'> </dna-config>
	```
	By default `home` is state named `home` and `auth` is false.

	To authosrise on fly using javascript:
	```js
	DNA.Auth.authorised = true
	//	Then reload
	var state = DNA.$State()
	state.reload()
	```
4. `dna-a` element:
	Equivalent to `a` tag in html.
	- With goto and params property
		```html
		<dna-a goto="contact">To state X</dna-a>
		```
	- You can use a Polymer variable in your params :
		```html
		<dna-a goto="stateY" params='[[parameterObject]]'>To state Y</dna-a>
		```

5. `dna-many-view` element.
	This element is visible only if any `dna-view` inside this element or any of it's `state` is active.
	```html
	<dna-many-view state='abc xyz'>
		This Example
		<dna-view state='home' ....></dna-view>
	</dna-many-view>
	```
	In above example, many view is visible for states `abc, xyz and home`. For any other state none of its content is visible. `"This Example"` is not visible for some state, i.e `login`.

<!-- 6. Define State for wrong url or page not found:
	
	Declare state to be used for wrong url in `dna-config` :

	```html
	<dna-config
		id='conf'
		home='some state'
		wrong-url='notfound'
		auth  // authorise
		template='\templates'> </dna-config>
	```

	Then simply define a state with name `notfound` (state name can be anything) and create a view for same.

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
 -->

# Executing a function on page load
`dna-router` provides a `DNA` object.
```js
DNA.run = function(next){
	// Do your stuff
	//	Check for login status in cookies
	//	Load your assets

	
	next()	// Donot forget to call this when using `run` function.
}
```

# Available global variable and function
1.	DNA.Auth: Contain authencation information. Can be used to share information accros the app just like a normal javascript variable
2.	DNA.$State(): Returns current state and its parameters. Also provide two function `go` and `reload`.

	
	```js
	var state = DNA.$State()

	//	GO example
	state.go("next_state_name", parameterObjectOptional)

	//	RELOAD example
	state.reload()

	```


Enjoy :)
