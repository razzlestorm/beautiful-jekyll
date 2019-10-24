---
layout: post
title: Using Natural Language Processing to Predict Topics of Articles (now in Chinese!)
subtitle: From the Official Chinese News, 新闻联播
gh-repo: razzlestorm/razzlestorm.github.io
gh-badge: [star, fork, follow]
tags: [NLP]
comments: true
---

![Xinwen Lianbo Header](razzlestorm.github.io/img/header.jpg)

## A Look into the Chinese Media
I have a complicated relationship with the Chinese media. I've done work for CCTV in the past, and most of my translation clients have some sort of government ties.
Overall, I think that the daily news (新闻联播) that gets presented on CCTV is generally objective when it comes to international news. However, when it comes to domestic Chinese news, there can be more of an agenda. Having been a regular consumer of 新闻联播 before, I'm well aware of how the program will often shape a story to be very pro-China, and will often do fluff pieces on important figures.
But as a Data Scientist, I wanted to perform an exploration of this, and see if there was actually any recognizable trends in how many fluff pieces (for Xi Jinping in particular) are produced over time.

## The Dataset
The dataset I'm using was found on ![Kaggle](https://www.kaggle.com/noxmoon/chinese-official-daily-news-since-2016), and features every written news article published for use on the 新闻联播 program between January 1, 2016 to October 15, 2018. It has 4 columns: Date, Tag, Headline, and Content.
The Tag column differentiates the rows in the dataset between "“详细全文” some relatively long news, "国内“ domestic short news, "国际“ international short news."

## Project Goals:
1. Explore whether there is any sort of trend in positive news/fluff pieces about Xi Jinping (especially surrounding the time of the constitutional change).
2. Come up with a way to predict whether a piece will be about Xi Jinping.
3. Create a filter for fluff pieces about Xi Jinping (sometimes it can feel like you are being bombarded by them) (Future Work)

## Data Exploration and Code
To get started, here's a list of all the imports that were used in this project:
![List of Imports](razzlestorm.github.io/img/Build1_imports.png)
Most of these are fairly standard for NLP, but one package that some might be unfamiliar with is ![jieba](https://github.com/fxsjy/jieba). Jieba is a popular tokenizer specifically for Chinese characters, and typically does a great job parsing which should be compound characters and which should not. It also allows you to add custom tokenization rules pretty easily, if something isn't parsing quite right.

And this is typically what the data looked like:
![Example Row](razzlestorm.github.io/img/Build1_Example_Row.png)
It had 20,738 rows with only 100 or so stories that repeated. Since these were all stories that did actually air, I decided to leave them in.

**Non-NLP Feature Engineering**
Before getting to the NLP aspect of the project, I wanted to see if there was perhaps an increase in Xi Jinping-friendly stories surrounding times when there may typically be negative public sentiment about him (specifically in March, when he changed to constitution to remove presidential term limits). This required me to "manually" determine if each news story was about Xi, an arduous task with over 20,000 rows of data.
So I counted the ratio of Xi Jinping (or Chairman Xi, Secretary Xi, etc.) mentions to the character count of an article, getting the percentage he was mentioned, and cross-referenced articles with high "Xi counts" with whether or not he is mentioned in the title. 
![Xi Count Example](razzlestorm.github.io/img/Build1_xi_count.png)
This was typically a good enough measure for whether an article was truly about Xi or not, which allowed me to tag it (for later use by NLP classifiers). I did do a manual count of about 2000 rows, and found 33 false positives. Logically, I didn't see how an article could be about Xi Jinping without actually mentioning him, so wasn't worried about articles with a Xi count of 0.0, and 33/2000 false positives is a good enough error that the rule seems alright.

**Results**
![Graph of Xi Jinping Mentions](razzlestorm.github.io/img/Build1_xi_graph.png)
![Ratio of Xi Mentions](razzlestorm.github.io/img/Build1_xi_ratio.png)



You can write regular [markdown](http://markdowntutorial.com/) here and Jekyll will automatically convert it to a nice webpage.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](http://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

**Here is some bold text**

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .center-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
