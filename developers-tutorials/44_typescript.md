_Application Developer tutorials_
# typescript kendo

#### Summary
- Details on building and running this applications are **[here](https://github.com/aurelia/skeleton-navigation/blob/master/skeleton-typescript/README.md)**.

- Kendo Core and KendoUI bridge installation is defined in **[this section of the Installation document](https://aurelia-ui-toolkits.gitbooks.io/kendo-ui-sdk-installation/content/installation/installing%20kendo/advanced/core/jspm.html).** 




***

#### Details

The subsequent steps are applied to the copy of the **[original typescript sample](https://github.com/aurelia-ui-toolkits/kendo-tutorials.code-2.0/tree/master/skeleton-typescript/before)**, which is located in the **["after"](https://github.com/aurelia-ui-toolkits/kendo-tutorials.code-2.0/tree/master/skeleton-typescript/after)** folder, rebuilt and verified from scratch using the command

```
 npm install && jspm install && gulp watch
```


##### Step 1.

Run the following command in the console (note the format **`npm:@progress/kendo-ui`**):

   ```
   jspm install css npm:@progress/kendo-ui aurelia-kendoui-bridge

   ```

##### Step 2.

Update **`config.js`** by adding the last line, pointed by the arrow.
 ```
    paths: {
       "*": "dist/*",
       "github:*": "jspm_packages/github/*",
       "npm:*": "jspm_packages/npm/*",
       "kendo.*": "jspm_packages/github/kendo-labs/bower-kendo-ui@2016.3.1306/js/kendo.*.js" <----
    },
 ```
 
##### Step 3.

Run the following command in the console:

  ```
  npm install @types/kendo-ui
  ```
  
**Note**: since Typescript 2.0, the command below is **[deprecated](https://github.com/typings/typings#deprecation-notice-regarding-typescript20)**, although it may still work. Do not run this, if `npm install @types/kendo-ui` is sufficient to make your application run correctly

 ```
 typings install kendo-ui --source=dt --global
 ```
 
 
##### Step 4. 

Add this **`autocomplete.ts`* file to the project

```
import 'kendo-ui/js/kendo.autocomplete.min';

export class autocomplete{
  private datasource: kendo.data.DataSource;

  constructor() {
    this.datasource = new kendo.data.DataSource({
      transport: {
        read: {
          dataType: 'jsonp',
          url: '//demos.telerik.com/kendo-ui/service/Customers'
        }
      }
    });
  }	
}
```

##### Step 5.

Add this **`autocomplete.html`** file to the project

```
template>
  <require from="aurelia-kendoui-bridge/autocomplete/autocomplete"></require>
  <require from="aurelia-kendoui-bridge/common/template"></require>
  <require from="kendo-ui/styles/kendo.common.min.css!"></require>
  <require from="kendo-ui/styles/kendo.bootstrap.min.css!"></require>
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

##### Step 6.

Add this **`autocomplete.css`** file to the project
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

##### Step 7.

Add the request to load the aurelia-kendoui-bridge plugin by adding the statement **`.plugin('aurelia-kendoui-bridge';`** to the file **`main.js`**

```
import 'bootstrap';
import {Aurelia} from 'aurelia-framework';

export function configure(aurelia: Aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging()
    .plugin('aurelia-kendoui-bridge');   <----

  aurelia.start().then(() => aurelia.setRoot());
}
```

##### Step 8.

Add the following line to `app.js`
    
```
{ route: 'autocomplete',  name: 'autocomplete', moduleId: 'autocomplete', nav: true, title: 'Autocomplete' }
```
    
***

### Important note
You can find this complete application as a **[companion document](https://github.com/aurelia-ui-toolkits/skeleton-navigation-typescript-kendo-bundled)** to this book. The application's **[README](https://github.com/aurelia-ui-toolkits/skeleton-navigation-typescript-kendo-bundled/blob/master/README.md)** file covers some sections in even more details, so make sure to check this document.

***











