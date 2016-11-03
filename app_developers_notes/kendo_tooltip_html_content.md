# 5.5 Kendo Tooltip HTML content

The example illustrating the use of Tooltip with a template behaves differently than most KendoUI Aurelia components, as stated in the Documentation tab on **[this page](http://aurelia-ui-toolkits.github.io/demo-kendo/#/samples/tooltip/content-template)**

_The tooltip does not compile templates like normal controls, so for this particular sample we recommend to use Kendo templates instead_

This was recently mentioned in our support discussions as:

"When I use aurelia repeat syntax in `<script ref="template" type="text/x-kendo-template">` it does not do anything. Is there any way to build dynamic template based on HTML, not via javascript?"

Example
```
<script ref="template" type="text/x-kendo-template">
        <div style="margin-bottom: 20px;">
            <span>Filter</span>
        </div>
        <div style="padding-left: 20px;">
            <div class="form-checkboxes-list-container">
                <span>${filter.orderStates.length}</span> <!--WORKS HERE-->
                <ul id="recipe-status-list">
                    <li repeat.for="state of filter.orderStates"> <!--NOT WORK HERE-->
                        <input type="checkbox" id="checkbox_${state}" name="checkbox_${state}" value.bind="${state}" checked.bind="$parent.selectedStates"/>
                        <label for="checkbox_${state}"><span></span>${state}</label>
                    </li>
                </ul>
            </div>
            <div>
                <select style="width: 100%;" value.bind="selectedRecipe">
                    <option></option>
                    <option repeat.for ="recipe of filter.recipes" model.bind="recipe"></option>
                </select>
            </div>
            <div>
                <button>Filter</button>
                <button>Clear</button>
            </div>
        </div>
</script>
```

#### Here is the explanation of what is happening behind the scenes:


By default the template compilation process is trigged by the Kendo control via a certain hook. Most controls calls this hook and aurelia templates will be compiled correctly. The problem is that some controls don't call this hook. Kendo templates would still work, but Aurelia templates won't.

The tooltip is one of those controls that doesn't call the hook. So templates have to be compiled manually, if you want to take that route. The problem is in the timing, how would you know when the Kendo control has added a template to the DOM so that you can trigger the compilation process? This is tricky, and is different per Kendo control. For example, the grid has a dataBound event that notifies you when the rows have been rendered, but other controls may not have such event.

In my opinion, the best way would be to fix this at the source, by creating a PR to the [kendo core repository](https://github.com/telerik/kendo-ui-core) so that the tooltip calls the angular hook (which we use to start compiling aurelia templates)

***
***
