_Application Developer notes_
# 5.9 What you need to know
There are a few things you should know when working with the aurelia-kendoui-bridge.

<br>
### 5.9.1 Who loads Kendo?
Aurelia-kendoui-bridge assumes that Kendo controls have been loaded before a wrapper is used. For example, if you use `<button ak-button>my button</button>` then you will want to make sure that the Kendo Button control has been loaded. The bridge does not care about **how** you load Kendo controls, just that you do.

<br>
<br>

### 5.9.2 How to load aurelia-kendoui-bridge custom elements / attributes
The recommended way to load aurelia-kendoui-bridge custom elements and attributes is via the `<require>` element. This way your application's startup will not be slowed down, and wrappers will be loaded on demand. 

Each sample on the [catalog](http://aurelia-ui-toolkits.github.io/demo-kendo/) has a "imports" button. Clicking this button will show you relevant `<require>` statements that you can copy paste into your app.
![img](http://i.imgur.com/IPLbhFR.png)

Alternatively, you may want to load wrappers on startup. This is possible via `main.js`. A callback can be provided when configuring the plugin:

`.plugin('aurelia-kendoui-bridge', kendo => kendo.core())`

The `kendo.core()` call will load all aurelia-kendoui-bridge custom elements / attributes of the Kendo Core suite for you.

`.plugin('aurelia-kendoui-bridge', kendo => kendo.pro())` does the same, but then for the Kendo Pro suite.

`.plugin('aurelia-kendoui-bridge', kendo => kendo.detect())` automatically detects which Kendo controls have been loaded (via index.html) and loads the matching aurelia-kendoui-bridge custom elements / attributes. This is very useful when you use `kendo.custom.min.js` (composed file from the Telerik website)

<br>
<br>
### 5.9.3 Conventions
Just like Aurelia, we use conventions. All custom elements and custom attributes use the `ak-` prefix, all properties are prefixed with `k-` and all events use the `k-on-` prefix.

For example, the [kendo API documentation](http://docs.telerik.com/kendo-ui/api/javascript/ui/button#configuration-enable) of the Button control, mentions an `enable` property.

This translates into this HTML tag:

    <input ak-button="k-enable.bind: true"/>

Here we are using the `ak-` prefix for the button custom attribute and the `k-` prefix for the `enable` property of the button.
<br><br>

When trying to use properties that are camelcased, in HTML they become dashed: `dataSource` -> `data-source`.

It is also possible to delegate Kendo events. The `k-on-` prefix has to be used here.

To illustrate this, we'll take a look at the [open](http://docs.telerik.com/kendo-ui/api/javascript/ui/autocomplete#events-open) event of the Autocomplete control. This translates into:
<br><br>

    <ak-autocomplete k-data-source.bind="items" k-on-open.delegate="myFunction($event.detail)">
      <input style="width: 100%;">
    </ak-autocomplete>
    
<br><br>

Notice the `k-on-` prefix for the autocomplete custom element and the `k-on-` prefix for the `open` event of the autocomplete control. The $event's detail property contains the original event raised by the Kendo control. In order to pass this to the `myFunction` function directly, we use `$event.detail` in the view directly.

<br>
<br>

### 5.9.4 When to .bind and not to .bind
Using Aurelia's `.bind` syntax on a property allows it to do two things: binding to a variable and type parsing.
<br>

If you would use `k-visible="true"` without `.bind`, the `k-visible` property will contain the value of "true" (as string, not as boolean).
<br>

By adding `.bind` after the property name, it is parsed by Aurelia's binding syntax as a boolean, which is what we need. `k-visible.bind="true"` is the correct syntax. So for integers, strings and objects, and function calls use `.bind`.
<br><br>

A couple of examples:

	k-title="my string title"
	k-visible.bind="true"
	k-max-integer-value.bind="15"
	k-type.bind="{ name: 'a' }"
	k-title.bind="aPropertyOnMyViewModel"
    k-mask.bind="getMask()"
	k-my-array-value.bind="['a', 'b']"
<br>
<br>
### 5.9.5 How to get to the Kendo control
Every wrapper exports a `k-widget` property that you can two-way bind to.

      <ak-autocomplete k-data-source.bind="items" k-widget.bind="autocomplete">
        <input style="width: 100%;">
      </ak-autocomplete>
<br><br>

You can then use the `autocomplete` variable to communicate with the Kendo widget.

**IMPORTANT**: **k-widget bound variables cannot be used from bind() or attached() as the Kendo control gets initialized after attached().**

Fortunately there are other ways to use the widget after it gets initialized. We will list a few below:
<br><br>

**k-on-ready event**

All aurelia-kendoui-bridge custom elements and custom attributes raises the `k-on-ready` DOM event as soon as the Kendo control has been initialized.

It can be used as follows:
```
<input ak-datetimepicker k-on-ready.delegate="onReady($event.detail)" style="width: 100%" />

onReady(datePicker) {
  datePicker.value(new Date(1994,4,2));
}
```

**setTimeout**

You can delay the execution of code in bind() or attached() so that when the code is executed the control will be available:
```
<button ak-button="k-widget.bind: button">My button</button>

attached() {
  setTimeout(() => {
    this.button.enable(false);
  }, 50);
}
```


**TaskQueue**  

Aurelia's TaskQueue can be used to delay execution of code until all tasks on the task queue (binding included) has finished:

    <button ak-button="k-widget.bind: button">My button</button>

	import {TaskQueue, inject} from 'aurelia-framework';

	@inject(TaskQueue)
	export class Sample {
		constructor(taskQueue) {
			this.taskQueue = taskQueue;
		}

		attached() {
			this.taskQueue.queueTask(() => {
			  this.button.enable(false);
			});
		}
	}


<br>
<br>

### 5.9.6 Two-way binding
Kendo controls do not support two-way binding, as Kendo controls are not monitoring properties for changes. So when you two-way bind to a property, the changes will not be picked up by the control and will have no effect. There are two ways to deal with this issue:

- call API functions on the control
- recreate the control after changing a property

** Calling API functions on the control**  

Lets say that you wanted to change the title of the Kendo Window. The k-title property does not support two-way binding but we can call a function on the Kendo control to change the title. We know that we can because the [kendo documentation](http://docs.telerik.com/kendo-ui/api/javascript/ui/window#methods) of the Window mentions a `title` function.

So the following will change the window title:
```
<div ak-window="k-widget.two-way: myWindow"></div>

changeTitle() {
   this.myWindow.title('Hello world');
}
```

** Recreate the control** 
 
Try to avoid this as much as possible, by using an API function (see above) if it exists. There is a recreate function on the **wrapper** (not the Kendo control) that recreates the Kendo control. 

```
<div ak-window="k-title.bind: title" ak-window.ref="myWindowVM></div>

changeTitle() {
  this.title = 'a title';
  // give aurelia's binding system time to persist the change to the ak-window wrapper
  setTimeout(() => {
    this.myWindowVM.recreate();
  }});
}
```

 
**Exceptions**
  
In order to make your life a bit easier we have added support for two-way binding on a few properties that are commonly used. The following properties support two-way binding:

- k-value
- k-enabled
- k-readonly

***
***
