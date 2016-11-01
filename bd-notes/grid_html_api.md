# 6.5 Grid HTML api


#### How is the KendoUI Grid component being used?
<br>
Before we can explain how it works, we must first look at how it is used.

The Kendo Grid can be used and configured via the following section of HTML code - (process known as declarative programming):
<br>
```html
    <k-grid k-data-source.bind="datasource" k-pageable.bind="pageable" k-sortable.bind="true">
      <k-col k-title="Contact Name" k-field="ContactName">
        <k-template for="template">
			<div class='customer-photo' style="background-image: url(http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg);"></div>
        	<div class='customer-name'>${ContactName}</div>
		</k-template>
      </k-col>
      <k-col k-title="Contact Name" k-field="ContactName"></k-col>
      <k-col k-title="Contact Title" k-field="ContactTitle"></k-col>
      <k-col k-title="Company Name" k-field="CompanyName"></k-col>
      <k-col k-field="Country"></k-col>
    </k-grid>
```
<br>

This code is the **[Basic use](#/samples/grid/basic-use)** sample, which is a part of the larger set of **[Button component samples](#/samples/button)** shown in the **[Aurelia KendoUI catalog](#/samples)**.

<br>
<br>
**What are the characteristics of this sample, that require this whole article to explain them?**

 &nbsp; &nbsp; - We see three custom elements: `k-grid` and `k-col`. The `k-col` custom elements are inside of the `k-grid` custom element, making the `k-col` custom elements "children" of the `k-grid` custom element. The `k-template` custom element is a child element of the `k-col`.
<br>

 &nbsp; &nbsp; - All three custom elements have some `@bindable` properties available, such as `k-data-source`, `k-title`, `k-command` and `for`. A bindable property is a property inside a view-model marked with the `@bindable` decorator. The `title` property is defined as `@bindable kTitle`. Because of the `@bindable` decorator, we can now set these properties from HTML: `<k-grid k-title="my value"></k-grid>`, and respond to changes of these properties when needed.
<br>

The `k-template` custom element is a bit special, as it has a template defined within. Let's look at this more closely:

```html
	<k-template for="template">
		<div class='customer-photo' style="background-image: url(http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg);"></div>
		<div class='customer-name'>${ContactName}</div>
	</k-template>
```
<br>

This `k-col` has two properties: **`for`** and **`template`**, where the **`for`** is easy to spot, but the **`template`** is somewhat hidden. The `template` property is extracted by the `<k-template>` custom element from its inner content (innerHTML).
<br><br>

#### How does the k-template extract the template?

Let's take a look at the code of the [k-template custom element](https://github.com/aurelia-ui-toolkits/aurelia-kendoui-bridge/blob/master/src/common/k-template.js).
<br>

We use the `@bindable` properties, to configure them via attributes on the custom element. If we use `<k-template for="template">`, then Aurelia sets the `for` property on the `k-template` view-model to "template".
<br>

Now, how does it extract the template? This magic happens inside of the `@processContent` decorator.
<br>

Aurelia's docs explain the @processContent decorator as follows:

&nbsp; &nbsp; &nbsp; &nbsp;The @processContent tells the compiler that the element's content requires special processing.
<br><br>

In other words, the `@processContent` decorator allows us to process the content of the custom element before anything else has happened to it. The implementation looks like this:
<br>
``` javascript

	@processContent((compiler, resources, element, instruction) => {
	  let html = element.innerHTML;
	  if (html !== '') {
	    instruction.template = html;
	  }
	  return true;
	})
```
<br>

The `@processContent` decorator passes us a few arguments we can use inside of the callback function. The things we care about are `element` and `instruction`. The `element` is the HTML tag of our custom-element, so in this case, `<k-template>`. It's the same thing as when you `@inject(Element)`. The `instruction` is a [BehaviorInstruction](http://aurelia.io/docs.html#/aurelia/templating/1.0.0-beta.1.0.1/doc/api/class/BehaviorInstruction). All you need to know is that this object is shared by both the `@processContent` and the actual view-model (`Template`). So, this means that we can **define properties** on this object from within the `@processContent` function, and access it from inside the view-model.
<br>

In our implementation of the `@processContent` function, we take the `innerHTML` of the `<k-template>` element, and put this in the `template` property of the `instruction`. We could have chosen any property name, but we chose the name **`template`**.
<br><br>

Now, in order for us to pull this `template` property of the [`instruction`](#instruction) inside the view-model, we need to inject the `TargetInstruction`, as shown below:
<br>

```javascript
	@inject(TargetInstruction)
	export class Template {
	  @bindable template;
	  @bindable for = 'template';
	
	  constructor(targetInstruction) {
	    this.template = targetInstruction.elementInstruction.template;
	  }
	}
```
<br>

The `targetInstruction` has an `elementInstruction` atttribute which contains the `template` we set inside the `@processContent` function. So now we can use just HTML to define all the column properties we need to pass onto Kendo's grid control! Great!
<br>

Now before we go on, let's take a look at `return true;` at the end of the `@processContent` function. What does this do? It tells Aurelia that we have handled content processing, and that no further processing should be done. This effectively removes the content of the custom element, and this is exactly what we want, because we don't want to see the template on the screen. We just want to store the template as a property of the AuClass, so we can pass this onto Kendo's grid.
<br><br>

#### How does the `k-template` element get to the properties of the `k-col` elements?

Let's take a step back, and look at the usage of the Grid control one more time:
<br>

```html
    <k-grid k-data-source.bind="datasource" k-pageable.bind="pageable" k-sortable.bind="true">
      <k-col k-title="Contact Name" k-field="ContactName">
        <k-template for="template">
			<div class='customer-photo' style="background-image: url(http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg);"></div>
        	<div class='customer-name'>${ContactName}</div>
		</k-template>
      </k-col>
      <k-col k-title="Contact Name" k-field="ContactName"></k-col>
      <k-col k-title="Contact Title" k-field="ContactTitle"></k-col>
      <k-col k-title="Company Name" k-field="CompanyName"></k-col>
      <k-col k-field="Country"></k-col>
    </k-grid>
```
<br>

We said that all the `k-col` custom elements are "children" of the `<k-grid>` custom element. What we want is to reach all view-models behind the `k-col` elements, from within the view-model of the `<k-grid>`. Aurelia's got us covered. We need to do something else first though.
<br>

Since the `k-grid` custom element now has content, it has become a view. For Aurelia to recognize this, in addition to the `grid.js` view-model, we need to create a `grid.html` file (see [Creating Components](http://aurelia.io/docs.html#/aurelia/framework/1.0.0-beta.1.0.3/doc/article/creating-components) article for more details). In this file, we define the following:
<br>
```html
	<template>
	  <content></content>
	</template>
```

The same applies for both `<k-col>` and `<k-template>`.
<br>

Notice the use of the `<content>` tag. This tells Aurelia to put the content of the `<k-grid>` tag, at that place in the view. More information about the `<content>` tag, can be found [here](http://patrickwalters.net/wielding-the-power-of-aurelia-content-selectors/).
<br>

Now we got this out of our way, we can get back to our problem. From inside the `Grid` view-model, we want to get the properties on the view-model's of all `<k-col>` custom elements. Luckily, Aurelia has made a nice decorator for this: `@children`. It is used as follows:
<br>

```html
	@customElement('k-grid')
	export class Grid {

	  @children('k-col') columns;

	}
```
<br>

That's it! The `Grid` class now has a `columns` property, containing all the view-models of all the `k-col` custom elements, as an array. The `'k-col'` bit is a css selector, so it is pretty powerful.
<br><br>

#### How are we communicating the column definitions to Kendo's Grid?

Now, if we would use Kendo Grid without Aurelia, the columns would be configured like this:
<br>

```javascript
	$("#grid").kendoGrid({
	  columns: [
	     {
	      title: "Contact Name",
	      field: "ContactName",
	      template: '<div class='customer-photo' style="background-image: url(http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg);"></div>
	        <div class='customer-name'>${ContactName}</div>'
	     },
	     {
	      title: "Contact Name",
	      field: "ContactName",
	     },
	     {
	      title: "Contact Title",
	      field: "ContactTitle",
	     },
	     {
	      title: "Company Name",
	      field: "CompanyName",
	     },
	     {
	      field: "Country",
	     }]
	});
```
<br>

If you remember, all these properties (title, field, template) are defined inside the view-model's of the `k-col` custom elements. Because we have the lovely `@children` decorator, all these view-models are available in the `columns` property of the `Grid`. So, we can just pass this array in when we initialize the grid!
<br>

```javascript
$("#grid").kendoGrid({
  columns: this.columns
});
```
<br>

And there we have it, Kendo's Grid now knows about the columns we want to see, configured purely via HTML.
<br><br>

#### High level overview

Let's take a look (at a high level) at the process.
<br>

The developer uses this piece of HTML code:
<br>

```html
    <k-grid k-data-source.bind="datasource" k-pageable.bind="pageable" k-sortable.bind="true">
      <k-col k-title="Contact Name" k-field="ContactName">
        <k-template for="template">
			<div class='customer-photo' style="background-image: url(http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg);"></div>
        	<div class='customer-name'>${ContactName}</div>
		</k-template>
      </k-col>
      <k-col k-title="Contact Name" k-field="ContactName"></k-col>
      <k-col k-title="Contact Title" k-field="ContactTitle"></k-col>
      <k-col k-title="Company Name" k-field="CompanyName"></k-col>
      <k-col k-field="Country"></k-col>
    </k-grid>
```
<br>

The `k-template` viewmodel uses `@processContent` to "extract" the template which can be found in the content of the `<k-template>` custom element, and it also stores the `for` property.
<br>

The `k-grid` custom element, uses `@children` to get an array of all `AuCol` instances, which contain the `k-title`, `k-field` and `template` properties.
<br>

When Kendo's Grid gets initialized, this array of `AuCol` instances is passed along. In essence, Kendo's Grid is now initialized as follows:
<br>

```javascript
	$("#grid").kendoGrid({
	  columns: [
	     {
	      title: "Contact Name",
	      field: "ContactName",
	      template: '<div class='customer-photo' style="background-image: url(http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg);"></div>
	        <div class='customer-name'>${ContactName}</div>'
	     },
	     {
	      title: "Contact Name",
	      field: "ContactName",
	     },
	     {
	      title: "Contact Title",
	      field: "ContactTitle",
	     },
	     {
	      title: "Company Name",
	      field: "CompanyName",
	     },
	     {
	      field: "Country",
	     }]
	});
```
<br>

Notice how we use Aurelia's binding syntax inside the template? How we compile this template is can be better explained in a different article.