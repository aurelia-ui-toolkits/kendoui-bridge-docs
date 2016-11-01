# Dynamic content creation

Adding dynamic elements to an Aurelia app and the Kendo UI bridge can qualify as one of the requisites for building a large Desktop like GUI or LOB (Line of Business) application. This can either be to migrate an existing application or to create a new one.

Please be aware that "dynamic" should mean the equivalent of adding a DOM node to a HTML page but in this particular case the DOM node consist of Aurelia and Kendo UI Bridge components.

## Creating dynamic tabs

For the following example consider the following use-case. An application has a main menu and a central zone which consists of tabbed panels. When the user selects a menu entry from the menu, a new tab is created and the associated component (as in Aurelia component) is loaded. This would allow for an application both to modularize itself and to load only the needed components. Please find the complete [gist.run](https://gist.run/?id=b3df9d740a3cf3d31098c83c9ffe8614https://gist.run/?id=b3df9d740a3cf3d31098c83c9ffe8614) here and some short explanations bellow.

### The menu entries

Passing data with a menu entry can be done using something similar to:

```HTML
<span class="d-menuitem" data-id="m1">First module</span>
```

The data-id contains the name of the component and the text the title of the tab.

### Event handler

When the menu entry is selected the name and text are retrieved:

```javascript
let menuId = $(e.item).children(".k-link").children(".d-menuitem").attr("data-id");
let menuName = $(e.item).children(".k-link").text();
```

### The "magic"

The magic is derived from the `<compose>` Aurelia component. The [code](https://github.com/aurelia/templating-resources/blob/75dcc209fafc441dfc637c5e4232a076f81a9dbc/dist/aurelia-templating-resources.js) of this component can be found on github.

To add a new tab, a new DOM node is created using basic js then the **CompositionEngine.compose** is called provided with a viewModel, the component name, and a slot to attach to, the newly created element. 

```javascript
this.tabStrip.append({
    text: '<span id="tabmodule' + menuId + '">' + menuName + '</span>',
    content: '<div id="module' + menuId + '"></div>',
    encoded: false
});

let el = document.getElementById('module' + menuId);
let vSlot = new ViewSlot(el, true);
this.instruction = Object.assign(this.instruction, {viewModel: menuId, host: el, viewSlot: vSlot});
this.compositionEngine.compose(this.instruction).then(controller => {
    vSlot.bind();
    vSlot.attached();
});
```

Please note that the last promise calls the bind and attached method which in turn call the *.recreate* method on all the Kendo UI Bridge components. This is **important**.