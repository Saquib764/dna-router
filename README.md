# dna-router

`ui-router` is an advance Angular's ui-router style all html router provider for Polymer. It has `auth` status which enables user to show different views for same state.
Inspired by ui-router: https://github.com/angular-ui/ui-router

Install using bower: 
```script
bower install dna-router
```

Import all element:
```script
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
	auth-req
	element='auth-home-template'
	no-auth-element='home-template'></dna-view>
```
If user is not authencated, `home-template` will be shown. To redirect to a different state. Example, `login`.
```html
<dna-view
	state='home'
	auth-req
	element='auth-home-template'
	no-auth-state='login'></dna-view>
```

3. Configure `dna-router`:
```html
<dna-config 
	id='conf' 
	home='some state' 
	auth='true'  // authorise
	template='\templates'> </dna-config>
```
By default `home` is state named `home` and `auth` is false.

To authosrise on fly using javascript:
```script
var conf = document.querySelector('#conf');
conf.auth = true
```

# Executing a function on page load
`dna-router` provides a `DNA` object. 
```script
DNA.run = function(){
	// Do your stuff
}
```



Enjoy :)
