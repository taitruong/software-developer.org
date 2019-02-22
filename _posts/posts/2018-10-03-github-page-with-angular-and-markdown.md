---
layout: post
category: post
title: A GitHub Page with Angular 6 and Markdown (NgxMd)
date: 2018-10-03 
excerpt: ""
tags: [Angular, GitHub pages]
comments: true
---
[Google translate this page e.g. to German](translate.google.com/translate?hl=en&sl=en&tl=de&u=https%3A%2F%2Ftaitruong.github.io%2Fsoftware-developer.org%2F2018%2F10%2F03%2Fgithub-page-with-angular-and-markdown%2F)

# Introduction
GitHub is great. It is a social coding platform. You can create repositories on their site. Mine for example is at [GitHub.com/taitruong](https://github.com/taitruong/). What's even greater: you can create GitHub pages. I use it for my blogs. In my other blog there is a [GitHub Pages how-to with Jekyll]({{site.baseurl}}/2017/11/17/blog-site-setup-using-github-pages-and-jekyll-theme/).

If you use GitHub pages with Angular 6 (or anything but Jekyll) it has one disadvantage or advantage - depending how you see it. The difference is that your GitHub repo is not in sync with your GitHub pages. For me this is an advantage: I can create a draft and push it to my remote repo. Once I am finished I publish it on my GitHub pages. If you want it automated then use Jekyll and let GitHub syncs it everytime when you push something to your repo.

NB: check the [demo](dimpu.github.io/ngx-md) from the creator of the NgxMd module. Especially the interactive [live view](https://dimpu.github.io/ngx-md/live) is cool :).

# Prerequisites

[Source: Install The Latest Node.Js And NMP Packages On Ubuntu 16.04 / 18.04 LTS](https://websiteforstudents.com/install-the-latest-node-js-and-nmp-packages-on-ubuntu-16-04-18-04-lts/)

You need to install the following:
- npm: comes with nodejs and is required for install packages like angular and ngx-md
- npm package: angular command line interface (cli)
- git: which is used by angular-cli

Install nodejs

```
# Optional: add this ppa for installing latest nodejs release
curl -sL https://deb.nodesource.com/setup_10.x
# installs latest (10.15.1) or LTS (8.10.0) release
sudo apt install nodejs
# check version
node -v # v10.15.1
```

Install angular globally (using '-g' option):
```
sudo npm install -g @angular/cli
```

Install and configure git:
```
sudo apt install git
git config --global user.name yourname
git config --global user.email your@email.address
```
# A plain Angular 6 project
Source: [angular.io quickstart guide](https://angular.io/guide/quickstart)

Give your project a name like 'angular-markdown' and create an ng project with a git repository:
```
ng new angular-markdown
cd angular-markdown/
# 'ng new' creates a local git repot with an initial commit
git log
> commit c580ac569c0a61deeb5ef07c5f38dc42b68eec4e (HEAD -> master)
> Author: yourname <your@email.address>
> Date:   Wed Oct 3 14:18:12 2018 +0000
>    initial commit
```

# Create a GitHub repo

Since you have a local repo all you need is to set an upstream to your remote repo on GitHub:
- First create a GitHub repo 'angular-markdown' [here](https://github.com/new).
- Configure your local git repo and link it to your remote repo on GitHub:

```
# add remote
git remote add origin https://github.com/taitruong/angular-markdown.git
# before pushing to github you need to set local branch to remote branch
git push --set-upstream origin master

```

# Publish your plain GitHub page

Source: [npmjs package 'angular-cli-ghpages'](https://www.npmjs.com/package/angular-cli-ghpages)
Install package angular-cli-ghpages and add it as a dev dependency to package.json (using --save-dev option):
```
npm i angular-cli-ghpages --save-dev
# git diff shows you that angular-cli-ghpages is added to package.json
git diff
```

Now build angular to your productive site:
```
# build to dist folder
ng build --prod --base-href "https://taitruong.github.io/angular-markdown/"
# deploy build from dist folder to github.io
npx ngh --dir=dist/angular-markdown
```

Now you can check your result on [taitruong.github.io/angular-markdown/](https://taitruong.github.io/angular-markdown/).

# Angular Markdown (NgxMd)

## Install and add angular-markdown module to project
Source: [npms package 'Angular Markdown (NgxMd)'](https://www.npmjs.com/package/ngx-md)

Let's get to the last step and include an angular-markdown module to your project.
Install ngx-md package and add dependency to package.json:
```
npm install ngx-md
```

edit src/app/app.module.ts and import ngx-md module:
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { NgxMdModule } from 'ngx-md';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    NgxMdModule.forRoot(),
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Configure blog folder
Now lets create a subfolder 'blog' where we store all our blogs in markdown format:
```
mkdir src/blog
```

This folder needs to be configured in the angular.json file. Add blog folder in angular.json at two places in build>options>assets and test>options>assets:
```
{
  "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
  "version": 1,
  "newProjectRoot": "projects",
  "projects": {
    "ng-blog": {
      "root": "",
      "sourceRoot": "src",
      "projectType": "application",
      "prefix": "app",
      "schematics": {},
      "architect": {
        "build": {
          "builder": "@angular-devkit/build-angular:browser",
          "options": {
            "outputPath": "dist/ng-blog",
            "index": "src/index.html",
            "main": "src/main.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "src/tsconfig.app.json",
            "assets": [
              "src/favicon.ico",
              "src/assets",
              "src/blog"
            ],
...
        "test": {
          "builder": "@angular-devkit/build-angular:karma",
          "options": {
            "main": "src/test.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "src/tsconfig.spec.json",
            "karmaConfig": "src/karma.conf.js",
            "styles": [
              "src/styles.css"
            ],
            "scripts": [],
            "assets": [
              "src/favicon.ico",
              "src/assets",
              "src/blog"
            ]
          }
        },
...
```

## Create and display a markdown file

Create a src/blog/hello-world.md:

```
# Hello

... world!
```

Now edit src/app/app.component.html, remove all lines and add this oneliner:
```
<ngx-md [path]="'blog/hello-world.md'"></ngx-md>
```
## Build and publish your changes

Finally build and publish:
```
ng build --prod --base-href "https://taitruong.github.io/angular-markdown/"
npx ngh --dir=dist/angular-markdown
```

Now going back to [taitruong.github.io/angular-markdown/](https://taitruong.github.io/angular-markdown/), you'll see a nice static page using Angular 6 and Markdown.

Last but not least, don't forget committing your changes:

```
git add --all
git commit -m 'install ngx-md module for markdown pages and create first hello-world.md'
```

# Prospects

I googled around and haven't found a solution of combining Jekyll and Angular. Maybe someday I'll figure out myself and let you know. All I found so far is how to use [Jekyll and AngularJS together](http://www.noxidsoft.com/development/using-jekyll-and-angularjs-together/). I'll keep you posted!

Enjoy, Mr. Tdocumentation
