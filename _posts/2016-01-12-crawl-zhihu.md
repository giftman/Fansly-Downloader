#crawl

##scrapy

reference:

1 http://cuiqingcai.com/990.html 嗅事百科
2 http://blog.csdn.net/xingkongtianyuzhao/article/details/45937455 //z shell/get_net_proxy
3 http://www.lijiejie.com/crawl-and-verify-http-proxies-python/ 测试代理可用
4 http://www.maiziedu.com/course/python/458-7433/ scrapy lesson

response.css('.class h3 a::attr(href)')
response.css('#id 
选属性时用  ::text
respose.xpath('//  /text()')

lesson 1

`

    import scrapy

    class StackOverflowSpider(scrapy.Spider):
    name = 'stackoverflow'
    start_urls=[
        'http://www.zaobao.com/special/report/politic/fincrisis'
    ]
    
    def parse(self,response):
        #l_title > ul > li:nth-child(1) > a:nth-child(1)
        for ul in response.css('#l_title a::attr(href)'):
            full_url = response.urljoin(ul.extract())
            yield  scrapy.Request(full_url,callback=self.parse_url)

    def parse_url(self,response):
        yield {
            'title':response.css('#a_title h1::text').extract()[0],
            'dt':response.css('.time::text').extract()[0],
            'body':response.css('.a_body').extract()[0],
            'link':response.url
        }     
`

shell:

`
scrapy genspider [options] <name> <domain>
scrapy list
scrapy help/version
scrapy view
`

发布
scrapyd 安装
工程打包成egg //http://www.worldhello.net/2010/12/08/2178.html

上传增加版本 //https://scrapyd.readthedocs.org/en/latest/api.html#addversion-json

or 

scrapy-client //https://github.com/scrapy/scrapyd-client  直接帮你egg然后addversion了
