---
layout: post
title: Angular Cheatsheet 01 - Angular project
date: 2019-02-11
excerpt: ""
tags: [Angular, angular project]
comments: true
---

# Overview
- an Angular app consists of one or more components
- app root/root component is the minimum component
- all other components are underneath root component
- component consists of
  - a (TypeScript) class, annotated with an [@Component](https://angular.io/api/core/Component) decorator
  - a view represented by a template

NB: components will be covered in my next angular cheatsheet blog.
More 
# App module, app component and bootstrapping
First lets create a new ng project 'Cheatsheet01Project', with no extra css files using inline-styles and an app prefix called "tai".

```console
# new ng project Cheatsheet01Project with inline-style (-s) and prefix tai (-p tai)
ng n Cheatsheet01Project -s -p tai
? Would you like to add Angular routing? No
? Which stylesheet format would you like to use? CSS
```

A module contains as a one or more components. As a minimum there is the app component (app.component.ts) defined in the main src/app folder.

The src/app folder contains the module app.module.ts. The app module configures and bootstraps our angular application. At the beginning only the app component is bootstrapped:

```
...
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
...
  bootstrap: [AppComponent]
})
export class AppModule { }
```

The bootstrapping process is triggered by the main.ts file:

```
...
if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
```

Now let's start it locally:
```
ng serve
** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **
 25% building 100/101 modules 1 active ...
Date: 2019-02-11T14:25:33.919Z
Hash: d6c08e62c5d3c1e14a6e
Time: 5079ms
chunk {es2015-polyfills} es2015-polyfills.js, es2015-polyfills.js.map (es2015-polyfills) 284 kB [initial] [rendered]
chunk {main} main.js, main.js.map (main) 9.32 kB [initial] [rendered]
chunk {polyfills} polyfills.js, polyfills.js.map (polyfills) 236 kB [initial] [rendered]
chunk {runtime} runtime.js, runtime.js.map (runtime) 6.08 kB [entry] [rendered]
chunk {styles} styles.js, styles.js.map (styles) 16.3 kB [initial] [rendered]
chunk {vendor} vendor.js, vendor.js.map (vendor) 3.51 MB [initial] [rendered]
ℹ ｢wdm｣: Compiled successfully.
```

# Folder structure
The project contains the following files and folders under Cheatsheet01Project:
- e2e: files for end-to-end tests
- node_modules: contains installed npm packages
- src: source folder for or 'tai'-app
  - app: contains modules, components, ...
    - app: main app files
      - app.component.html
      - app.component.spec.ts
      - app.module.ts
  - assets: media files
  - environments: like prod, dev
  index.html
  tslint.json: TSLint configuration
- angular.json: angular configuration
- tsconfig.json: global TypeScript configuration

# 'app' prefix -> 'tai'
Affecting changes using prefix "tai" in Cheatsheet01Project folder:
(please note without a prefix the default prefix would be "app")

angular.json
```
{
  "projects": {
    "Cheatsheet01Project": {
...
      "prefix": "tai", //default: "prefix": "app"
...
}Cheatsheet01Project
```

e2e/src/app.po.ts
```
import { browser, by, element } from 'protractor';

export class AppPage {
  navigateTo() {
    return browser.get(browser.baseUrl) as Promise<any>;
  }

  getTitleText() {
    return element(by.css('tai-root h1')).getText() as Promise<string>; //default: return element(by.css('app-root h1')).getText() as Promise<string>;
  }
}
```

src/app.component.ts
```
import { Component } from '@angular/core';

@Component({
  selector: 'tai-root', //default: 'app-root'
  templateUrl: './app.component.html',
  styles: []
})
export class AppComponent {
  title = 'Cheatsheet01Project';
}
```

src/index.html
```
...
<html lang="en">
...
<body>
  <tai-root></tai-root> //default: <app-root></app-root>
</body>
</html>
```

src/tslint.json
```
{
...
    "rules": {
        "directive-selector": [
...
            "tai", //default: app
...
        ],
        "component-selector": [
...
            "tai", //default: app
...
        ]
    }
}

```

That's it for now. Enjoy, Mr. T
