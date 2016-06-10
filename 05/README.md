# Class 5 - Web Scraping

+ What is HTML?
+ BeautifulSoup
+ Simple Example
+ How many adjuncts?

## HTML

+ HyperText Markup Language
+ HTML is primarily about positioning things on a page/how to make things appear visually, secondary for telling what is a header, title, etc.
+ The main unit in HTML is a **tag**:
    + Sample (contains a tag and **attributes** that are **key-value pairs**): `<TAG_NAME KEY=VALUE> </TAG`
    + Sample, bolding text: `<b>Hello</b>`
    + Sometimes we will say **element**, in place of tag

More sample code:
````
<ul>
    <li> one </li>
    <li> two </li>
    <li> three </li>
</ul>

````

+ `<ul>` in the code above is the **parent** of the `<li>` tags. The `<li>` tags are the **children** of the `<ul>` tag, and **siblings** of each other. **Descendants** include tags embedded inside the **children** tags.

+ An attribute is something you can include inside of a tag to tell it to do something extra.
+ Markup doesn't tell us about what the text means or what the data contains.
+ Web scraping is can sometimes be the only way to get the data you want.

Use the **web inspector tool** on your browser to see the tag associated with the item you're interested in.

## BeautifulSoup + simple example: kittens.html

Let's take a look at the [webpage](http://static.decontextualize.com/kittens.html) we'd like to scrape:

+ html has two children: **head** tag and the **body** tag
+ The body tag contains all of the elements that you see on the page.
+ Note that the names of all the kittens on this webpage are contained in `<h2>` tags. This is a hint for when you are scraping!

We're going to be installing and importing a Python library called BeautifulSoup to help us scrape websites.

On the command line, in a virtual environment, run `pip3 install bs4` -- you also have the option of doing this directly from inside your Jupyter notebook if using the following command: `!pip3 install bs4`.

**Check out the accompanying Jupyter notebook to see how we scraped [kittens.html](http://static.decontextualize.com/kittens.html).** The majority of the notes are contained within the Jupyter notebook -- for additional explanation on any of the concepts, consult [Allison's notes](https://github.com/ledeprogram/data-and-databases/blob/master/Scraping_HTML.ipynb) as always and Slack me if you have any questions!

## How many adjuncts...

To be continued next class!
