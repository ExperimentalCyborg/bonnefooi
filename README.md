# Bonnefooi

This npm package lets you access nonexistent object properties in javascript.

Bonnefooi is Dutch for going on a journey without planning ahead.

## Usage
`npm install bonnefooi`

When you try to access a nonexistent member, it will automatically be initialized as an empty object. This means you can safely do this:
```javascript
const bonnefooi = require('bonnefooi');

let myobj = {};
let myobjSafe = new bonnefooi(myobj);

myobjSafe.something.somethingelse.importantnumber = 123;
console.log(myobj); // Output: { something: { somethingelse: { importantnumber: 123 } } }
```

And you can even do this:
```javascript
const bonnefooi = require('bonnefooi');

let myobjSafe = new bonnefooi({});

myobjSafe.something.somethingelse.importantnumber = 123;
console.log(myobjSafe); // Output: { something: { somethingelse: { importantnumber: 123 } } }
```

## What it can't do

Beware that some code requires nonexistent properties to return `undefined`. For example, this will result in a stack overflow:
```javascript
const bonnefooi = require('bonnefooi');

let myobjSafe = new bonnefooi({});

myobjSafe.something.somethingelse.importantnumber = 123;
JSON.stringify(myobjSafe); // infinite recursion
```

But this works fine:
```javascript
const bonnefooi = require('bonnefooi');

let myobj = {};
let myobjSafe = new bonnefooi(myobj);

myobjSafe.something.somethingelse.importantnumber = 123;
JSON.stringify(myobj); // String: { something: { somethingelse: { importantnumber: 123 } } }
```

`myobjSafe.hasOwnProperty()` also does not work, because it's a proxy object which doesn't have an object prototype. If you want to check whether a property exists or not, refer to the original object, or use `in`:
```javascript
const bonnefooi = require('bonnefooi');

let myobjSafe = new bonnefooi({});
console.log('nonexistentproperty' in myobjSafe); // Output: false
```

## License
[GNU GPLv3 ](https://choosealicense.com/licenses/gpl-3.0/)
