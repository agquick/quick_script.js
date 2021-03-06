# Application

The `Application` class is the heart of your web application views. You must extend it using the standard coffeescript call, and name it whatever you passed to `include_quick_script_init`. Here you define the name of your application, initialize your application views, and handle routing. `Application` extends the `View` class, so you have all of the functionality available in a normal view. For more information, see the `defining` method.

## Defining

`class @AppView extends @Application`

To begin developing your application, you need to extend the <b>Application</b> class. This allows you to setup your application flow and settings.

```coffeescript
class @AppView extends @Application
	init : ->
		@name = "TodoApp"
		@addView 'home', HomeView, 'view-home'
	account_model : User
```
	
## Properties

### path_parts

`@path_parts[idx]`

An array that tracks the URL location of the website 

```ruby
#!ruby
# user is on page /todo_item/12345
root = @path_parts[1]
# root = "todo_item"
id = @path_parts[2]
# id = "12345"
```

## Methods

### handlePath

`handlePath : =>`

The `handlePath` method can be overridden to control the routing of the application based on the browser URL. Specifically `@app.path_parts` contains an array of the, well, parts of the path, split by '/' (note that it starts at 1). Using a simple switch statement you can control the routing of your application.

```coffeescript
handlePath : =>
	switch @app.path_parts[1]
		when 'help'
			@selectView 'help'
		when 'home', ''
			@selectView 'home'
		else
			@app.redirectTo '/home'
```

### redirectTo

`@redirectTo(path)`

Routes the user to a new path (local to the web application)

`path` - new path

```ruby
@redirectTo("/todo_item/12345")
```

