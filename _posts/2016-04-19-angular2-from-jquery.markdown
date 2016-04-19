---
layout: post
title:  "Angular2 from jQuery"
categories: notes
---

# Learning Angular2 as a jQuery developer

Recently we decided to use Angular2 with Typescript for our new project. My position
was to learn Angular2 and guide University interns to work on the project. I learnt
basic syntax of Angular2 from online resources but there were not many practical
ones yet. I felt the need for practical experience with Angular2 so I decided to
convert some of our web pages using Angular2.

This blog is about me converting few complex web pages from jQuery to Angular2. I have
used jQuery over 4 years in my current company and built some complieted pages
including a single page application. I was wondering how these pages can be converted
using Angular2.

## Setup development environment

It took 3 nights to just setup the development environment. At first, I was looking for
an easy way to integrate into Rails. However, there was no angular2 gem and I failed
to make typescript modules working with Rails assets pipeline.

I gave up on that solution and decided to use what Angular2 project recommands.
Angular2 website has a tutorial `5 Min Quick Start`. This project uses `lite-server`
to serve assets and `systemjs` to load modules. It seemed like it uses too many
javascript libriaries and npm packages to start with but the result was impressive.
lite-server provides assets and systemjs dynamically loads typescript files
from the server as needed and run them after transpiling in the browser.

Unfortunately, this was not the end of the story. `5 Min Quick Start` is perfect
for a quick development setup but not for the production. I didn't want to transpile
typescript files in the browser and didn't want the browser requesting each single
typescript file I wrote.

will continue...
