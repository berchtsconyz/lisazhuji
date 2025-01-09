
# Beginner's Guide to Java Web Scraping: Using Jsoup to Scrape News From AutoHome

Web scraping is an essential skill for extracting and working with data from websites. In this guide, we’ll explore how to use **Jsoup**, a powerful Java library, to scrape news articles from **AutoHome** and organize the extracted data into usable formats.

---

## What is Jsoup?

**Jsoup** is a Java HTML parser that allows you to:

- Parse HTML directly from a URL, file, or string.
- Use **DOM** or **CSS selectors** to locate and extract data.
- Manipulate HTML elements, attributes, and text.

**Jsoup's Core Features:**

1. Parse HTML from a URL, file, or string.
2. Extract and manipulate data using DOM or CSS selectors.
3. Modify HTML elements, attributes, and text efficiently.
4. Open-source and released under the **MIT license**, making it suitable for commercial projects.

---

## Step 1: Project Overview

Here’s a preview of the web scraping project:

![Project Overview](http://www.demodashi.com/ueditor/jsp/upload/image/20170521/1495331216177085900.png)

We’ll build a Java program to fetch news articles from AutoHome using **Jsoup**, process the extracted data, and display it in a structured format.

---

## Step 2: Code Implementation

### Core Implementation

The main program, `GrapNews`, demonstrates how to scrape content from AutoHome. The process involves parsing the target URL, extracting relevant HTML elements, and converting the data into usable formats.

Here’s an overview of the core methods:

1. **getNewsFromCarHome**: Extracts news articles from AutoHome.
2. **replaceImgSrcFromDataSrc**: Handles image sources by replacing placeholders.
3. **removeHref**: Removes unnecessary hyperlinks from the extracted content.

### Example Code

```java
package net.sinolbs.ycd.news;

import java.net.URLEncoder;
import java.util.ArrayList;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

public class GrapNews {
    public static ArrayList<News> getNewsFromCarHome(int size, String baseUrl, String domainName, String newsListId,
                                                     String newsContentClass, String titleTag, String dateTag, String needDeleteAlt) {
        ArrayList<News> newsList = new ArrayList<>();
        Document doc;
        Elements elements;
        News news;
        try {
            doc = Jsoup.connect(baseUrl).timeout(10000).get();
            elements = doc.getElementById(newsListId).children();
            if (elements != null && elements.size() > 0) {
                for (Element ele : elements) {
                    news = new News();
                    Element title = ele.select("a").first();
                    if (title == null) continue;
                    
                    news.setTitle(title.getElementsByTag(titleTag).text());
                    if (news.getTitle() == null || news.getTitle().isEmpty()) continue;
                    
                    news.setHref(domainName + title.attr("href"));
                    if (dateTag != null) {
                        String date = ele.select(dateTag).text();
                        news.setDate(date);
                    }
                    
                    String newsUrl = URLEncoder.encode(news.getHref(), "utf-8").toLowerCase();
                    Document newsDoc = Jsoup.connect(newsUrl).timeout(10000).get();
                    String text = newsDoc.getElementsByClass(newsContentClass).html();
                    
                    news.setContent(text);
                    newsList.add(news);
                    
                    if (newsList.size() == size) break;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return newsList;
    }

    public static void main(String[] args) {
        getNewsFromCarHome(2, "http://m.autohome.com.cn/channel", "http://m.autohome.com.cn", "list", "details", "h4", "time", null);
    }
}
```

### Data Carrier Class

We use a **News** class to encapsulate the scraped data, including title, URL, content, and date.

```java
package net.sinolbs.ycd.news;

public class News {
    private String title;
    private String href;
    private String content;
    private String date;

    public News() {}

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getHref() {
        return href;
    }

    public void setHref(String href) {
        this.href = href;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }
}
```

---

## Step 3: Run and Visualize

When you run the program, it fetches news articles from AutoHome and outputs structured data, including titles, URLs, and content.

![Result Example](http://www.demodashi.com/ueditor/jsp/upload/image/20170521/1495332025782046370.png)

---

## Why Use ScraperAPI for Web Scraping?

Manually managing web scraping projects can be time-consuming and prone to IP blocks. ScraperAPI simplifies the process by handling proxies, CAPTCHAs, and dynamic content automatically.

👉 **Stop wasting time on proxies and CAPTCHAs! ScraperAPI makes it simple to scrape structured data from Amazon, Google, Walmart, and more.**  
[Start your free trial today!](https://bit.ly/Scraperapi)

ScraperAPI features include:
- Unlimited bandwidth
- 99.9% uptime guarantee
- Access to 40M+ IPs worldwide

---

## Advantages of Jsoup

1. **Ease of Use**: Its intuitive API allows developers to write simple, efficient scraping code.
2. **Customizability**: Fine-grained control over DOM manipulation and content extraction.
3. **Compatibility**: Works seamlessly with Java-based systems and applications.

---

## Ethical Considerations for Web Scraping

While web scraping is a powerful tool, always ensure that you:

- Adhere to website terms of service.
- Avoid scraping sensitive or private data.
- Respect the intellectual property of website owners.

---

## Conclusion

Using **Jsoup** to scrape websites is a great way to learn Java while building practical applications. Whether you're scraping data for analysis, research, or automation, Jsoup’s powerful API makes it a reliable choice.

For larger-scale scraping needs, [ScraperAPI](https://bit.ly/Scraperapi) offers a comprehensive solution to automate scraping tasks with ease. Sign up today and take your data collection efforts to the next level!
