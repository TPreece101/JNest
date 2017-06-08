<img src="https://raw.githubusercontent.com/TPreece101/JNest/master/JNest-logo-Final.gif"  width="360" height="162">

## Introduction

JNest is a useful collection of functions for traversing and performing operations on nested JSON objects, this library is particularly useful when dealing with tree diagrams or if you are working with nested data of any kind. For the examples in this documentation I will be using the [example.json](https://github.com/TPreece101/JNest/blob/master/example.json) file which I created for this purpose. The short version of the license is that you are able to include this library in any software you like as long as you keep the copyright notice in the js file.

## Installation

Copy the [JNest.v1.min.js](https://raw.githubusercontent.com/TPreece101/JNest/master/JNest.v1.min.js) file to your project folder and add the following to the top of your HTML document 

```html
<script src="JNest.v1.min.js"></script>
```
I will get round to writing a node package when I find the time.

## Usage With Examples 

### Apply

This function is used to apply a function of your choice to each node in the nested object, this means that you have a lot of freedom to perform custom operations, if you find yourself doing one operation in particular a lot then feel free to open an issue and I can include a wrapper in the next release.  

#### Syntax

JNest.<b>apply</b>(object, function)

#### Example 

In this example I am going to create a new value with key "sum" at each node which will be the sum of the values under "key1" and "key3" at each node.

```js
var transformedData = JNest.apply(data, function(d) { d.sum = d.key1 + d.key3 });
```
Output:
```json
{
  "Array1": [],
  "Array2": [],
  "Id": 1,
  "key1": 23,
  "key2": "global",
  "key3": 42,
  "sum": 65
 } 
```

