---
layout: post
title: "用chatgpt编写爬虫"
category: daily
---

最近需要从一个网站爬取语料，用 chatGPT 写了一个爬虫，发现 chatGPT 的 Python 代码生成是真滴强。

只需要准确描述好需求，chatGPT 的代码分毫不差，生成后不需要任何修改就可以执行。

prompts:

```
请你帮我用 Python 写一个爬虫。需求如下：

网站的导航页，是这样的
https://xinxihua.bjx.com.cn/yw/1/
https://xinxihua.bjx.com.cn/yw/2/
https://xinxihua.bjx.com.cn/yw/3/
https://xinxihua.bjx.com.cn/yw/4/

一直到第 50 页，每一页都列举了很多文章。

爬取每一个导航页，然后解析 class="cc-list-content" 的 div，该 div 下面是一个 <ul></ul> 表格，表格内每一个 <li></li> 都是一个 <a></a>的超链接，超链接的 title 是文章标题，href 是文章的具体地址。

根据 href 爬取导航页的每一篇文章，定位到 class="cc-article" 的 div，该 div 下面的每一个 <p></p> 的内容，都是文章的内容，每一个 <p></p> 的内容，作为一行，合并为一篇文章。

就这样，爬取所有导航页的所有文章。

注意，每爬取完一篇文章，等待两秒。

请求携带如下 header 信息：


authority: xinxihua.bjx.com.cn
method: GET
path: /yw/1/
scheme: https
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8,en-US;q=0.7,en;q=0.6
Cache-Control: no-cache
Cookie: bjx_mt:uuid=7686974a-d853-4a45-8345-95bdf6fae7f8; Hm_lvt_797e95e42c7a8bdc8814749cbcddd277=1710232582; bjx_mt:nonce=6747386450; Hm_lvt_db82935ada12c5131d00c95e176beb1f=1710731093; Hm_lpvt_797e95e42c7a8bdc8814749cbcddd277=1710742679; Hm_lpvt_db82935ada12c5131d00c95e176beb1f=1710742679
Pragma: no-cache
Sec-Ch-Ua: "Chromium";v="122", "Not(A:Brand";v="24", "Google Chrome";v="122"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36

最后将爬取到的文章，存储到当前目录下的 news 目录下，txt 文件名为文章名，txt文件内容为文章的内容，如果文章名中有需要转义的字符，需要处理一下。

```

output:

```python
import requests
import time
import re
from bs4 import BeautifulSoup


def get_article_content(article_url, headers):
    response = requests.get(article_url, headers=headers)
    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        article_content = soup.find('div', class_='cc-article')
        if article_content:
            paragraphs = article_content.find_all('p')
            article_text = '\n'.join(p.text.strip() for p in paragraphs)
            return article_text
    return None


def scrape_navigation_pages(base_url, start_page, end_page, headers):
    for page_num in range(start_page, end_page + 1):
        page_url = f"{base_url}{page_num}/"
        print(page_url)
        response = requests.get(page_url, headers=headers)
        print(response)
        if response.status_code == 200:
            soup = BeautifulSoup(response.text, 'html.parser')
            article_links = soup.find_all('div', class_='cc-list-content')
            for link in article_links:
                for a in link.find_all('a'):
                    article_title = a.get('title')
                    article_href = a.get('href')

                    article_content = get_article_content(article_href, headers)
                    if article_content:
                        article_data = {
                            'title': article_title,
                            'content': article_content
                        }
                        print(article_data)
                        article_title = re.sub(r'[\/:*?"<>|]', '-', article_title)
                        file_name = f"news/{article_title}.txt"
                        with open(file_name, 'w', encoding='utf-8') as f:
                            f.write(article_content)
                        time.sleep(2)


if __name__ == '__main__':
    base_url = 'https://xinxihua.bjx.com.cn/yw/'
    start_page = 1
    end_page = 50
    headers = {
        'authority': 'xinxihua.bjx.com.cn',
        'method': 'GET',
        'scheme': 'https',
        'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
        'Accept-Encoding': 'gzip, deflate, br, zstd',
        'Accept-Language': 'zh-CN,zh;q=0.9,zh-TW;q=0.8,en-US;q=0.7,en;q=0.6',
        'Cache-Control': 'no-cache',
        'Cookie': 'bjx_mt:uuid=7686974a-d853-4a45-8345-95bdf6fae7f8; Hm_lvt_797e95e42c7a8bdc8814749cbcddd277=1710232582; bjx_mt:nonce=6747386450; Hm_lvt_db82935ada12c5131d00c95e176beb1f=1710731093; Hm_lpvt_797e95e42c7a8bdc8814749cbcddd277=1710742679; Hm_lpvt_db82935ada12c5131d00c95e176beb1f=1710742679',
        'Pragma': 'no-cache',
        'Sec-Ch-Ua': '"Chromium";v="122", "Not(A:Brand";v="24", "Google Chrome";v="122"',
        'Sec-Ch-Ua-Mobile': '?0',
        'Sec-Ch-Ua-Platform': '"macOS"',
        'Sec-Fetch-Dest': 'document',
        'Sec-Fetch-Mode': 'navigate',
        'Sec-Fetch-Site': 'none',
        'Sec-Fetch-User': '?1',
        'Upgrade-Insecure-Requests': '1',
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36'
    }
    scrape_navigation_pages(base_url, start_page, end_page, headers)

```

