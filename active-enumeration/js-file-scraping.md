# Scraping\(JS/Source code\)





Javascript files are used by modern web applications providing dynamic content which contains various functions & events. Each website's processes its Javascript files are a great resource for finding those internal subdomains used by the organization. Mostly JS files 





## Tools:

### 1\) Gopsider

* Author: [Jaeles](https://github.com/jaeles-project)
* Language: Go

[**Gospider**](https://github.com/jaeles-project/gospider) is a fast web spidering tool capable of crawling the whole website within in a short amount of time. This means gospider will visit/scrap each and every URL mentioned in the JS file and source code. So, since source code & JS files makeup a website they may contain links to other subdomains too. 

### Installation:

```bash
go get -u github.com/jaeles-project/gospider
```

**This is a long process so Brace yourself !!!** 

### Running:

* Since we are crawling a website, gospider excepts us to provide URL's, means in the form of `http://`  `https://` 
* So, first we need to web probe all the subdomains we have gathered till now. For this purpose we will use [**httpx**](https://github.com/projectdiscovery/httpx) .
* So, lets first web probe the subdomains:

```bash
cat subdomains.txt | httpx -random-agent -retries 2 -no-color -o probed_tmp_scrap.txt
```

* Now, that we have web probed URL's, we can send them for crawling to gospider.

```bash
gospider -S probed_tmp_scrap.txt --js -t 50 -d 3 --sitemap --robots -w -r > gospider.txt
```

{% hint style="danger" %}
**Caution**: This generates a huge traffic on your target 
{% endhint %}

#### Flags:

* **S** - Input file
* **js** - Find links in JavaScript files
* **t** -  Number of threads \(Run sites in parallel\) \(default 1\)
* **d** - depth \(3 depth means scrap links from second-level JS files\)
* **sitemap** -  Try to crawl sitemap.xml
* **robots** - Try to crawl robots.txt

> six2dez explain this
>
> ```bash
> sed -i '/^.\{2048\}./d' /gospider.txt
> ```

 Point to note here is we have got URLs from JS files & source code till now. We are only concerned with subdomains. Hence we must just extract subdomains from the gospider output.

This can be done using Tomnomnom's [unfurl ](https://github.com/tomnomnom/unfurl) tool. It takes in a list of URLs and ecxtract the subdoamin/domain part from them.

```bash
go get -u github.com/tomnomnom/unfurl
 | grep -Eo 'https?://[^ ]+' | sed 's/]$//' | unfurl -u domains | grep ".example.com$" | anew scrap_subs.txt
```

