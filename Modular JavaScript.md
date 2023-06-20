> A module is a self-contained piece of code that provides functions and methods that can then be used in other files and by other modules.

+ The code is kept into separate, reusable files which improves mantainabilty.
+ Helps make the code loosely coupled and interchangeable
+ Exact opposite of monolithic libraries
+ Allows specific uses
+ Avoiding wasting code
+ Allows API to be exposed, while keeping implementation hidden away

> It is considered good design to keep code as loosely coupled as possible as this allows for the most flexibility in developing systems of code, as different modules can be used independently and in a variety of different applications, rather than being restricted to a single use-case.

## ES6 Modules

Native support to modules has been added to ES6.

-   All code in modules is always in strict mode without the need for 'use strict' and there is no way to opt out of this.
-   A module has its own global scope, so any variables created in the top-level of a module can only be accessed within that module.
-   The value of `this` in the top level of a module is `undefined`, rather than the global object.
-   You can't use HTML-style comments in modules (although this isn't very common in any JavaScript program these days).

A ES6 module file is just a normal JavaScript file, but uses the keyword `export` to specify any values or functions that are to be made available from the module. *This highlights another important fact about modules – not everything in the module needs to be used.*

Very simple module is this code saved in `pi.js`
```javascript
export const PI = 3.1415926
```

How to import a module in `main.js`:
```javascript
import { PI } from './pi.js'
```

Functions can also be exported from a module. Example with `stats.js`:

```javascript
function square(x) {
    return x * x;
}

function sum(array, callback) {
    if(callback) {
        array = array.map(callback);
    }
    return array.reduce((a,b) => a + b );
}

function variance(array) {
    return sum(array,square)/array.length - square(mean(array))
}

function mean(array) {
    return sum(array) / array.length;
}

export {
    variance,
    mean
}
```


To import these functions into the `main.js` file, you’d add this line of code:
```javascript
import { variance, mean } from './stats.js'
mean([2,6,10])

// OR

import * as stats from './stats.js';

stats.mean([2,6,10])
```

## Default Exports

**Default exports** refer to _a single_ variable, function or class in a module that can be imported without having to be explicitly named.

```javascript
// Export a variable
const PI = 3.145926;

export default PI;

// Export a function
function square(x) {
    return x * x;
}

export default square;

// Export an object
const stats = {

    square(x) {
        return x * x;
    },

        sum(array, callback) {
        if(callback) {
            array = array.map(callback);
        }
            return array.reduce((a,b) => a + b );
        },

    mean(array) {
        return this.sum(array) / array.length;
    },

    variance(array) {
        return this.sum(array,this.square)/array.length - this.square(this.mean(array))
    }
}

export default stats;

```

> DO NOT USE more than one default export

Import the default exports:

```javascript
import PI from './pi.js';
import square from './square.js';
import stats from './stats.js';
```

Biggest difference is that you don't need the `{}` or make any mention of the value that is being imported. You can use an `alias` and use whatever name you prefer:

```javascript
import sq from './square.js';
```

## Node.js Modules
Node.js uses a different notation called [Common JS modules](http://wiki.commonjs.org/wiki/Modules/1.1).

A Common JS module is created in a separate file, and the `module.exports` method is used to make any functions available to other files, in a similar way to ES6 modules. 

For example, we could create a module for squaring numbers using the following code inside a file called `squareFunction.js`:

```javascript
module.exports = x => x * x;
```

To use the module, it needs to then be required inside the another JS file (or from within the Node REPL). This is done using the `require()` method. This takes the file that contains the module as an argument and returns the function that was exported:

```javascript
const square = require('./squareFunction')
```




