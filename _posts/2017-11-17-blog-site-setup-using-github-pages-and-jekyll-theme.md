---
layout: post
title: Setting Up a Blog Site using GitHub Pages and a Jekyll Theme
date: 2017-11-17 01:18
excerpt: "A step-by-step guide for installing a blog site using git, GitHub, and Markdown."
tags: [GitHub Pages, Jekyll Themes, git, GitHub, git clone, docs like code, docs as a code, content as code]
comments: true
---
# Motivation

I'm... :ramen: 
:panda_face:
> a developer, foodie :ramen:, and generally passionate human :panda_face: by :heart:.
> intrigued by concept docs-as-code/docs like code, content-as-a-service

It was about time...
> getting ownership and control back from social medias like Facebook and Instagram.
> Now it's my content and ownership and social media is just a platform for sharing mine

Last but not least...
> following my passions, sharing my notes for the ones it may help, and
> finding, getting connected, and share interests and thoughts with you :point_left:!

# Preparations

- Sign up at [GitHub.com](https://github.com/join)
- There are about ~10 GitHub themes. More to find here: http://jekyllthemes.org
- My selected theme is [TaylanTatli's Moon theme](https://taylantatli.github.io/Moon/moon-theme/)

# Fork Theme

As described on [GitHub pages](https://pages.github.com) you can setup an
- user site,
- organization site, or a
- project site.

In my case I go for a project site, namely 'software-developer.org'. Its project repo goes to my GitHub user account. Since I prefer using git instead of clicking on GitHub around. I do this:
- create an empty GitHub repo e.g. 'software-developer.org'
- do this on your shell:

```
# source: https://stackoverflow.com/a/19279822
# clone your selected theme under your project site's name e.g. 'software-developer.org'
git clone https://github.com/TaylanTatli/Moon software-developer.org

# remove origin and point it to your remote project/repo
cd software-developer.org
git remote remove origin
git remote add origin https://github.com/taitruong/software-developer.org.git
git push -u origin master
```

# Install Site
Depending on which theme you have selected, there is a setup guide. My choice is [TaylanTatli's beautiful Moon theme](https://taylantatli.github.io/Moon/moon-theme/)

So I did:
- edit _config.yml
- remove all sample posts from _posts folder
- edit index.md in about folder

Next you can publish your first post:
- write your post e.g. 2017-11-17-my-first-post.md and store it under _posts folder
- don't forget to adjust README.md to your needs

If you are not familiar with Markdown, have a look here:
- [GitHub's introduction](https://guides.github.com/features/mastering-markdown/)
- [GitHub: Basic writing and formatting syntax](https://help.github.com/articles/basic-writing-and-formatting-syntax/)
- [Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet/)

# Enable your GitHub pages
All you need is to enable it:
<figure class="half">
	<img src="/_posts/2017-11-17-github-settings-enable-pages.png" alt="GitHub Repo Settings">
	<figcaption>GitHub Repo Settings</figcaption>
</figure>

- Go to your Git Hub repo settings, scroll down to GitHub pages
- select master branch
- save

That's it!

Enjoy, Tai
