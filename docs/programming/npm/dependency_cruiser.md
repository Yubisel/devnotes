# Dependency cruiser

Easily visualize a project's dependency graph with dependency-cruiser

You can install `dependency-cruiser` globally with `npm i -g dependency-cruiser`. Then, in the folder of any project you care about, you can run:

```bash
depcruise --exclude "^node_modules" --output-type dot src | dot -T svg > dependencygraph.svg
```

assuming the core of your code lives in the `src` sub folder