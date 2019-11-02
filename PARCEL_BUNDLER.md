# PARCEL BUNDLER



##### INSTALLATION & SET UP

Npm:

```bash
npm install -g parcel-bundler
```



Create a package.json file:

>  (-y: accept all the default values for package.json)

```bash
npm init -y
```



Install parcel-bundler package:

```bash
npm install parcel-bundler
```



FILE STRUCTURE (SAMPLE ASSETS):

```
src
└──── scripts
			└──── index.js
index.html
package.json
```



<hr>

##### PARCEL DEV-SERVER

Run parcel dev-server using npm script:

- Add ```"dev": "parcel src/index.html"```

```json
{
  "name": "bundler",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "parcel src/index.html"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "parcel-bundler": "^1.12.4"
  }
}
```

* Run parcel dev-server via npm:

```bash
npm run dev

> bundler@1.0.0 dev /path/lib/static/vendors/bundler
> parcel src/index.html

Server running at http://localhost:1234 
✨  Built in 501ms.
```



<hr>

##### BABEL



* Install plugin support for class-properties:

> Note: we do this to be able to write ES6 class, the plugin allows us to transform class to native javascript.

```bash
npm install --save-dev @babel/plugin-proposal-class-properties
```



* Add ```.babelrc``` in the project directory:

```json
{
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```



<hr>

##### USE SASS 



* Install sass package

```bash
npm install sass
```



* Create index.scss:

```scss
$primary-color: red;

h1 {
  color: $primary-color;
}
```

* Inside index.js:

```
import '../styles/index.scss'
```



<hr>

##### BUILD PRODUCTION

* Add script for npm run:

```json
"scripts": {
    "dev": "parcel src/index.html",
    "build": "parcel build src/index.html --out-dir prod"
 }
```

> Note: It will create a new folder "prod" which contains all the production files



* Then, package.json looks like below:

```json
{
  "name": "bundler",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "parcel src/index.html",
    "build": "parcel build src/index.html --out-dir prod"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "bootstrap": "^4.3.1",
    "jquery": "^3.4.1",
    "popper.js": "^1.16.0",
    "sass": "^1.23.3"
  },
  "devDependencies": {
    "@babel/core": "^7.6.4",
    "@babel/plugin-proposal-class-properties": "^7.5.5",
    "@fortawesome/fontawesome-free": "^5.11.2",
    "parcel-bundler": "^1.12.4"
  }
}

```



* Import all in one file "index.js":

```js
import 'bootstrap'
import 'bootstrap/dist/css/bootstrap.css' // Import precompiled Bootstrap css
import '@fortawesome/fontawesome-free/css/all.css'
import '../styles/index.scss' 
```



* Run build

```bash
npm run build
```



