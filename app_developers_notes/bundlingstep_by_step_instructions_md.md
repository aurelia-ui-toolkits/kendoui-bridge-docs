_Application Developer notes_
# 5.8 Bundling step by step instructions
This set of instructions will guide you through setting up bundling in an Aurelia application where Kendo is used. Since there are multiple skeleton-navigation flavors out there, this tutorial will use the **[skeleton-typescript](https://github.com/aurelia/skeleton-navigation/tree/master/skeleton-typescript)** flavor. So if you want to follow this tutorial, download skeleton-typescript and follow **[these instructions](https://aurelia-ui-toolkits.gitbooks.io/kendoui-sdk-installation/content/jspm_based-installation/kendoui_pro/using_%60vendors%60_folder.html)** to install Kendo and the aurelia-kendoui-bridge into the application.

##### The completed version of this tutorial can be found [here](https://github.com/aurelia-ui-toolkits/skeleton-navigation-typescript-kendo-bundled) and the application's **[README](https://github.com/aurelia-ui-toolkits/skeleton-navigation-typescript-kendo-bundled/blob/master/README.md)** file covers bundling issues in even more details, so make sure to check that document.

***

1. Open `build/bundles.js`
2. Add the following bundle configuration (to get a Kendo bundle):
  ```
  "dist/kendo-build": {
      "includes": ["kendo-ui/js/*.min.js"],
      "excludes": [
        "[kendo-ui/js/angular.min.js]",
        "[kendo-ui/js/jquery.min.js]",
        "[kendo-ui/js/kendo.angular.min.js]",
        "[kendo-ui/js/kendo.angular2.min.js]",
        "[kendo-ui/js/kendo.spreadsheet.min.js]",
        "[kendo-ui/js/kendo.all.min.js]",
        "[kendo-ui/js/kendo.web.min.js]",
        "[kendo-ui/js/kendo.dataviz.min.js]",
        "[kendo-ui/js/kendo.dataviz.mobile.min.js]",
        "[kendo-ui/js/kendo.mobile.min.js]"
      ],
      "options": {
        "inject": true,
        "minify": true,
        "rev": true
      }
    }
    ```
3. Modify the `"dist/app-build"` bundle configuration to include the files of the aurelia-kendoui-bridge:
  ```
    "dist/app-build": {
      "includes": [
        "[**/*.js]",
        "**/*.html!text",
        "**/*.css!text"
      ],
      "options": {
        "inject": true,
        "minify": true,
        "depCache": true,
        "rev": false
      }
    }
  ```
  The `bundles.js` file should now look like **[this](https://github.com/aurelia-ui-toolkits/skeleton-navigation-typescript-kendo-bundled/blob/master/build/bundles.js)**.
  
4. Run `gulp bundle`. **This may take several minutes to complete**
   ![img](http://i.imgur.com/UbvfzsS.png)
5. In order to run the app without gulp modifying the `dist` folder, we can use `http-server`. Run the following command to install it: `npm install http-server -g`
6. Now run `http-server`: ![img](http://i.imgur.com/5v7dJRO.png)
7. Open http://localhost:8080 in your browser and notice that significantly less files are loaded:
![img](http://i.imgur.com/x8oVhr0.png)

***

### Important note

You can find this complete application as a **[companion document](https://github.com/aurelia-ui-toolkits/skeleton-navigation-typescript-kendo-bundled)** to this book. .


***