![Preview](https://i.imgur.com/jvvnvXq.png)

# How It Works

This is a single-file browser app. The HTML, CSS, and JavaScript live in `index.html`; there is no build step, backend, API key, or package installation.

## Data Flow

When the page opens, JavaScript requests Apple’s public Books chart feed:

```text
https://rss.marketingtools.apple.com/api/v2/{country}/books/{chart}/10/books.json
```

For example, `us/books/top-paid/10` requests the US Top Paid chart. The country selector changes the storefront code, and the chart tabs select `top-paid` or `top-free`. Changing either control or pressing refresh sends a new request. An `AbortController` cancels an older request if a newer selection is made before it finishes.

The response is JSON. The app reads `feed.results` for the ranked books and `feed.updated` for the chart’s update time. Each result supplies the title, author, genre, cover-art URL, and Apple Books URL. JavaScript uses those fields to build the featured first result and the remaining ranked list. Cover images load lazily, with the smaller image URL used if a larger cover fails.

Search filters the already-loaded titles, authors, and genres in the browser; it does not make another API request. Loading, empty-search, and request-error states are handled in the page.

## Run It

Open `index.html` in a modern browser with an internet connection. The browser calls Apple directly, so a working connection and access to Apple’s feed and image hosts are required.

## Chart Notes

The feed provides Apple’s Top Paid and Top Free rankings, not the number of copies sold, prices, or reader ratings. The page shows up to 10 entries per chart. Rankings and availability can differ by country and may change as Apple updates its feed.
