# TripUp Itinerary Widget

This widget is a frontend client that uses TripUp Cruise and Booking APIs.

[**See here our Demo Widget** ](https://widget.meine-landausfluege.de/itinerary/iframe.html)

[**Try Our Code Generator**](https://widget.meine-landausfluege.de/widget-generator/index.html)

## Getting Started

There are two option how you can use this widget: using it as an iframe or bind it directly as a script into your page. 
The simplest and most secure way to use this widget is to paste the iframe code into your html (see iframe description below). It works well with every site, even with CMS systems. If you use widget in an iframe you can not modify anything inside the iframe. The secound option is more advanced and needs more technical skills, but it allows you to change style inside the widget.

#### Domain

We have several domains for different markets. By changing domain you can set from which shop the data should be loaded and which currency and language will be displayed.
The different shops are listed in the following table.

| widget src | shop | currency | language 
| --- | --- | --- | ---
| widget.meine-landausfluege.de | meine-landausfluege.de | EUR | GER
| widget.meine-landausfluege.ch | meine-landausfluege.ch | CHF | GER

### iFrame
You only need to past this code in your page and change initial parameter. See parameter description below.

```html
<div id="tripup-widget-1" data-customer="TA123" data-port_id="90"
     data-components="search, itinerary, products"></div>
<script async defer type="text/javascript" 
        data-containers="tripup-widget-1, tripup-widget-2" 
        src="https://widget.my-cruise-excursion.com/itinerary/js/main.min.js"></script>
```

#### iFrame parameters

In data attribute "containers" of the script tag you can set one or more container id separated by comma. You can use more than one widget on your page. By default is the id of widget container: "tripup-widget"

In data attributes of your container you can store the start parameter. In the following table you can see the posible initial parameter.

| Name | Required | Description | Example value
| --- | --- | --- | ---
| customer | required | Your Partner-ID | TA123
| coupon | optional | You can set a coupon code to make some sale | TA123-10p-ASDF
| channel | optional | You can use this field to send some additional information, that will be shown in your reports | My-Custom-Channel
| components | optional | The widget contains 3 components. By default all 3 components will be loaded. You can define comma separated list of components you want to be shown  | search, itinerary, products
| ***The following parameter combination allow you to show whole itinerary with products to each port. You need to set at least cruise data***
| ship | required | Cruise ship name | *Azamara Pursuit*
| date | required | Cruise date | *2019-04-06* Format: YYYY-MM-DD
| duration | required | Cruise duration in days | 7
| ***If you only want to show products by port without itinerary, you have to use the following combination***
| port_id | required* | Either Port ID from our Database | *90*
| port_name | required* | Or Port name should be set | *Mallorca*
| port_date | optional | Selected date. By default is today  | *2019-04-12* Format: YYYY-MM-DD
| shortproducts | optional | Show only 3 first products and display more on button click  | false
| showtitles | optional | Show titles above blocks  | false
| showport | optional | Show port name and description | false

