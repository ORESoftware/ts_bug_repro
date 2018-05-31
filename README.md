

## Project showing a potential bug in tsc transpilation

In package_a, we have a tsconfig.json file with these typeRoots:

```
    "typeRoots": [
      "./node_modules/@types",
      "./node_modules/@oresoftware/package_b/dts"
    ]

```


package_a depends on package_b

(they are published to @oresoftware/package_a and @oresoftware/package_b)


### Here is the problem

here is the source structure:

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


