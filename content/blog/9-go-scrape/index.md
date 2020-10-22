---
title: "Web Scraping and Language Detection with Go"
date: "2020-10-22"
description: "How to scrape a webpage and detect the language with Go"
---
Recently I've been experimenting with Go - below is a simple script I used to scrape some text from a webpage with the help of [goquery](https://github.com/PuerkitoBio/goquery). I then determine the scraped text's language with [whatlanggo](https://github.com/abadojack/whatlanggo). Markdown seems to do some weird formatting with Go - apologies for the inconsistent indentation.

```go
package main

import (
	"fmt"
	"log"
	"net/http"

	"github.com/PuerkitoBio/goquery"
  "github.com/abadojack/whatlanggo"
)

func main() {
	someText, err := GetSomeText("https://www3.nhk.or.jp/news/")
	if err != nil {
		log.Println(err)
	}
	fmt.Println("Text:", someText)
  info := whatlanggo.Detect(someText)
  fmt.Println("Language:", info.Lang.String(), "|", "ISO:", info.Lang.Iso6391(), "|", "Confidence:", info.Confidence)
}

// GetSomeText gets text using a selector and returns it.
func GetSomeText(url string) (string, error) {

	// Get the HTML
	resp, err := http.Get(url)
	if err != nil {
		return "", err
	}

	// Convert HTML into goquery document
	doc, err := goquery.NewDocumentFromReader(resp.Body)
	if err != nil {
		return "", err
	}

	// Gets first text from given element/selector
  text := doc.Find("a").First().Text()
	return text, nil
}
```

If you save the above (e.g. as `scrapelang.go`) and run
```bash
go run scrapelang.go
```
in your terminal, then you should see results that look like this

```bash
Text: ページの先頭へ戻る
Language: Japanese | ISO: ja | Confidence: 1
```

The script is also on my [GitHub](https://github.com/ezramizrahi/scrape_lang).
