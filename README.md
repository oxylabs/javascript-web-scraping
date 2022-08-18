# javascript-web-scraping
JavaScript Web Scraping


```bash
npm init -y
```

```bash
npm install axios
```

```bash
npm install axios cheerio json2csv
```

```css
#content_inner > article > div.row > div.col-sm-6.product_main > h1
```

```css
h1
```

```javascript
const cheerio = require("cheerio");
const axios = require("axios");
```

```javascript
const url = "http://books.toscrape.com/catalogue/category/books/mystery_3/index.html";
```

```javascript
const response = await axios.get(url);
```

```javascript
const response = await axios.get(url, {
      headers: 
      {
        "User-Agent": "custom-user-agent string",
      }
    });
```

```javascript
const $ = cheerio.load(response.data);
```

```javascript
const genre = $("h1").text();
```

```javascript
console.log(genre);
```

```javascript
const cheerio = require("cheerio");
const axios = require("axios");
const url = "http://books.toscrape.com/catalogue/category/books/mystery_3/index.html";

async function getGenre() {
  try {
    const response = await axios.get(url);
    const document = cheerio.load(response.data);
    const genre = document("h1").text();
    console.log(genre);
  } catch (error) {
    console.error(error);
  }
}
getGenre();
```

```javascript
node genre.js
```

```javascript
Mystery
```

```javascript
const books = $("article"); //Selector to get all books
books.each(function () 
           { //running a loop
		title = $(this).find("h3 a").text(); //extracting book title
		console.log(title);//print the book title
			});
```

```javascript
const cheerio = require("cheerio");
const axios = require("axios");
const mystery = "http://books.toscrape.com/catalogue/category/books/mystery_3/index.html";
const books_data = [];
async function getBooks(url) {
  try {
    const response = await axios.get(url);
    const $ = cheerio.load(response.data);
 
    const books = $("article");
    books.each(function () {
      title = $(this).find("h3 a").text();
      price = $(this).find(".price_color").text();
      stock = $(this).find(".availability").text().trim();
      books_data.push({ title, price, stock }); //store in array
    });
    console.log(books_data);//print the array
  } catch (err) {
    console.error(err);
  }
}
getBooks(mystery);
```

```bash
node books.js
```

```javascript
if ($(".next a").length > 0) {
      next_page = baseUrl + $(".next a").attr("href"); //converting to absolute URL
      getBooks(next_page); //recursive call to the same function with new URL
}
```

```javascript
const baseUrl ="http://books.toscrape.com/catalogue/category/books/mystery_3/"
```

```bash
npm install json2csv
```

```javascript
const j2cp = require("json2csv").Parser;
```

```javascript
const fs = require("fs");
```

```javascript
const parser = new j2cp();
const csv = parser.parse(books_data); // json to CSV in memory
fs.writeFileSync("./books.csv", csv); // CSV is now written to disk
```

```javascript
const fs = require("fs");
const j2cp = require("json2csv").Parser;
const axios = require("axios");
const cheerio = require("cheerio");
 
const mystery = "http://books.toscrape.com/catalogue/category/books/mystery_3/index.html";
 
const books_data = [];
 
async function getBooks(url) {
  try {
    const response = await axios.get(url);
    const $ = cheerio.load(response.data);
 
    const books = $("article");
    books.each(function () {
      title = $(this).find("h3 a").text();
      price = $(this).find(".price_color").text();
      stock = $(this).find(".availability").text().trim();
      books_data.push({ title, price, stock });
    });
    // console.log(books_data);
    const baseUrl = "http://books.toscrape.com/catalogue/category/books/mystery_3/";
    if ($(".next a").length > 0) {
      next = baseUrl + $(".next a").attr("href");
      getBooks(next);
    } else {
      const parser = new j2cp();
      const csv = parser.parse(books_data);
      fs.writeFileSync("./books.csv", csv);
    }
  } catch (err) {
    console.error(err);
  }
}
 
getBooks(mystery);
```

```bash
node books.js
```


