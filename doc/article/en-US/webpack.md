---
name: Webpack
description: Webpack and the Aurelia CLI.
author: Jeroen Vinke (https://jeroenvinke.nl)
---

## Introduction
The Aurelia CLI fully supports Webpack. When you create a new project by running `au new` you can either choose for the Aurelia CLI bundler or you can choose for Webpack. The information in this document is specifically for people using the Webpack bundler.

The Aurelia CLI will generate a `webpack.config.js` based on the technology that you select during `au new`. Like SASS more than LESS? Jest more than Karma? The Aurelia CLI can configure `webpack.config.js` just how you like it.

The generated project makes use of the aurelia-webpack-plugin. As opposed to the Aurelia CLI Bundler, the bundle configuration is in the `webpack.config.js`, and not in `aurelia.json`. 

If you would like to learn more about Webpack, take a look at the following resources:
- https://www.pluralsight.com/courses/webpack-fundamentals
- https://blog.madewithenvy.com/getting-started-with-webpack-2-ed2b86c68783
- https://webpack.js.org/configuration/

## Deploying Your App
When your application is done, the next step is to deploy it. First you will want to create a production build with `au build --env prod`. Depending on whether you use ASP.NET Core or not, the build output will end up in the `dist` folder or the `wwwroot/dist` folder.

If you're not using ASP.NET core then everything you need to deploy can be found in the dist folder. Just copy this to the webserver and you'll be good to go. For ASP.NET Core however you'll want to do a production build (`au build --env prod`) and then deploy the ASP.NET Core application. Details can be found in the [Microsoft Docs](https://docs.microsoft.com/en-us/aspnet/core/publishing/?tabs=aspnetcore2x).

## Build options
When you look in the `webpack.config.js` you'll see that it exports a function:

```
module.exports = ({production, server, extractCss, coverage} = {}) => ({
```

The configuration that's returned from the function is based on the parameters that get passed in. That means that you can have a different Webpack config for production and development, but you can also configure whether to extract any CSS.

The build and run tasks passes in these parameters. Most of these settings are defined in the `build.options` object in `aurelia.json`:

<code-listing heading="Build options">
  <source-code lang="JavaScript">
	"build": {
		"options": {
		  "server": "dev",
		  "extractCss": "prod",
		  "coverage": false
		}
	},
  </source-code>
</code-listing>

One is missing though: `production`. The environment (whether it's a production build or not) is determined from the `--env` flag. `au build` creates a development build, while `au build --env prod` creates production build.

## Installing 3rd party dependencies
Webpack is a powerful and smart bundler, which means that you often don't need to configure a 3rd party dependency.  And if you do then there's a good chance that someone has done it before, so Google is your friend here. 

Check out the [Webpack documentation](https://webpack.js.org/concepts/) for more info on the `webpack.config.js`.

## Unit Testing
Depending on what you've selected during `au new` you will have the possibility to run one of the following test runners:

- Jest
- Karma
- Protractor

Jest can be launched using `au jest` or `au jest --watch` if you would like to do continuously run tests.

Similarly, Karma can be started using `au karma` or `au karma --watch`. 

However, Protractor is a bit different. It has to be started through `nps`, so make sure that you've installed that globally (`npm install nps -g`). Afterwards, run `nps e2e` to start Protractor.

## ASP.NET Core
When developing an ASP.NET Core application you will want to set the `ASPNETCORE_ENVIRONMENT` environment variable. Detailed instructions can be found on the [Microsoft Docs](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments#setting-the-environment).