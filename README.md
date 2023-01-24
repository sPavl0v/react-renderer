# react-spa-prerender

The easiest way to prerender static pages, optimize SEO and build high performance for your React SPA.
Build production-ready code just by adding few lines of code.

- [Example of usage with create-react-app](https://github.com/sPavl0v/react-spa-prenderer/tree/master/examples/cra)
- [create-react-app + lazy loading](https://github.com/sPavl0v/react-spa-prenderer/tree/master/examples/cra-lazy)

### Upcoming features

- Auto sitemap generation
- Prebuilding pages with dynamic routes

Follow the steps below:

## Install

With npm

```
npm install react-spa-prerender --save-dev
```

With yarn

```
yarn add react-spa-prerender --dev
```

## Add as postbuild script

In your package.json add the following in the scripts section:

```
"scripts": {
    "postbuild": "react-spa-prerender",
}
```

## Add .rsp.json file

**.rsp.json** is the configuration file for `react-spa-prerender`. Create this file in your **application root folder**.
The minimum configuration requires the **routes** you want to be parsed.
Example:

```
{
    routes: [
        "/",
        "/about",
        "/services"
        "/blog/article1"
        ...
    ]
}
```

From example above:
Your "/" route will transform into "index.html" page.
"/about" -> "about.html"
"/services" -> "services.html"
"/blog/article1" -> create blog directory with file "article1.html" ("/blog/article1.html")
and so on...

**The rest of the .rsp.json options described below**

## Add ReactDOM.hydrate in your index.js

In your index.js(main app file) change the ReactDOM.render logic:

```
import ReactDOM from 'react-dom';

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

Into following:

```
import ReactDOM from 'react-dom';

const rootElement = document.getElementById("root");

if (rootElement.hasChildNodes()) {
  ReactDOM.hydrate(<App />, rootElement);
} else {
  ReactDOM.render(<App />, rootElement);
}
```

## Voila!!!

That's it. After accomplishing all the steps above, run you build command and your prerendered files will be in your build directory.

## .rsp.json Options

| option                  | type   | default   | description                                                           |
| ----------------------- | ------ | --------- | --------------------------------------------------------------------- |
| routes(Required)        | Array  | -         | An array of routes you want to parse and prerender into static html   |
| urlParameters(Optional) | Array  | -         | An array of URL Parameter you want to pass along with Route           |
| port                    | Number | 3000      | port where prerendering server will be starting                       |
| buildDirectory          | String | './build' | a relative path to your build folder                                  |
| engine                  | Object | {}        | params for Puppeteer engine, list of available params described below |

### Engine options:

- launchOptions - object, containing properties to control **puppeteer.launch()** command. The whole list of available properties available here: [https://pptr.dev/#?product=Puppeteer&version=v10.0.0&show=api-puppeteerlaunchoptions](https://pptr.dev/#?product=Puppeteer&version=v10.0.0&show=api-puppeteerlaunchoptions)
- gotoOptions - object, navigation parameters. The whole list of available properties available here: [https://pptr.dev/#?product=Puppeteer&version=v10.0.0&show=api-pagegotourl-options](https://pptr.dev/#?product=Puppeteer&version=v10.0.0&show=api-pagegotourl-options)

#### Example of .rsp.json with engine options:

```
{
  "port": 3000,
  "buildDirectory": "./build",
  "engine": {
    "launchOptions": {
      "args": ["--no-sandbox", "--disable-setuid-sandbox"],
      "product": "chrome",
      "headless": true,
      "ignoreHTTPSErrors": true
    },
    "gotoOptions": {
      "timeout": 0
    }
  },
  "routes": [
    "/",
    "/about",
    "/services",
    "/blog/article1",
    "/blog/article2"
  ],
  "urlParameters": [
    "?type=a",
    "?type=b",
    "",
    "?demo=yes&tester=yes"
  ]
}
```
