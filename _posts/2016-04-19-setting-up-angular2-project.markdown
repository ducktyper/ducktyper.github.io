---
layout: post
title:  "Setting up Angular2 project"
categories: notes
---

# Learning Angular2 as a jQuery developer

Recently we decided to use Angular2 with Typescript for our new project. My position
was to learn Angular2 and guide University interns to work on the project. I learnt
basic syntax of Angular2 from online resources but there were not many practical
ones yet. I felt the need for practical experience with Angular2 so I decided to
convert some of our web pages using Angular2.

## Setup development environment

It took 3 nights to just setup the development environment. At first, I was looking for
an easy way to integrate into Rails. However, there was no angular2 gem and I failed
to make typescript modules working with Rails assets pipeline.

I tried to use the same structure used in a tutorial `5 Min Quick Start` from Angular2 website.
This tutorial uses `lite-server` to serve assets to the browser and `systemjs`
tanspiles Typescript files to ES5 javascipt files in the browser. The tutorial
works well in development environment, but didn't provide a way to deploy.

I wanted a simple way to build production files without using JSPM, Webpac and so on
so I looked at the way using Typescript. Typescript provides a compiler option `--outFile`
to concatenate the output. This feature was working but the output file didn't do
anything in the browser becuase there was no main module to execute.
After playing with the transpiled code for a while, I found that if the first module
in the file has no module name, it will be excuted after load. I didn't want to
make that change for every build so I gave up on that solution.

After some more research, I found a project (angular2-seed)[https://github.com/angular/angular2-seed.git]
which uses Webpack to provide production build. Using Webpack I could easily
build multiple apps from one project and also embed html and css files into Javascript
on build using loaders. However, I found that production Javascript files go over 4MB
which I didn't like.

Webpack was working well and used it for few weeks until one day I heard about
Angular-CLI. Angular-CLI seemed like the standard way to build Angular2. So
I converted using Angular-CLI from Webpack. I did not find a way to embed css
and html files into Javascript and Angular-CLI generates each of my modules in
a different file but the size in total was less them 1MB so I was happy with the results.

## Things I have learn

Setting up Angular2 project was a painful process with too many choices and too much things to know.
I think this is the same to any mordern front-end development environment.
I think Angular-CLI is trying to solve one of the biggest issue to beginners by providing the default
development environment.
