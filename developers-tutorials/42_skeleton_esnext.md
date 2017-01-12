# 4.2 esnext kendo



1. Kendo Core installation: https://aurelia-ui-toolkits.gitbooks.io/kendo-ui-sdk-installation/content/installation/installing%20kendo/advanced/core/jspm.html



2. \`jnpm install css kendo-ui aurelia-kendoui-bridge\`



3. Update \`config.js\`

    \`\`\`

    paths: {

       "\*": "dist/\*",

       "github:\*": "jspm\_packages/github/\*",

       "npm:\*": "jspm\_packages/npm/\*",

       "kendo.\*": "jspm\_packages/github/kendo-labs/bower-kendo-ui@2016.3.1306/js/kendo.\*.js" &lt;----

    },

     \`\`\`\`

4. Add the \`autocomplete.js\` file

    \`\`\`

    import 'kendo-ui/js/kendo.autocomplete.min';



    export class autocomplete{

      constructor\(\) {

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

    \`\`\`



5. Add the \`autocomplete.html\` file

    \`\`\`

    &lt;template&gt;

      &lt;require from="aurelia-kendoui-bridge/autocomplete/autocomplete"&gt;&lt;/require&gt;

      &lt;require from="aurelia-kendoui-bridge/common/template"&gt;&lt;/require&gt;

      &lt;require from="kendo-ui/styles/kendo.common.min.css!"&gt;&lt;/require&gt;

      &lt;require from="kendo-ui/styles/kendo.bootstrap.min.css!"&gt;&lt;/require&gt;

      &lt;require from="./autocomplete.css"&gt;&lt;/require&gt;



      &lt;div id="example" style="margin-top: 20px; margin-left: 20px"&gt;

        &lt;div class="demo-section k-content"&gt;

          &lt;p&gt;&lt;strong&gt;Customers:&lt;/strong&gt;&lt;/p&gt;



          &lt;ak-autocomplete k-data-source.bind="datasource"

                          k-height.bind="400"

                          k-min-length.bind="1"

                          k-data-text-field="ContactName"&gt;

            &lt;ak-template&gt;

              &lt;span class="k-state-default" style="background-image: url\('http://demos.telerik.com/kendo-ui/content/web/Customers/${CustomerID}.jpg'\)"&gt;&lt;/span&gt;

              &lt;span class="k-state-default"&gt;&lt;h3&gt;${ContactName}&lt;/h3&gt;&lt;p&gt;${CompanyName}&lt;/p&gt;&lt;/span&gt;

            &lt;/ak-template&gt;

            &lt;ak-template for='headerTemplate'&gt;

              &lt;div class="dropdown-header k-widget k-header"&gt;

                &lt;span&gt;Photo&lt;/span&gt;

                &lt;span&gt;Contact info&lt;/span&gt;

              &lt;/div&gt;

            &lt;/ak-template&gt;



            &lt;input id="autocomplete-customizing-templates-customers" style="width: 50%"/&gt;

          &lt;/ak-autocomplete&gt;



          &lt;p class="demo-hint"&gt;Start typing to find a customer. E.g. "Ann" &lt;/p&gt;

        &lt;/div&gt;

      &lt;/div&gt;

    &lt;/template&gt;

    \`\`\`



6. Add the \`autocomplete.css\` file

    \`\`\`

    .dropdown-header {

            border-width: 0 0 1px 0;

            text-transform: uppercase;

        }



        .dropdown-header &gt; span {

            display: inline-block;

            padding: 10px;

        }



        .dropdown-header &gt; span:first-child {

            width: 50px;

        }



        .k-item {

            line-height: 1em;

            min-width: 300px;

        }



        /\* Material Theme padding adjustment\*/



        .k-material .k-item,

        .k-material .k-item.k-state-hover,

        .k-materialblack .k-item,

        .k-materialblack .k-item.k-state-hover {

            padding-left: 5px;

            border-left: 0;

        }



        .k-item &gt; span {

            -webkit-box-sizing: border-box;

            -moz-box-sizing: border-box;

            box-sizing: border-box;

            display: inline-block;

            vertical-align: top;

            margin: 20px 10px 10px 5px;

        }



        .k-item &gt; span:first-child {

            -moz-box-shadow: inset 0 0 30px rgba\(0,0,0,.3\);

            -webkit-box-shadow: inset 0 0 30px rgba\(0,0,0,.3\);

            box-shadow: inset 0 0 30px rgba\(0,0,0,.3\);

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



    \`\`\`



7. Load the kendoui bridge \(add \`    .plugin\('aurelia-kendoui-bridge'\);\` in \`main.js\`

    \`\`\`

    \`\`\`



8. Add the following lines to \`autocomplete.html\`

    \`\`\`

    &lt;require from="aurelia-kendoui-bridge/autocomplete/autocomplete"&gt;&lt;/require&gt;

    &lt;require from="aurelia-kendoui-bridge/common/template"&gt;&lt;/require&gt;  

    \`\`\`

9. Add the following to the \`autocomplete.js\`



    \`\`\`

    import 'kendo-ui/js/kendo.autocomplete.min';

    \`\`\`

    as well as 

    \`\`\`\`

    { route: 'autocomplete',  name: 'autocomplete', moduleId: 'autocomplete', nav: true, title: 'Autocomplete' }



    \`\`\`\`















