---
title: Implementing Pagination using Jade and Bootstrap
layout: single
author: Hamza
comments: true
---

A few weeks ago while building a simple node.js app, I had to display a really long list of items. 
Pagination had to come in at some point. Since I was using Bootstrap, I had to use its pagination template. 
However, manipulating it directly wasn't so straightforward. Below is my implementation which you can 
use by simply copying, pasting and switching key variables.

* _recordsPerPage_ = maximum number of records displayed per page
* _count_ = total number of records
* _base_url_ = the base URL
* _pageButtonCount_ = the number of page buttons to show (3 in my case [1] [2] [3]) 
* _pageUrl_ = link to each page. It is formed from the base url and the page number of the page in question eg. `base_url + "?count=1&page=" + paginationNo`
* _page_ = the current page number

---

{% highlight jade %}
-var count = [INSERT VALUE HERE]
-var recordsPerPage = [INSERT VALUE HERE]
-var pageButtonCount = [INSERT VALUE HERE]
-var base_url = [INSERT VALUE HERE]
-var page = [INSERT VALUE HERE]
-var recordsPerPage = [INSERT VALUE HERE]

-var lower_limit = (parseInt((page - 1)/pageButtonCount) * pageButtonCount) + 1
-var upper_limit = lower_limit * pageButtonCount;
-var paginationNo = lower_limit - 1

if count >= recordsPerPage
    nav(aria-label='...', style='text-align: center')
        ul.pagination
            -if (lower_limit > 1)
                li.page-item
                    -var pageUrl =  base_url + "?page=" + (lower_limit - pageButtonCount); [MODIFY THIS TO SUIT YOUR USAGE]
                    a.page-link(href=pageUrl, tabindex='-1') Previous
            -else
                li.page-item.disabled
                    a.page-link(href='#', tabindex='-1') Previous
            -for (i = 1; i < pageButtonCount + 1; i++)

                -if (count > (recordsPerPage * paginationNo))
                    -paginationNo = paginationNo + 1
                    -var pageUrl =  base_url + "?count=1&page=" + paginationNo; [MODIFY THIS TO SUIT YOUR USAGE]
                        -if (page != paginationNo)
                            li.page-item
                                a.page-link(href=pageUrl) #{paginationNo}
                        -else
                            li.page-item.active
                                a.page-link(href='#')
                                    | #{paginationNo}
                                    span.sr-only (current)
            - if (count > (recordsPerPage * upper_limit))
                -var pageUrl = base_url + "?count=1&page=" + (upper_limit + 1) [MODIFY THIS TO SUIT YOUR USAGE]
                li.page-item
                    a.page-link(href=pageUrl) Next
            -else
                li.page-item.disabled
                    a.page-link(href='#', tabindex='-1') Next
{% endhighlight %}

A simple demo is shown below
{:refdef: style="text-align: center;"}
![jade bootstrap pagination demo]({{ site.url }}/assets/images/paginagif.gif)
{: refdef}

