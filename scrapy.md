# Issue with parsing a `br` in the middle of achor tag text

This is the html:

`<a href="https://goo.gl/maps/CeHJFchN5YPZU9VQ8">2825 Adelaida Road<br>Paso Robles, CA 93446</a>`

I tried multiple solutions, but kept getting an error from scrapy that said scrapy could not process the address. The solution that finally worked for me was loading two address items into the item loader. Scrapy's item loader will append the response to the second item to the response to the first item.

`l.add_xpath(

​                "address",

​                "//*[@id='single-blocks']/div/div[1]/div/div[1]/div/div[2]/div[1]/p[2]/a/text()[1]",

​            )

  l.add_xpath(

​                "address",

​                "//*[@id='single-blocks']/div/div[1]/div/div[1]/div/div[2]/div[1]/p[2]/a/text()[2]",

​            )`

Since even the result of the first address load is stored in MongoDB as an array, the second address load just adds the second response as the next item in the array.