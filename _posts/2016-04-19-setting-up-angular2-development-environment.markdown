---
layout: post
title:  "Setting up Angular2 development environment"
categories: angular
---

Recently we decided to use Angular2 with Typescript for our new project. My position
was to learn Angular2 and guide University interns to work on the project. The
first thing I did was setting up the development environment for interns so
we all develop in the same envioronment.

## Setup development environment

It took 3 nights to just setup the development environment. At first, I was looking for
an easy way to integrate into Rails. However, there was no angular2 gem and I failed
to make typescript modules working with Rails assets pipeline.

I was looking at Angular2 website and found a tutorial `5 Min Quick Start`.
The tutorial shows how to setup the first project from nothing. I tried it
to see if we can use the same setup. The tutorial uses `lite-server` to serve assets
to the browser and `systemjs` to transplie Typescript files in the browser.
This setup works well in development environment.
However, we didn't want to traspile Typescript in the browser in production and
the tutorial did not mention how to build production code in this setup.

I wanted a simple way to build production code without using JSPM, Webpack and so on
so I looked at the way using Typescript. Typescript provides a compiler option `--outFile`
to concatenate the output. This feature was working but the output file didn't do
anything in the browser becuase Typescript does not know what is the main module
to execute. After playing with the transpiled code for a while, I found that
if the first module in the file has no module name, it will be excuted after load.
However, this seems like not the proper solution to use.

After some more research, I found a project [angular2-seed](https://github.com/angular/angular2-seed.git)
which uses Webpack to build production code. Webpack provides build for multiple apps
from one project and also allow to embed html and css files into Javascript using loaders.
However, I found that a simple app's production build went over 4MB which was quite large.

I played with Angular2 using Webpack for few weeks and everything was fine until
one day I heard about Angular-CLI. Angular-CLI was in beta 1 but it seemed like
the standard way to build Angular2 apps. So I switched using Angular-CLI from Webpack.
I did not find a way to embed css and html files into Javascript but the size of
production code were less then 1MB so I was happy with the results.

## Things I have learn

Setting up Angular2 project was a painful process with too many choices and
too much things to know. I think this is the same to many modern front-end
development environment. I was glad to see Angular-CLI is trying to solve this
problem. Now I can't wait to play with Angular2.
