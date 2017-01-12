_Application Developer notes_
# 5.1 Current limitations
This section contains additional information which we hope will be of value to Aurelia application developers using KendoUI bridge.
<br><br>

#### Two way binding

Let's just begin by saying that with plain Kendo controls it's not possible to have two-way databinding at all. We build a value-binding feature that allows you to two-way databind the value property and it uses the value() function of the Kendo control to tell it to update the value. So this is not "real" two-way databinding, but from a user perspective it would seem as shown in this: **[example](http://aurelia-ui-toolkits.github.io/demo-kendo/#/samples/autocomplete/value-binding)**.
<br>

The datasource is also a bit unique. You can bind the datasource property of a control to a plain javascript object like we do here. Kendo's combobox is getting this javascript object and wraps it into a datasource class instance. The original reference to the datasource is lost, so if you update a property in there it will not be noticed by the control. Wrapping the plain javascript object into a new `kendo.data.DataSource()` is one of the solutions you may consider:
<br>
```
	datasource = new kendo.data.DataSource({
		transport: {
		  read: function(options) {
		    return System.import('samples/chart/area-charts/json/spain-electricity.json!json')
		    .then(data => options.success(data));
		  }
		},
		sort: {
		  field: 'year',
		  dir: 'asc'
		}
	});
```
<br>
That way Kendo's control wont wrap the datasource into a a kendo.data.DataSource instance again, but it will just use the original instance (call by reference is used instead of call by value with plain javascript objects). So if you use `kendo.data.DataSource` instead of the plain javascript object, so you should be able to bind a single datasource to multiple controls.
<br>

In all other cases you'd need to either use functions as `enable()`, `setOptions()` or even our `recreate() `function to tell the control to pick up the changed value
<br>

**Note:** Some Kendo controls have functions like `setOptions()` or `setDataSource()` that allows you to change an option of the Kendo control without having to destroy and recreate it.

***
***



