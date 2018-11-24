# Link-Crawler
Installation: pip install scrapy

Usage: scrapy crawl <spider-name> 

For our program Usage: scrapy crawl link_checker

The crawler program has placed in 'spiders' directory which can be viewed by: cd basic/spiders

Working of crawl spider program:

class MyItem(Item):
    url= Field()                                                            #Field is a scrapy dictionary - we will use it to 
                                                                            #store the response parsed

class MySpider(CrawlSpider):
    with open("2000urls.txt", "rt") as f:                                   #Open file from where URL is fetched by the spider
        start_urls = [url.strip() for url in f.readlines()]                 #Initiate start URL line by line of the file
    name = 'link_checker'                                                   #Spider name    
    rules = (Rule(LinkExtractor(), callback='parse_url', follow=True), )    #Link extractors are objects whose only purpose is 
                                                                            #to extract links from web pages. We need to declare 
                                                                            #some rules so that the spider works as intended, 
                                                                                  
                                                                               #callbck:  calls our function for each link of the start
                                                                               #URL and  returns a list containing Item and/or                                                                                          #Request objects (or subclasses of them). In our case,
                                                                               #the URLs and all the URLs residing in the webpage of 
                                                                               #the first URL
                                                                                  
                                                                               #follow=True is a boolean which specifies if links
                                                                               #should be recursively followed from each response
                                                                               #that are extracted  

    def parse_url(self, response):                                             #our function which recieves URL in the'respose'
                                                                               #argument
        f = open("2001urls.txt",'a+')                                          #open our source file in append mode
        item = MyItem()                                                            
        item['url'] = response.url                                             #parsing all the 'URL' fields from the response and 
                                                                               #write into the Field dictionary
        if "http" in item['url']:                                              #checking if http is present in the 'Field dictionary' 
                                                                               #-- http is specified to filter out garbage links 
            f.write(str(item['url'])+'\n')                                     #if http found write into file
        return item
