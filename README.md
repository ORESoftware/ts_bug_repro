

## Project showing a potential bug in tsc transpilation

In package_a, we have a tsconfig.json file with these typeRoots:

```
    "typeRoots": [
      "./node_modules/@types",
      "./node_modules/@oresoftware/package_b/dts"
    ]

```





