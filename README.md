## Spider File - example_org.py
```Python
import scrapy
from scrapy.loader import ItemLoader
from examplebot.items import EventItem

class MySpider(scrapy.Spider):
    name = "example.org"

    def start_requests(self):
        # this example only has a single url for the main calendar
        # yields the output from the main calendar link
        url = 'https://www.example.org/'
        yield scrapy.Request(url=url, callback=self.parse)

    def parse(self, response):
        # extract events from the response
        events = response.css('.event')
        # loop through events
        for event in events:
            # extract the event url and and process
            url = event.xpath('@href').get()
            yield scrapy.Request(url=url, callback=self.extract)

    def extract_event(self, response)
        # results from the event will be stored in a Scrapy Item
        l = ItemLoader(item=EventItem(), response=response)))
        # add results for relevant fields into the Item Loader using
        # add_xpath, add_value or 
        # missing fields
        return l.load_item() 
 ```
 
## EventItem definition - items.py
 ```Python
 import scrapy
from scrapy.loader.processors import TakeFirst, MapCompose, Join

def remove_none_empty(value):
    if value == "None" or value == "":
        return None
    else:
        return value
        
 class EventItem(scrapy.Item):
    event_type          = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_title         = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_venue         = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_venue_address = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_image         = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_description   = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_start_date    = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_end_date      = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_ticket_price  = scrapy.Field(input_processor=MapCompose(remove_none_empty), output_processor=Join())
    event_ticket_url    = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_organizer     = scrapy.Field(input_processor=MapCompose(remove_none_empty), output_processor=TakeFirst()) 
    event_org_tel       = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=TakeFirst())
    event_org_email     = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_url           = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=Join())
    event_category      = scrapy.Field(input_processor=MapCompose(str.strip), output_processor=TakeFirst())
 ```
