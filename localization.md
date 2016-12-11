# 5.10 Localization

## **JSPM**

In order to load a Kendo translation file, the following import statements can be added to `main.js:`

```
import 'kendo.core.min';
import 'kendo-ui/js/messages/kendo.messages.nl-NL.min';
import 'kendo-ui/js/cultures/kendo.culture.nl-NL.min';
```

`kendo.core.min is imported to ensure that the kendo global is available for the localization files to attach to.`

This by itself has no effect and Kendo will still use the default en-US localization. In main.js you also have to call \`kendo.culture\('nl-NL'\);\` for Kendo to switch the localization to \`nl-NL\`.

---

---

## **Aurelia-CLI**

Example configuration for aurelia-cli:

```js
import 'kendo-ui-core/js/kendo.core';
import 'kendo-ui-core/js/cultures/kendo.culture.nl-NL';
import environment from './environment';

//Configure Bluebird Promises.
Promise.config({
  longStackTraces: environment.debug,
  warnings: {
    wForgottenReturn: false
  }
});

export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .feature('resources');

  if (environment.debug) {
    aurelia.use.developmentLogging();
  }

  if (environment.testing) {
    aurelia.use.plugin('aurelia-testing');
  }

  kendo.culture('nl-NL');

  aurelia.start().then(() => aurelia.setRoot());
}
```

Note that kendo.core gets loaded first \(so that the kendo global is registered\), and kendo.culture.nl-NL is loaded afterwards. This by itself has no effect, until you tell kendo that the default culture should be \`nl-NL\` as well. This is done with the line \`kendo.culture\('nl-NL'\);\`

