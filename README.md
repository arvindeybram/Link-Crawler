# Link-Crawler
Installation: pip install scrapy

Usage: scrapy crawl <spider-name> 

For our program Usage: scrapy crawl link_checker

The crawler program has placed in 'spiders' directory which can be viewed by: cd basic/spiders

Working of crawl spider program:


    class MyItem(Item):
        url= Field()            (1)

    class MySpider(CrawlSpider):
        with open("2000urls.txt", "rt") as f:                                       (2)
            start_urls = [url.strip() for url in f.readlines()]                     (3)    
        name = 'link_checker'                                                       (4)
        rules = (Rule(LinkExtractor(), callback='parse_url', follow=True), )        (5)

        def parse_url(self, response):
            f = open("2001urls.txt",'a+')                                           (6)
        item = MyItem()                         
            item['url'] = response.url                                              (7)
            if "http" in item['url']:                                               (8)
                f.write(str(item['url'])+'\n')                                      (9)
            return item


1)  Field is a scrapy dictionary - we will use it to store the response parsed
2)  Open file from where URL is fetched by the spider
3)  Initiate start URL line by line of the file
4)  Spider name
5)  Link extractors are objects whose only purpose is to extract links from web pages. 
    We need to declare some rules so that the spider works as intended, 
    
    callbck:  calls our function for each link of the start URL and returns a list 
    containing Item and/or Request objects (or subclasses of them). In our case, 
    the URLs and all the URLs residing in the webpage of the first URL  
    
    follow=True is a boolean which specifies if links should be recursively followed 
    from each response that is extracted our function which receives URL in the 'respose' argument
                     
6)  open our source file in append mode
7)  parsing all the 'URL' fields from the response and write into the Field dictionary
8)  checking if http is present in the 'Field dictionary' -- http is specified to filter out garbage links 
9)  if http found write into file



