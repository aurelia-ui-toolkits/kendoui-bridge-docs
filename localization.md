# Localization
In order to load a Kendo translation file, the following import statements can be added to `main.js`:

```
import 'kendo.core.min';
import 'kendo-ui/js/messages/kendo.messages.nl-NL.min';
import 'kendo-ui/js/cultures/kendo.culture.nl-NL.min';
```

`kendo.core.min` is imported to ensure that the `kendo` global is available for the localization files to attach to.