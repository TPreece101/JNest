<img src="https://raw.githubusercontent.com/TPreece101/JNest/master/JNest-logo-Final.gif"  width="360" height="162">

## Introduction

JNest is a useful collection of functions for traversing and performing operations on nested JSON objects, this library is particularly useful when dealing with tree diagrams or if you are working with nested data of any kind. For the examples in this documentation I will be using the [example.json](https://github.com/TPreece101/JNest/blob/master/example.json) file which I created for this purpose. The short version of the license is that you are able to include this library in any software you like as long as you keep the copyright notice in the js file. This library is pure Javascript and has no dependencies!

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
Example Output Node (root):
```json
{
  "Id": 1,
  "key1": 23,
  "key2": "global",
  "key3": 42,
  "sum": 65,
  "Array1": [],
  "Array2": [],
 } 
```

### Add Parent Property

This function cycles through the nested data and adds a "parent" property to each node which returns a reference to the node's parent, this can be useful if you later want to change properties based on a node's parent. Be wary though as this creates circular references in your object so if you wish to convert it to a string later then you must remove this property first.

#### Syntax

JNest.<b>addParentProperty</b>(object)

#### Example 

```js
var dataWithParent = JNest.addParentProperty(data);
```

Example Output Snippet:

```json
{
  "Id": 9,
  "key1": 39,
  "key2": "flavour",
  "key3": 18,
  "parent": {},
  "Array1": [
      {
        "Id": 10,
        "key1": 90,
        "key2": "clinic",
        "key3": 10,
        "parent": 
            {
              "Id": 9,
              "key1": 39,
              "key2": "flavour",
              "key3": 18,
              "parent": {},
              "Array1": []
            }
       }],
 } 
```

### Delete Property

This function cycles through the tree and deletes the property with the key that you define, this is useful for removing the parent property if you want to stingify your object.  

#### Syntax

JNest.<b>deleteProperty</b>(object, key)

#### Example 
In this example I am going to use it to remove the parent property and therfore removing the circular references.

```js
var dataWithoutParent = JNest.deleteProperty(data, "parent");
```

Example Output Snippet:

This would be the same as the original input.

### Add Level Count

This function cycles through the tree and adds a "level" property to each node which tells you what level of the tree that node is on, root node is 0 and all it's children 1 and their children 2 etc. This function adds the parent property at each node for it's own use, if leaveParent = "Y" then it will leave the parent property at each node, if it is anything else or excluded as below the parent property will be deleted.   

#### Syntax

JNest.<b>addLevelCount</b>(object, leaveParent)

#### Example 

```js
var dataWithLevels = JNest.addLevelCount(data);
```

Example Output Snippet:

```json
{
  "Id": 1,
  "key1": 23,
  "key2": "global",
  "key3": 42,
  "level": 0,
  "Array1": [
    {
      "Id": 2,
      "key1": 75,
      "key2": "frontier",
      "key3": 93,
      "level": 1,
      "Array1": [
        {
          "Id": 3,
          "key1": 32,
          "key2": "binding",
          "key3": 68,
          "level": 2,
          "Array1": [
            {
              "Id": 4,
              "key1": 35,
              "key2": "pilot",
              "key3": 62,
              "level": 3,
              "Array1": [
                {
                  "Id": 5,
                  "key1": 53,
                  "key2": "wolf",
                  "key3": 12,
                  "level": 4
                },
                {
                  "Id": 6,
                  "key1": 30,
                  "key2": "blend",
                  "key3": 80,
                  "level": 4
                },
                {
                  "Id": 7,
                  "key1": 37,
                  "key2": "noir",
                  "key3": 25,
                  "level": 4
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```
### Search By Key Value

This function cycles through the tree, finds all of the nodes where a specified key equals a specified value and returns these nodes in an array. This could be used to find all of the nodes on a certain level or find a node by it's ID. Due to the properties of Javascript any changes made to the resulting array will make changes to those nodes in the original object, this can be very useful.

#### Syntax

JNest.<b>searchByKeyValue</b>(object, key, value)

#### Example 

In this example, I am going to use the Search By Key Value function to find all of the nodes on level 3. Assume that the Add Level Count function has already been applied. 

```js
var resultArray = JNest.searchByKeyValue(data, 'level', 3);
```

Example Output Snippet:

```json
[
  {
    "Id": 4,
    "key1": 35,
    "key2": "pilot",
    "key3": 62,
    "key4": "apartment",
    "Array1": [
      {
        "Id": 5,
        "key1": 53,
        "key2": "wolf",
        "key3": 12,
        "key4": "duke",
        "level": 4
      },
      {
        "Id": 6,
        "key1": 30,
        "key2": "blend",
        "key3": 80,
        "level": 4
      },
      {
        "Id": 7,
        "key1": 37,
        "key2": "noir",
        "key3": 25,
        "level": 4
      }
    ],
    "level": 3
  },
  {
    "Id": 8,
    "key1": 58,
    "key2": "hop",
    "key3": 14,
    "level": 3
  }
]
```

### Search By Key

This function cycles through the tree, finds all of the nodes that has a specified property and returns these nodes in an array. Due to the properties of Javascript any changes made to the resulting array will make changes to those nodes in the original object, this can be very useful.

#### Syntax

JNest.<b>searchByKey</b>(object, key)

#### Example 

In this example, I am going to use the Search By Key function to find all of the nodes that have a property "key4".  

```js
var resultArray = JNest.searchByKey(data, 'key4');
```

Example Output Snippet:

This will be similar to the previous function but would be a lot longer in this case due to all of the arrays of children so I see no need in showing it here.
