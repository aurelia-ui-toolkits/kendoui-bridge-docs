# 5.11 Prevent JSPM from loading jQuery

All Aurelia skeletons have Bootstrap installed. Bootstrap has a dependency on jQuery, so when bootstrap gets loaded JSPM will load jQuery as well. If you are loading Kendo from a `<script>` tag, then you need to load jQuery via a `<script>` tag first. This causes an issue where jQuery gets loaded twice (once via  the script tag and once via JSPM). This may cause unexpected things to happen.

In order to resolve this, you need to do two things: override the jQuery dependency of bootstrap and map `jquery` to `@empty`.

1. run this command: `jspm install bootstrap@3.3.6 -o "{ dependencies: {} }"`
  This adds the following override to package.json:
  ```
  "overrides": {
    "github:twbs/bootstrap@3.3.6": {
      "dependencies": {}
    }
  }
  ```
2. Add the following line to the `map` section of `config.js`  
   `"jquery": "@empty",`
   
***
***