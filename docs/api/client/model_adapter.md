# ModelAdapter

The `ModelAdapter` class allows you to define how a `Model` retrieves its data. You can define the ModelAdapter as follows:

## Defining

`Model.includeAdapter(adapter, this)`

Defining the a new `ModelAdapter` is easiest in the `Model` definition, as shown below. In the definition, you can define all the potential URLs for interacting with the API server (remember to exclude the host).

The URLs that are definable are `load_url`, `save_url`, `index_url` (Delete also uses the `save_url` but uses the `DELETE` HTTP method).

```coffeescript
class @TodoItem extends @Model
	Model.includeAdapter
		type : ModelAdapter					# this is optional, type is ModelAdapter by default
		load_url : '/api/todo_items',		# also used for index unless defined separately
		save_url : '/api/todo_item'			# also used for delete
	, this
	init : ->
		...
```
	
## Properties

### ModelAdapter.host

`ModelAdapter.host = new Host("/api")`

The `ModelAdapter.host` property lets you set the default host for all ModelAdapters defined for your Application. To override this, you need to set this property before any other ModelAdapters are defined, as they inherit this value upon instantiation.

```coffeescript
ModelAdapter.host = "http://api.mycoolapp.com/v1/"	# default host for all adapters
TodoItem.Adapter.host = "http://api.a-different-host.com/v2" # this adapter talks to a different host
```

## Default Actions

`ModelAdapter.[save|load|index|delete] opts`

The ModelAdapter is essentially a wrapper that let's you access the server at predefined endpoints using a consistent format. A few actions are predefined for you automatically, such as `save`, `load`, `index`, and `delete`. These methods take a hash argument which should define the `data` and the `callback`.

```coffeescript
class @HomeView extends @View
	init : ->
		@todo = new TodoItem()
		@adapter_is_saving = ko.observable(false)
	...
	saveTodoItem : ->
		TodoList.Adapter.save
			data : {id: @todo.id(), description: @todo.description()}
			loading : @adapter_is_saving
			progress : (ev, value)=>
				... handle the progress here (this is optional) ...
			callback : (response)=>
				... handle the response here ...
```

Note the `loading` field, which sets the passed observable to true while the request is pending, and false when done. The `loading` and `progress` parameters are optional.

To define your own actions, use the `route_method` action.

## Methods

### route_method

`TodoItem.Adapter.route_method method_name, url_path, [http_method]`

Generally you will need more actions than `save` and `index`. This method lets you define an action and it's corresponding endpoint. It is callable in the same fashion as the default actions.

```coffeescript
TodoItem.Adapter.route_method 'mark_completed', '/todo_item/mark_completed' # if third argument is ignored, it defaults to a 'POST'
```

