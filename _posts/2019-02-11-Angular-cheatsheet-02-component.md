---
layout: post
title: Angular Cheatsheet 02 - Components
date: 2019-02-11
excerpt: ""
tags: [Angular, Component]
comments: true
---

# Overview

"my-list" component results in new 'src/app/my-list' component folder containing:
- my-list.component.html: (component) template
- my-list.component.spec.ts: (component) test specification
- my-list.component.ts: component (class)

Each component must be imported and declared in app.module.ts:
```typescript
...
import { MyListComponent } from './my-list/my-list.component';

@NgModule({
  declarations: [
    AppComponent,
    MyListComponent
  ...
```

# Generate new component my-list

```console
# new component my-list
ng g c my-list
```

Have a look at the changes on this [commit on GitHub](https://github.com/taitruong/angular-examples/commit/5ec3abbdf3044a0d3cccb2fe7c7ccb3ae8df5a17):


Component class defined in my-list.component.ts:
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'tai-my-list',
  templateUrl: './my-list.component.html',
  styles: []
})
export class MyListComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```
nb: selector starts with 'tai' because ng project is created with prefix 'tai'.

# [@Component](https://angular.io/api/core/Component) decorator

Overview:
- decorates a class as a component
- configures [options](https://angular.io/api/core/Component#options) (metadata) like (CSS) selector and template
- for selectors it is good practice using element selectors only (and not e.g. attribute or class selectors)

# Selector 'tai-my-list'
- Selector defines host element
- host element can be used in html (templates)
- option is inherited from [directive decorator](https://angular.io/api/core/Directive#selector)
- usage: add an element in any template (e.g. app.component.html):

```html
<tai-my-list></tai-my-list>
```

- element refers to component template my-list.component.html:

```html
<p>
  my-list works!
</p>
```

# Selector Example
- component with selector name 'tai-my-list'
- use my-list component e.g. in app root template (app.component.ts):

```
<tai-my-list></tai-my-list>
```

- start 'ng serve', you can inspect the source on the browser:

<img src="{{site.baseurl}}/assets/img/2019-02-11-component html source.png">

Outcome
- component template is added underneath my-list element:

```html
<html lang="en">
...
<body>
    <tai-root ng-version="7.2.4">
        <tai-my-list>
            <p> my-list works!
            </p>
        </tai-my-list>
    </tai-root>
...
</body>

</html>
```

# Template my-list.component.html

- defined in option of component decorator
  - [template](https://angular.io/api/core/Component#template): option for inline template
  - [templateUrl](https://angular.io/api/core/Component#templateurl): path to template file
- component has exactly one template (one-to-one mapping)

- data communication between component and template via bindings:

<img src="{{site.baseurl}}/assets/img/2019-02-11-angular-architecture.png">
Source:[angular.io>docs>fundamentals>architecture>architecture overview](https://angular.io/guide/architecture)

# Component styles

- defined in option of component decorator
 - [styles](https://angular.io/api/core/Component#styles): inline styles
 - [styleUrls](https://angular.io/api/core/Component#styleurls): path to one or more stylesheets

## Inline styles

my-list.component.ts:
```typescript
...
@Component({
...
  styles: [
    'p { color:blue }'
  ]
})
export class MyListComponent implements OnInit { ... }
```

## Style files

my-list.component.ts:
```typescript
...
@Component({
...
  styleUrls: ['./stylesheet1.css']
})
export class MyListComponent implements OnInit { ... }
```

NOTE:
- all files are in the component folder
- define one file for one purpose.

As the [Angular Style Guide](https://angular.io/guide/styleguide#single-responsibility) says about 'Rule of One' as part of Single Responsibility principle:
>Apply the single responsibility principle (SRP) to all components, services, and other symbols. This helps make the app cleaner, easier to read and maintain, and more testable.
>
> Rule of One
>
> Do define one thing, such as a service or component, per file.
>
> Consider limiting files to 400 lines of code.
>
>Why? One component per file makes it far easier to read, maintain, and avoid collisions with teams in source control.
>
>Why? One component per file avoids hidden bugs that often arise when combining components in a file where they may share variables, create unwanted closures, or unwanted coupling with dependencies.
>
>Why? A single component can be the default export for its file which facilitates lazy loading with the router.

