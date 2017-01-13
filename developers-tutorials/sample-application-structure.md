_Application Developer tutorials_
# 4.2 Sample application structure

All details about each sample application exist in the application's own README.md file in the **[skeleton-navigation](https://github.com/aurelia/skeleton-navigation)** repository, so this document contains additional instructions needed to add the **[Kendo SDK](http://www.telerik.com/download/kendo-ui-core)** and **[Aurelia-KendoUI bridge](https://www.npmjs.com/package/aurelia-kendoui-bridge)** to our sample app. The **[Installation chapter](http://aurelia-ui-toolkits.github.io/demo-kendo/#/installation)** of the Aurelia KendoUI bridge **[catalog](http://aurelia-ui-toolkits.github.io/demo-kendo)** application remains the authoritative source on all information used in the tutorial.

As stated in the **[Introduction](./41_introduction.html)** section we are showing how to add the Autocomplete component from the free KendoUI Core SDK. Adding this component consists of:

1. Adding KendoUI SDK using **[`npm`](https://github.com/angular/angular.js/blob/master/src/ng/browser.js)** commands in the console

2. Modifying existing configuration and source files

3. Adding the `autocomplete` "widget" defined as Aurelia custom component with it's three files:
 - autocomplete.html
 - autocomplete.js
 - autocomplete.css

We should use the "colored agenda" below to tag the files in each of the subsequent tutorials starting with **[esnext - kendo](./developers-tutorials/42_skeleton_esnext.html)**

<br>

<p align=center>
  <img src="https://cloud.githubusercontent.com/assets/2712405/21949374/aa860d36-d9bf-11e6-9a18-c30a1b7b4578.png"></img>
 <br><br>
Image 3
</p>

 
 