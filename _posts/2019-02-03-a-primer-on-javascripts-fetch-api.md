---
layout: post
title: A Primer on JavaScript’s Fetch API
---
<em>Note: This introductory guide is designed for relatively new JavaScript users.</em>

In this post, I will provide an overview of the Fetch API and demonstrate how to use the interface to execute a simple GET request. At a high level, the fetch() method enables JavaScript users to retrieve resources asynchronously.

The fetch() method takes in two parameters, the path of the resource, and an optional init object. The init object allows the user to further customize the request by specifying the request method, headers, and other properties. In the interest of keeping this guide succinct, I plan to cover POST and other requests in a future article.

Fetch() will return a Promise which resolves to a Response object. For those unfamiliar with the term, a Promise is essentially an object that is used to represent the outcome of an asynchronous action. It may or may not successfully return a value at a later point. In addition, the Response object contains data relevant to the server’s response (ex. status code).

As an illustrative example, I will make a request to the TVmaze API using fetch(). The TVmaze API provides various information about television shows and is also a great resource for demonstrating GET requests.

![Tvmaze](/assets/images/fetch1.png)

In the above snippet, I used the fetch() method to make a GET request to the TVmaze url and received a Promise which resolved to a Response object. For reference, in my requests, I am specifying that I want to retrieve data on all of the episodes that comprise the Futurama series.

Typically, JavaScript users will use the then() method to extract information from the resolved Promise. The then() method takes a callback function as an argument and is executed when the Promise resolves. Additionally, this method actually returns a Promise, which can be made visible to a subsequent then() method. Multiple then() methods are typically combined to create a Promise chain.

![fetch2](/assets/images/fetch2.png)

Now, appended to the initial fetch() request, I chained two then() methods (note that my callback functions use arrow function syntax — consult the <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions" target="_blank">documentation</a> if you are unacquainted with this). First, I utilized the json() method to parse the data as JSON. Next, I simply printed the extracted data to the console. In the second then() method, the data received from the API request can be easily transformed with vanilla JavaScript and can be used, for example, to add the retrieved information to a data store.

![fetch3](/assets/images/fetch3.png)

At this point, I have covered the basics of using the fetch() method to make a simple GET request to an API. I recommend that readers use my code as a template to attempt their own requests and explore the MDN documentation on the fetch() method.

## Sources
<a href="https://www.tvmaze.com/api" target="_blank">TVmaze API</a>

<a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch" target="_blank">MDN Documentation</a>
