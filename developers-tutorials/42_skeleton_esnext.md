_Application Developer tutorials_
# esnext kendo

#### Summary
- Details on building and running this application are **[here](https://github.com/aurelia-ui-toolkits/kendo-tutorials.code-2.0/blob/master/skeleton-esnext/before/README.md)**.

- KendoUI Library (SDK) and KendoUI bridge installation is defined in **[this section of the Installation document](https://aurelia-ui-toolkits.gitbooks.io/kendo-ui-library-installation-version-2-0/content/library-installation/advanced-via-module-loader/jspm.html).** 


***

#### Details

The subsequent steps are applied to the copy of the **[original esnext sample](https://github.com/aurelia-ui-toolkits/kendo-tutorials.code-2.0/tree/master/skeleton-esnext/before)**, which is located in the **["after"](https://github.com/aurelia-ui-toolkits/kendo-tutorials.code-2.0/tree/master/skeleton-esnext/after)** folder, rebuilt and verified from scratch using the command

```
 npm install && jspm install && gulp watch
```

##### Step 1. 

Run the following command (see [JSPM based KendoUI Library installation](https://aurelia-ui-toolkits.gitbooks.io/kendo-ui-library-installation-version-2-0/content/library-installation/advanced-via-module-loader/jspm.html)) in the console:

```
jspm install css npm:@progress/kendo-ui aurelia-kendoui-bridge
```
<p align=center>
  <img src="https://user-images.githubusercontent.com/2712405/32817593-b1d8f572-c98c-11e7-9a1e-3f7a96b27e9e.png"></img>
 <br>
 Image 1 - installed KendoUI SDK using npm
</p>

##### Step 2.

Update `config.js`, _by adding the last line, pointed by the arrow._

```
paths: {
   "*": "dist/*",
   "github:*": "jspm_packages/github/*",
   "npm:*": "jspm_packages/npm/*",
   "kendo.*": "kendo.*": "jspm_packages/github/npm/progress/kendo-ui@2017.3.1026/js/kendo.*.js",
   "kendo-ui/*": "jspm_packages/npm/@progress/kendo-ui@2017.3.1026/*" 
}, 
```

 _Note that the version number shown above (2017.3.1026) is the latest available at the time of writing this document - you will likely have to change it._
 
##### Step 3.

Add the `autocomplete.js` file to the project.

```
import 'kendo-ui/js/kendo.autocomplete';

export class autocomplete{
  constructor() {
    this.datasource = {
      transport: {
        read: {
          dataType: 'jsonp',
          url: '//demos.telerik.com/kendo-ui/service/Customers'
        }
      }
    };
  }	
}
```

##### Step 4.

Add the `autocomplete.html` file

```
<template>
  <require from="aurelia-kendoui-bridge/autocomplete/autocomplete"></require>
  <require from="aurelia-kendoui-bridge/common/template"></require>
  <require from="kendo-ui/css/web/kendo.common.min.css!"></require>
  <require from="kendo-ui/css/web/kendo.bootstrap.min.css!"></require>
  <require from="./autocomplete.css"></require>

  <div id="example" style="margin-top: 20px; margin-left: 20px">
    <div class="demo-section k-content">
      <p><strong>Customers:</strong></p>

      <ak-autocomplete k-data-source.bind="datasource"
                      k-height.bind="400"
                      k-min-length.bind="1"
                      k-data-text-field="ContactName">
        <ak-template>
          <span class="k-state-default" style="background-image: url('http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg')"></span>
          <span class="k-state-default"><h3>${ContactName}</h3><p>${CompanyName}</p></span>
        </ak-template>
        <ak-template for='headerTemplate'>
          <div class="dropdown-header k-widget k-header">
            <span>Photo</span>
            <span>Contact info</span>
          </div>
        </ak-template>

        <input id="autocomplete-customizing-templates-customers" style="width: 50%"/>
      </ak-autocomplete>

      <p class="demo-hint">Start typing to find a customer. E.g. "Ann" </p>
    </div>
  </div>
</template>
```


##### Step 5.

Add the `autocomplete.css` file to the project

```
.dropdown-header {
        border-width: 0 0 1px 0;
        text-transform: uppercase;
    }

    .dropdown-header > span {
        display: inline-block;
        padding: 10px;
    }

    .dropdown-header > span:first-child {
        width: 50px;
    }

    .k-item {
        line-height: 1em;
        min-width: 300px;
    }

    /* Material Theme padding adjustment*/

    .k-material .k-item,
    .k-material .k-item.k-state-hover,
    .k-materialblack .k-item,
    .k-materialblack .k-item.k-state-hover {
        padding-left: 5px;
        border-left: 0;
    }

    .k-item > span {
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        box-sizing: border-box;
        display: inline-block;
        vertical-align: top;
        margin: 20px 10px 10px 5px;
    }

    .k-item > span:first-child {
        -moz-box-shadow: inset 0 0 30px rgba(0,0,0,.3);
        -webkit-box-shadow: inset 0 0 30px rgba(0,0,0,.3);
        box-shadow: inset 0 0 30px rgba(0,0,0,.3);
        margin: 10px;
        width: 50px;
        height: 50px;
        border-radius: 50%;
        background-size: 100%;
        background-repeat: no-repeat;
    }

    h3 {
        font-size: 1.2em;
        font-weight: normal;
        margin: 0 0 1px 0;
        padding: 0;
    }

    p {
        margin: 0;
        padding: 0;
        font-size: .8em;
    }

```

##### Step 6.

Add the request to load the aurelia-kendoui-bridge plugin by adding the highlighted statement below (`.plugin('aurelia-kendoui-bridge');` to the file `main.js`

```
import 'bootstrap';

export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging()
    .plugin('aurelia-kendoui-bridge');

  aurelia.start().then(() => aurelia.setRoot());
}
```

##### Step 7.

Add the following line to `app.js`
    
```
    { route: 'autocomplete',  name: 'autocomplete', moduleId: 'autocomplete', nav: true, title: 'Autocomplete' }
```
    
***






