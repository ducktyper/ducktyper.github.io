---
layout: post
title:  "Angular2 from jQuery"
categories: notes
---

# Learning Angular2 as a jQuery developer

Recently I started replacing a single page portion of our website written in jQuery
with Angular2 with typescript. Here is the list of things I struggled with
and the way to solve.

## Setup development environment

It took 3 days to just setup the development environment. At first, I was looking for
an easy way to integrate to Rails. However, there was no angular2 gem and I failed
to make typescript modules working in Rails assets pipeline.

I gave up on that solution and decided to use what Angular2 project recommands.
Angular2 website has a tutorial `5 Min Quick Start`. This project uses `lite-server`
to serve assets and `systemjs` to load modules. It seemed like it uses too many
javascript libriaries and npm packages to start with. However the result was really cool.
lite-server provides assets and systemjs dynamically loads typescript files
from the server as needed and run them after transpiling in the browser.

Unfortunately, this was not the end of the story. `5 Min Quick Start` is perfect
for a quick development setup but not for the production. I didn't want to transpile
typescript files in the browser and didn't want the browser requesting each single
typescript files.

will continue...
