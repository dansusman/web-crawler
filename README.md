# Web Crawler

This is the source code for Project 5 of CS3700 Networks and Distributed Systems, authored by Dean Frame and Dan Susman Fall 2021.

## High-level Approach

To start exploring this project, we worked with Postman to make various GETs and POSTs to Fakebook. We also used the Dev Tools in Firefox/Chrome extensively to get our bearings in the project. Through investigations of Fakebook's BTS, we were able to figure out how the cookies worked, CSRF tokens, etc. and began drafting some code to log into Fakebook and react to the initial 302 Redirection. To learn the general HTTP header/body formatting, we used slides from class and experimentation with print() statements.

Overall, our code uses two sets, one for pages to visit and one for pages we have already explored. The Crawler object explores the pages in Fakebook hunting for `<a>` tags and secret flags. To parse for `<a>` tags, we use the HTMLParse library and to parse for secret flags we use regular expressions. When we find a link to visit, we add it to our set of pages to visit iff it is not an element of the seen_pages set. This ensures we only visit each page once.

Initially, we were using a Python deque as our pages_to_visit entity, but we switched to a set to see if it could improve efficiency slightly, and it did.

## Challenges Faced

One of the main challenges we faced was the reliability of the Fakebook server. We would run into strange errors such as receiving an empty byte array response from a socket, or a broken pipe error thrown by the socket, without knowing if the problems stemmed from our code or the server. When debugging these issues, however, we were still able to find and fix a number of other issues with our code, however.

Another problem we faced was the performance of the web crawler. We originally implemented the crawler with a deque. This is so that the list of URLs to visit was ordered, and we could choose to pop the first item in the deque or the last item in the deque. We compared this to breadth-first search and depth-first search, and figured we could try both approaches to see which one was faster. However, the performance of the crawler was still too slow for our liking with a deque. So, we decided to change the deque to a set. Since sets are not ordered, we knew that this meant that the performance of the crawler was randomly decided. After running it several times we found that the set implementation was on average faster than the deque implementation, so we decided to stick with the set implementation.

## Testing the Code

To test our web crawler, we ran the crawler on both the Khoury virtual machines as well as our Mac laptops. We additionally utilized many print statements (now removed) to debug our code. For example, we would print out the requests we were sending and receiving from Fakebook during the login process in order to determine if we were correctly managing cookies. 
