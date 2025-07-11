Query parameters in a URL are _not_ always present. In the context of websites, query parameters are often used for marketing analytics or for changing a variable on the web page. With website URLs, query parameters _rarely_ change _which_ page you're viewing, though they often will change the page's _contents_.

That said, query parameters can be used for anything the server chooses to use them for, just like the URL's path.

## How Google Uses Query Parameters

1. Open a new tab and go to [https://google.com](https://google.com).
2. Search for the term "hello world"
3. Take a look at your current URL. It should start with `https://www.google.com/search?q=hello+world`
4. Change the URL to say `https://www.google.com/search?q=hello+universe`
5. Press "enter"

You should see new search results for the query "hello universe". Google chose to use query parameters to represent the value of your search query. It makes sense - each search result page is _essentially_ the same page as far as HTML structure and CSS styling are concerned - it's just showing you different results based on the search query.