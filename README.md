# Welcome to TripUp Developer Documentation

This site discribs the possibility to integrate with our sites.

my-cruise-excursion.com (co.uk) or meine-landausfluege.de (.ch)

We have got 4 intergation types for our partners, those are disribed bellow. 
In the navigation menu above you can quick jump to the integration type you are interessted in.

## Affiliate links
There are 2 types of partner links.
- A customer link can be sent to customer, so we can allocate the commission to a
partner at the booking time. It looks as following: 
   
  https://<i></i>my-cruise-excursion.co.uk?customer=YourPartnerID
   
- A partner link can be used by travel agent to make a booking. As partner visitor you have some features on the website, those are disabled for customers. It looks as following:
  
  https://<i></i>my-cruise-excursion.co.uk?partner=YourPartnerID

#### Link parameters

An customer affiliet link can include GET paramters described in the following table.

| parameter | | description | example
| --- | --- | --- | ---
| customer | required | Your Partner-ID | TA123
| coupon | optional | You can set a coupon code to make some sale | TA123-10p-ASDF
| channel | optional | You can use this field to send some additional information, that will be shown in your reports | My-Custom-Channel

For the deeplinking to our itinerary page you have to set the cruise route by sending paramters in the following table additinaly to the parameters above.

| parameter | | description | example
| --- | --- | --- | ---
| ship | required | Cruise ship name | *Azamara Pursuit*
| date | required | Cruise date | *2019-04-06* Format: YYYY-MM-DD
| duration | required | Cruise duration in days | 7

#### Domains

We have serveral shops for different markets. Please choose correct shop for your localtion. 

| shop | currency | language 
| --- | --- | ---
| my-cruise-excursion.com | USD | ENG
| my-cruise-excursion.co.uk | GBP | ENG
| meine-landausfluege.de | EUR | GER
| meine-landausfluege.ch | CHF | GER

## Simple Widget
The simple widget allow you to show a subset of products on an itinerary or in a port.
Please follow this [link](https://tripup-company.github.io/widget.html) to learn more.

## Itinerary Widget
This is the new version of widget with itinerary navigation and full product listing.
Please follow this [link](https://tripup-company.github.io/itinerary.html) to learn more.

## REST API
The REST API's allow you to use our data for cusom integration.
Please follow this [link](https://tripup-company.github.io/api.html) to learn more.
