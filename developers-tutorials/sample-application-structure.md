_Application Developer tutorials_
# 4.2 Sample application structure

All details about each sample application exist in the application's own README.md file in the **[skeleton-navigation](https://github.com/aurelia/skeleton-navigation)** repository, so this document contains additional instructions needed to add the **[Kendo SDK](http://www.telerik.com/download/kendo-ui-core)** and **[Aurelia-KendoUI bridge](https://www.npmjs.com/package/aurelia-kendoui-bridge)** to our sample app. The **[Installation chapter](http://aurelia-ui-toolkits.github.io/demo-kendo/#/installation)** of the Aurelia KendoUI bridge **[catalog](http://aurelia-ui-toolkits.github.io/demo-kendo)** application remains the authoritative source on all information used in the tutorial.

As stated in the **[Introduction](./41_introduction.html)** section we are showing how to add the Autocomplete component from the free KendoUI Core SDK. Adding this component consists of:

1. Adding KendoUI SDK using **[`npm`](https://github.com/angular/angular.js/blob/master/src/ng/browser.js)** commands in the console (_this is not true across all tutorial articles - **x, y, and z** are execeptions_

2. Modifying existing configuration and source files

3. Adding the `autocomplete` "widget" defined as Aurelia custom component with it's three files:
 - autocomplete.html
 - autocomplete.js
 - autocomplete.css

We will use the "colored agenda" below to tag the "categories" of files in each of the subsequent tutorials starting with **[esnext - kendo](./42_skeleton_esnext.html)**

<br>

<p align=center>
  <img src="https://cloud.githubusercontent.com/assets/2712405/21949515/055e7698-d9c1-11e6-9752-4c65b6fb6f44.png"></img>
</p>

***


 
 