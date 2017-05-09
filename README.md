# pixiedust_node

PixieDust extension that enable a Jupyter Notebook user to invoke Node.js commands.

## Prerequisites

To use `pixiedust_node` you need to be running a Jupyter notebooks with the [Pixedust](https://github.com/ibm-cds-labs/pixiedust) extension installed. Notebooks can be run in the cloud using the [Data Science Experience](http://datascience.ibm.com/) or locally by [installing Pixiedust and its prerequisites](https://ibm-cds-labs.github.io/pixiedust/install.html).

You also need Node.js/npm installed. See the [Node.js downloads](https://nodejs.org/en/download/) page to find an installer for your platform.

## Installation

Inside your Jupyter notebook, install *pixiedust_node*  with

```python
!pip install -e /path/to/pixiedust_node
```

## Running

Once installed, a notebook can start up *pixiedust_node* with:

```python
import pixiedust_node
```

## Using %%node

Use the `%%node` prefix in a notebook cell to indicate that the content that follows is JavaScript.

```js
%%node
print(new Date());
```

## Installing npm modules

You can install any [npm](https://www.npmjs.com/) module to use in your Node.js code from your notebook. To install npm modules, in a Python cell:

```python
npm.install('silverlining')
```

or install multiple libraries in one go:

```python
npm.install( ('request', 'request-promise') )
```

and then "require" the modules in your Node.js code.

```js
%%node
var silverlining = require('silverlining');
var request = require('request-promise');
```

## Display/print/store

There are three functions that can be used in JavaScript to interact with the Notebook

- print - print out a variable
- display - use Pixiedust's display function to visualise a JavaScript object
- store - turn a JavaScript array into a Pandas data frame

### print

```js
%%node
// connect to Cloudant using Silverlining
var url = 'https://reader.cloudant.com/cities';
var cities = silverlining(url);

// fetch number of cities per country
cities.count('country').then(print);
```

### display

```js
%%node

// fetch cities called York
cities.query({name: 'York'}).then(display);
```

### store

```js
%%node

// fetch the data and store in Pandas dataframe called 'x'
cities.all({limit: 2500}).then(function(data) {
  store(data, 'x');
});
```

The dataframe 'x' is now available to use in a Python cell:

```python
x['population'].sum()
```

## Managing the Node.js process

If enter some invalid syntax into a `%%node` cell, such as code with more opening brackets than closing brackes, then the Node.js interpreter may not think you have finished typing and you receive no output.

You can cancel execution by running the following command in a Python cell:

```python
node.cancel()
```

If you need to clear your Node.js variables and restart from the beginning then issue the following command in an Python cell:

```python
node.clear()
```

## Help

You can view the help either in Python:

```python
node.help()
```

or in a Node.js cell:

```js
%%node
help();
```