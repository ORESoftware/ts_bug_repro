

## Project showing a potential bug in tsc transpilation

In package_a, we have a tsconfig.json file with these typeRoots:

```
    "typeRoots": [
      "./node_modules/@types",
      "./node_modules/@oresoftware/package_b/dts"
    ]

```


package_a depends on package_b
(they are published to @oresoftware/package_a and @oresoftware/package_b, but you don't need to use NPM to reproduce the problem)


### Here is the problem

here is the source structure for:

```
/src
  a.ts
```

and a.ts looks like:

```
import * as x from 'lodash';

export {Foo} from '@oresoftware/package_b/dts/b'

console.log(x);
```


if you go into package_a, and run `tsc`, you will get this dist structure:

```
dist/
  a.d.ts
  a.js
```


and a.js looks like this:


```js
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const x = require("lodash");
var b_1 = require("@oresoftware/package_b/dts/b");
exports.Foo = b_1.Foo;
console.log(x);
```


The *problem* is it's trying to load:

```
var b_1 = require("@oresoftware/package_b/dts/b");
```

at runtime, even though this is a .d.ts file!

My guess is that `tsc` just guesses that it's loadable,
because it could not find the file at compile time.


How to fix the problem!

If you cd into package_a and run `npm install`, and then run `tsc`, the problem goes away,
we now have this dist/a.js file:

```js
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const x = require("lodash");
console.log(x);
```

so, idk if this is a bug or not.
