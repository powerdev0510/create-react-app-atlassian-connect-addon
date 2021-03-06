# create-react-app-atlassian-connect-addon

# Quick Start

Create Atlassian Connect addon just in a few minutes.

## Install

```shell
git clone https://github.com/powerdev0510/create-react-app-atlassian-connect-addon.git <YOUR-ADDON-NAME>
cd <YOUR-ADDON-NAME>
rm -rf .git
npm i
```

**You’ll need to have Node >= 6 on your machine.**

## Configure

### package.json

Specify the following properties:

+ `atlassian-connect-dev-instance-host` - name of the host of your Atlassian **development** instance (e.g. `<YOUR-DEV-INSTANCE-NAME>.atlassian.net`)
+ `atlassian-connect-prod-instance-host` - name of the host of your atlassian **production** instance (e.g. `<YOUR-PROD-INSTANCE-NAME>.atlassian.net`)
+ `atlassian-connect-addon-home` - URL to your addon on production (e.g. `<YOUR-ADDON-NAME>.firebaseapp.com`)

### atlassian-connect.json

`atlassian-addon.json` is an Atlassian Addon Descriptor - visit [official Atlassian documentation](https://developer.atlassian.com/static/connect/docs/latest/modules/) for more info.

### Start Development

```shell
npm start
```

### Upload Addon

+ Go to `https://<YOUR-DEV-INSTANCE-NAME>.atlassian.net/plugins/servlet/upm`
+ Click "Upload add-on"
+ Paste the URL that was automatically added to your clipboard after running `npm start` (path to the `atlassian-connect.json` generated by [ngrok](https://ngrok.com/))

Now you are all set! Use all the benefits of the `react-create-app` tooling developing an awesome Atlassian add-on.

# Boilerplate Structure

The addon's initial structure will look like the following:

```
<YOUR-ADDON-NAME>
  README.md
  node_modules/
  package.json
  .gitignore
  .editorconfig
  public/
    atlassian-connect.json
    favicon.ico
    index.html
    manifest.json
  scripts/
    pre-start.js           
    pre-build.js
    util.js
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
    registerServiceWorker.js
```

It contains all the files from the create-react-app initial structure plus some Atlassian Addon specific ones.

## New Files

### atlassian-addon.json 

`atlassian-addon.json` is an Atlassian Addon Descriptor - visit [official Atlassian documentation] for more info.

### scripts/pre-start.js

+ Being executed before `react-scripts start` (see `package.json`)
+ Adds `REACT_APP_HOST` environment variable with your Atlassian development instance host URL specified in the `"atlassian-connect-dev-instance-host"` in your `package.json` file
+ Initializes [ngrok](https://ngrok.com/) to tunnel the `https://localhost:3000`
+ Updates `public/atlassian-connect.json`'s `"baseUrl"` with the by ngrok generated URL
+ Updates `public/atlassian-connect.json`'s `"links/config"` with the full public path to the `atlassian-connect.json` file (e.g. `https:/4c413a45.ngrok.io/atlassian-connect.json`)
+ Adds atlassian-connect public URL (generated by `ngrok`) to the clipboard so you don't need to open the file to copy it yourself before installing in your Atlassian instance

### scripts/pre-build.js

+ Being executed before `react-scripts start` (see `package.json`)
+ Adds `REACT_APP_HOST` environment variable with your Atlassian production instance host URL specified in the `atlassian-connect-prod-instance-host` in your `package.json` file
+ Updates `public/atlassian-connect.json`'s `"baseUrl"` with one specified in `package.json`'s `"atlassian-connect-addon-home"` property
+ Updates `public/atlassian-connect.json`'s `"links/config"` with the full public path to the production `atlassian-connect.json` file (e.g. `https://myaddon.firebaseapp.com/atlassian-connect.json`)
+ Adds atlassian-connect **production** public URL to the clipboard so you don't need to open the file to copy it yourself before installing in your Atlassian instance

### util.js

Contains helper functions to be used in `pre-start.js` and `pre-build.js` scripts.

## Edited Files

### package.json

#### Atlassian specific properties, modules and scripts

+ `atlassian-connect-dev-instance-host` - host URL of your Atlassian development instance
+ `atlassian-connect-prod-instance-host` - host URL of your Atlassian production instance
+ `atlassian-connect-addon-home` - where your addon will be hosted on production
+ `"copy-paste"` - used to add the development `atlassian-connect.json` URL to the clipboard
+ `"ngrok"` - used to create a tunnel to the localhost for the development
+ `"start": "node scripts/pre-start.js & react-scripts start"` - runs `pre-start.js` in addition to `react-scripts start`
+ `"build": "node scripts/pre-build.js & react-scripts build"` - runs `pre-build.js` in addition to `react-scripts build`

```json
{
  "name": "create-react-app-atlassian-connect-addon",
  "version": "0.1.0",
  "private": true,
  "atlassian-connect-dev-instance-host": "dev-example.atlassian.net",
  "atlassian-connect-prod-instance-host": "example.atlassian.net",
  "atlassian-connect-addon-home": "https://example.com/",
  "dependencies": {
    "react": "^15.6.1",
    "react-dom": "^15.6.1"
  },
  "devDependencies": {
    "copy-paste": "^1.3.0",
    "ngrok": "^2.2.10",
    "react-scripts": "1.0.7"
  },
  "scripts": {
    "start": "node scripts/pre-start.js & react-scripts start",
    "build": "node scripts/pre-build.js & react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}

```

### public/index.html

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="theme-color" content="#000000">
    <!--
      manifest.json provides metadata used when your web app is added to the
      homescreen on Android. See https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json">
    <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
    <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.

      Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    <title>React Atlassian Addon</title>
  </head>
  <body>
    <noscript>
      You need to enable JavaScript to run this app.
    </noscript>
    <div id="root"></div>
    <!--
    See https://developer.atlassian.com/static/connect/docs/latest/concepts/javascript-api.html#js-client-lib for more details on what this script does and why it is required.
    -->
    <script data-options="sizeToParent:true;" src="%REACT_APP_HOST%/atlassian-connect/all.js"></script>

    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.

      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.

      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
  </body>
</html>
```
# create-react-app-atlassian-connect-addon
