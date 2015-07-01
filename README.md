# dna-router

`ui-router` is an advance Angular's ui-router style router provider for Polymer.

Install using bower: 
bower install `dna-router`

Setting up is easy two step proccess.
1. Define state names and url. Import `dna-new-state.html`.
 ```html
<dna-new-state state='home' route='/home'></dna-new-state>
<dna-new-state state='user' route='/user/:id/'></dna-new-state>
<dna-new-state 	state='app' route='/app' parent='home'>     // Create link /home/app
</dna-new-state>
```


2. Declare your `dna-view` for a particular State. You can have multiple `dna-view` for a single state. 
```html
<dna-view state='home' element='home-template' template='\templates\home.html'></dna-view>
<dna-view state='user' element='login-template' template='\templates\user.html'></dna-view>
<dna-view state='app' element='app-template' template='\app.html'></dna-view>
<dna-view state='app' element='app-sidebar-template' template='\templates\app-sidebar.html'></dna-view>
```

`home-template` is an polymer element inside `home.html`. It looks something like this:


```html
<link rel="import" href="/bower/dna-router/dna-router-behaviors.html">

<dom-module id='home-template'>
	<style >
	:host {
		display: inline-block;
	}
	</style>
	<template>
		<div flex>home</div>
	</template>
</dom-module>
<script type="application/javascript">
    Polymer({
        is:'home-template',
        behaviors: [DnaStateProviderBehavior],		// Note this
        attached: function(){
          // Do your normal stuff.
          // `param` is available in this.param
	}
    })

</script>
```

`DnaStateProviderBehavior` must be imported in the template. It allows to access `param` and also to communicate between different views of same state by triggering custom events.

To communicate just use `this.notify('your-event', data)`. This function will trigger `your-event` type event in all polymer element of same state. Listen it via `listeners`.

```script
listeners:{
	'your-event': '_toDoFunction',
},
```

You can navigate to a different `state` in `ui-router` fashion.
``` script
this.go('user', {id: 123}) //Second parameter is just object containg all params
```


`Note: ` In above exapmple `template` path is absolute.

Don't forget to include `Polymer 1.0` in your project ;) 

Enjoy :)
