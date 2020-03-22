## Create a new project connected to Github

Create GitHub repository
```
https://github.com
```

Pull on local
```
mkdir example-emptyproject
cd example-emptyproject
git init
git remote add origin https://github.com/jordisabadell/example-emptyproject
git pull origin master
```

Open VSCode IDE
```
Add folder to workpace...
Menu > Terminal > New terminal
```

Initialize project
```
npm init
npm i --save-dev webpack webpack-cli
```

Create src folder and index.js file
```
src/index.js
```

Add dummy log
```
console.log("Done!")
```

Test
```
webpack --mode development
```

Edit package.json
```
"scripts": {
	"build": "webpack --mode production",
	"builddev": "webpack --mode development"
	},
```
 
Create webpack configuration file
```
webpack.config.js
```
	
Add webpack configuration lines
```
const path = require('path');

module.exports = {
	output: {
	path: path.resolve(__dirname, 'dist/'),
	filename: 'main.js'
	},
	entry: {
	main: './src/index.js'
	}
}
```

Create Sass file
```
src/style.scss
```
	
Add dummy style
```
body {
	color: red;
}
```
	
Install plugin and loaders
```
npm install -D html-webpack-plugin
npm install -D mini-css-extract-plugin
npm install -D css-loader
npm install -D node-sass sass-loader```
```
Add webpack configuration lines at webpack.config.js
```
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCSSExtractPlugin = require('mini-css-extract-plugin');

...

module.exports = {
	output: {
		...
	},
	entry: {
		...
	},
	plugins: [
		new HtmlWebpackPlugin({  
			filename: 'index.html',
			template: 'src/index.html',
			hash: true
		}),
		new MiniCSSExtractPlugin({
			filename: "style.css",
		})
	],
	module: {
		rules: [
		{ 
			test: /\.scss$/, 
			loader: [
				MiniCSSExtractPlugin.loader,
				"css-loader",
				'sass-loader'
			]
		}
		]
	}
}
```

Add *import style* file at src/index.js
```
import './style.scss';
```
	
Create HTML file
```
src/index.html
```
	
Add HTML content
```
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Example</title>
	
</head>
<body>
	<h1>Done!</h1>
</body>
</html>
```
	
Install server 
```
npm install -D webpack-dev-server
```
	
Add webpack server configuration at package.json
```
"scripts": {
	...
	"start": "webpack-dev-server --mode development --open"
},
```

## Deploy to Firebase hosting

Install firebase
```
npm install -g firebase-tools
```

Login and init
```
firebase login
firebase init ##if an error occurs, create the project through the web (https://firebase.google.com)
```

Deploy
```
firebase deploy
```

## Connect Github to TravisCI for CI/CD

Create .travis.yml file
```
#.travis.yml
language: node_js
node_js:
  - "12.16.1"
branches:
only:
  - master
before_script:
  - npm install -g firebase-tools
script:
  - npm run build 
after_success:
  - firebase deploy --token $FIREBASE_TOKEN
notifications:
  email: jordisabadell@gmail.com
  on_failure: always
  on_success: always
```

**Important:** update nodeJS version on .travis.yml file
```
node --version
```

Get Token Firebase 
```
firebase login:ci
```

Open Travis-CI web, login using Github account and add your project
```
https://travis-ci.com/
```

Set your Token Firbase as Environment Travis-CI variable
```
Travis-CI > My Repositories > {Your repository} > More options > Settings > Environment Variables
```