Hosted at [https://docs.catalyze.io/guides/api/](https://docs.catalyze.io/guides/api/)

## Local Running

Install gitbook first (`npm install -g gitbook`).

```
gitbook serve ./content
```

## Building

```
gitbook build ./content --output=./book
```

Always build before you push - saves us time later. It's probably also a good idea to update gitbook (`npm update -g gitbook`) before you do a build - it seems to change frequently.
