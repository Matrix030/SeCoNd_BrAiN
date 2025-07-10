There is _always_ a trade-off.

## Pros for Monoliths

- Simpler to get started with
- Easier to deploy new versions because everything is always in sync
- In the case of the data being embedded in the HTML, the performance can result in better UX and SEO

## Pros for Decoupled Architectures

- Easier, and probably cheaper, to scale as traffic grows
- Easier to practice good separation of concerns as the codebase grows
- Can be hosted on separate servers and using separate technologies
- Embedding data in the HTML is still possible with pre-rendering (similar to how Next.js works), it's just more complicated

## Can We Have the Best of Both Worlds?

Perhaps. My recommendation to someone building a new application from scratch would be to start with a monolith, but to keep the API and the front-end decoupled logically within the project from the start (like we're doing with Chirpy).

That way, our app is easy to get started with, but we can migrate to a fully decoupled architecture later if we need to.

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: monoliths vs decoupling