MyMusicBook

---------------------------------------------------------------------------------------------------------------------------------------------
####electron-boilerplate:

The application consists of two main folders...

src - files within this folder get transpiled or compiled (because Electron can't use them directly).

app - contains all static assets which don't need any pre-processing. Put here images, CSSes, HTMLs, etc.

The build process compiles the content of the src folder and puts it into the app folder, so after the build has finished, your app folder contains the full, runnable application.

Treat src and app folders like two halves of one bigger thing.

The drawback of this design is that app folder contains some files which should be git-ignored and some which shouldn't (see .gitignore file). But this two-folders split makes development builds much, much faster.

###Development:

Build process uses Webpack. The entry-points are src/background.js and src/app.js. Webpack will follow all import statements starting from those files and compile code of the whole dependency tree into one .js file for each entry point.

Babel is also utilised, but mainly for its great error messages. Electron under the hood runs latest Chromium, hence most of the new JavaScript features are already natively supported.

###Environments
Environmental variables are done in a bit different way (not via process.env). Env files are plain JSONs in config directory, and build process dynamically links one of them as an env module. You can import it wherever in code you need access to the environment.

import env from "env";
console.log(env.name);

###Upgrading Electron version
To do so edit package.json:

"devDependencies": {
  "electron": "1.7.9"
}

###Adding npm modules
Remember to respect the split between dependencies and devDependencies in package.json file. Your distributable app will contain modules listed in dependencies after running the release script.

Side note: If the module you want to use in your app is a native one (not pure JavaScript but compiled binary) you should first run npm install name_of_npm_module and then npm run postinstall to rebuild the module for Electron. You need to do this once after you're first time installing the module. Later on, the postinstall script will fire automatically with every npm install.

###Testing
Run all tests:

npm test
Unit
npm run unit
Using electron-mocha test runner with the Chai assertion library. You can put your spec files wherever you want within the src directory, just name them with the .spec.js extension.

###End to end
npm run e2e
Using Mocha and Spectron. This task will run all files in e2e directory with .e2e.js extension.

###Making a release
To package your app into an installer use command:

npm run release
Once the packaging process finished, the dist directory will contain your distributable file.

We use electron-builder to handle the packaging process. It has a lot of customization options, which you can declare under "build" key in package.json.

You can package your app cross-platform from a single operating system, electron-builder kind of supports this, but there are limitations and asterisks. That's why this boilerplate doesn't do that by default.