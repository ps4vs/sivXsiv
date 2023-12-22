---
title: anySTOCKnow
date: "{{date}}"
tags:
  - seed
enableToc: false
---
### Introduction

Yesterday, I was going through Microsoft's [{guidance}](https://github.com/guidance-ai/guidance?tab=readme-ov-file) repo, and some relevant blogs posts. Surprisingly I came across name '**Handlebars-inspired Syntax**' though it is for v1 version. 

I want to use words like 'WTF is Handlebars-inspired Syntax' etc, but let's those are not natural for me, so we'll skip them for now.

### Project Overview

I found my calling to learn some basics of node, express, dotenv, and express-handlebars after smashing poor GPT-4 with stupid questions.

- It involves 20 lines backend code using express. Mostly wrapper around third-party API using axios.
- Two dynamical views rendered by server via templating engine. Around 10 lines of HTML, and CSS code.

![[anySTOCKnow-stockPrice.png]]![[anySTOCKnow-home.png]]
### Background

**Handlebars** is one of the most used **templating engines** used on server side along with express framework, others are EJS etc. It basically takes **template files (static HTML file + placeholder)**, and **fills them with necessary data on server-side then sends this to client-side.**

**Axios is simple promise based HTTP client for browser and node.js**. It is often on **browser to make HTTP requests**, and on Node.js is **used to make requests to external APIs** or other web services. 

#### Form in HTML

The `name=symbol` in the below html form is sent along with the get request made to `/search` endpoint.
```html
  <form action="/search" method="get">
    <input type="text" name="symbol" placeholder="Enter Stock Symbol" required>
    <button type="submit">Search</button>
  </form>
```
#### Views in Express.js with Handlebars

1. **Overview**:
   - In Express.js applications using Handlebars, the `views` directory typically contains two main types of files: layouts and templates.

2. **Layouts**:
   - **Purpose**: Layouts provide a consistent structure for web pages. They define the common HTML elements that are shared across different pages, such as headers, footers, and navigation bars.
   - **Default Layout**: The `main.handlebars` file is often used as the default layout. It includes a placeholder (usually `{{{body}}}`) where the content of individual templates will be inserted.
   - **Customization**: While `main.handlebars` is the default, you can create and use multiple layout files for different sections of your application.

3. **Templates**:
   - **Purpose**: Templates are individual view files that contain the specific content for each page. They are inserted into the layout's `{{{body}}}` placeholder when rendered.
   - **Flexibility**: Templates allow you to define the unique content of each page while maintaining a consistent overall layout.

4. **Rendering Views**:
   - **Default Behavior**: When a view is rendered using `res.render('viewName')`, Express.js uses Handlebars to insert the content of `viewName.handlebars` into the default layout (`main.handlebars`).
   - **Specifying a Layout**: You can specify a different layout for a particular view by passing a `layout` option in the render function: `res.render('viewName', { layout: 'otherLayout' })`.
   - **No Layout**: If you want to render a template without any layout, you can set the `layout` option to `false`.
### Setting up
#### node.js project setup

Open terminal and execute code

```
mkdir anySTOCKnow // creating directory
cd anySTOCKnow    // moving to directory
npm init -y       // initializing node project
npm install express express-handlebars axios --save
```
#### .git  and .env setup

Here I will explain creating a local repository and remote repository, setting up remote repository as upstream for local repository, and pushing the local repository to remote repository. 

1. Create a repository on GitHub using UI `+` procedure.
2. Open terminal and execute code
3. Generate your api key from [Financial Modeling Prep website](https://financialmodelingprep.com/developer/docs/).

```
git init
echo node_modules >> .gitignore
git add .
git commit -m 'Initial commit'
git remote add origin <REMOTE_REPOSITORY_URL>
git push -u origin [main or master]

echo API_KEY=your_api_key >> .env
```

### Creating views [layouts and templates]
```
mkdir views
cd views
touch home.handlebars
touch stockPrice.handlebars
mkdir layouts
cd layouts
touch main.handlebars
```
![[views-anySTOCKnow.png]]




### Code

`./index.js`
```javascript
// index.js file

// Importing necessary modules
const express = require('express'); // Express.js framework
require('dotenv').config(); // dotenv for loading environment variables
const { engine } = require('express-handlebars'); // Handlebars templating engine
const axios = require('axios'); // Axios for making HTTP requests

// Setting up Handlebars as the view engine for Express
app.engine('handlebars', engine()); 
app.set('view engine', 'handlebars');

// title is present in head which contains meta-data, which is not visible on the html page.
// Route handler for home page
app.get('/', async (req, res) => {
	// Rendering the 'home' view with a title
	res.render('home', {title: 'home antar ra babu'});
});


// Route handler for the '/search' endpoint
app.get('/search', async (req, res) => {
	// Extracting the 'symbol' query parameter and converting it to uppercase
	const symbol = req.query.symbol.toUpperCase();
	// Retrieving the API key from environment variables
	const apiKey = process.env.API_KEY;
	// Constructing the URL for the API request
	const url = `https://financialmodelingprep.com/api/v3/quote/${symbol}?apikey=${apiKey}`; // template strings

	try {
		// Making an HTTP GET request using Axios
		const response = await axios.get(url);
		const stockData = response.data[0];
		const company = stockData.name;
		const stockPrice = stockData.price;
		// Logging the symbol and price to the console
		console.log(symbol, stockPrice);

		// Rendering the 'stockPrice' view with retrieved data
		res.render('stockPrice', {title: "stock stalk", company: company, stockPrice: stockPrice});
	} catch (error) {
		// Logging any errors to the console
		console.error('Error fetching stock data:', error);
		// Sending a 500 Internal Server Error response
		res.status(500).send('Error fetching stock data');
	}
});
```

`./views/layouts/main.handlebars`
```html
<!DOCTYPE html>
<html>
<head>
    <title>Basics</title>
</head>
<body>
    <div class="container">
        <header>
            <h5>made with node, express, and handlebars</h5>
        </header>
        {{{body}}} // this is where template is inserted
        <footer>
            <h5>© {{year}} Subhash's Exploration. All rights reserved.</h5>
        </footer>
    </div>
</body>
</html>
```

`./views/home.handlebars`
```html
<!DOCTYPE html>
<html>
<head>
  <title>{{title}}</title>
  <style>
    body {
      color:white;
      text-align: center;
      background-color: black
    }
    h1 {
      color:grey;
    }
    h2 {
      color:grey;
    }
  </style>
</head>
<body>
  <h1>Welcome to the Stock Tracker</h1>
  <form action="/search" method="get">
    <input type="text" name="symbol" placeholder="Enter Stock Symbol" required>
    <button type="submit">Search</button>
  </form>
  <h2>swamiyeee saranamyyapa</h2>
</body>
</html>
```

`./views/stockPrice.handlebars`
```html
<!DOCTYPE html>
<html>
<head>
  <title>anySTOCKnow</title>
  <style>
    body {
      background-color: #0c0b0b; 
      text-align: center;
      color: #e2ece1; 
    }
    h1 {
      color:grey;
    }
    h2 {
      color:grey; /* Green color for the stock price */
      margin-top: 20px; /* Space above the stock price */
    }
    h3 {
      color:grey; /* Green color for the stock price */
      margin-top: 20px; /* Space above the stock price */      
    }
  </style>
</head>
<body>
  <h1>{{title}}</h1>
  <h2>{{company}} </h2>
  <h3><span>{{stockPrice}}</span></h3>
</body>
</html>
```