Here's the deal, JavaScript is the _only_ language that runs in the browser. (okay, there's [WebAssembly](https://webassembly.org/), but that's different). If your app is accessed via a web browser, it's almost certainly gonna need some JavaScript.

JavaScript has some things I don't like:

- Not statically typed (although [TypeScript](https://www.typescriptlang.org/) helps)
- Legacy baggage (like `var` and old code that doesn't use [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise))
- Weird quirks (like `==` vs `===`)
- Always acts slightly differently in every browser/runtime

But it has some serious things going for it:
- Almost every technology company uses it somehow, so there are a lot of jobs
- C-style syntax, so it's easy to learn if you know any other C-style language
- Many built-in features that are particularly useful on the web
- Can be used for both front-end and back-end development, simplifying an org's tech stack
- Big companies have invested a lot in making it better and faster over the years
- Great support for asynchronous programming