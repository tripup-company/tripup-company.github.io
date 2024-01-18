# TripUp JavaScrip Search widget

This widget is a client that uses Cruise and Booking APIs. It have several components.

## How to use

### IE 10 and 11 compatibility

- For InternetExplorer compatibility the following script should be added firstly
```js
<script type="text/javascript">
      if (/MSIE \d.|Trident.*rv:\d./.test(navigator.userAgent)) {
          var el = document.createElement('script');
          el.src = "https://cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.2.5/polyfill.min.js";
          el.addEventListener('load',function(){el.id="tripup-polyfill";});
          document.head.appendChild(el);
      }
</script>
```
#### Domain

We have several domains for different markets. By changing domain you can set from which shop the data should be loaded and which currency and language will be displayed.
The different shops are listed in the following table.

| widget src | shop | currency | language 
| --- | --- | --- | ---
| widget.meine-landausfluege.de | meine-landausfluege.de | EUR | GER
| widget.meine-landausfluege.ch | meine-landausfluege.ch | CHF | GER

### Embedding to html page

- At first to the head part of  HTML document the following stylesheet link should be added:
 ```html
<link rel="stylesheet" type="text/css" href="https://widget.meine-landausfluege.de/itinerary/css/tripup.min.css">
```
- If the loader component needed, to the head part HTML document the following JavaScript should be added:
```html
<script src="https://widget.meine-landausfluege.de/itinerary/js/loader.min.js" id="tripup-loader">
```
- Next the core js script must be added:
```html
<script src="https://widget.meine-landausfluege.de/itinerary/js/main.min.js" id="tripup-main"></script>
```
- After that to the head part of document or before the end of body tag the initialization script must be pasted:
```js
            TripUpMain.Init({
                KEY: '<customer key>',
                CUSTOMER_ID: "<customer id>",
                COUPON: "<coupon>",
                CHANNEL: "<channel>",
                toggleComponents: true,
                // Contain all comonents and its default init parameters
                components: {
                    search: {
                        //selector for search form conteiner
                        container: '#tripup-search',
                        cruiseType: 'sea',
                    },
                    itinerary: {
                        // Default init parameters
                        params: {
                            date: "2019-04-06",
                            duration: 7,
                            ship: "Azamara Pursuit"
                        },
                        // Allow rewrite default params from url
                        passUrlParams: true,      
                        // Selector for itinerary list                                          
                        container: '#itinerary-holder',
                        // Package click handler
                        packageClick: function (data) {
                            console.log("packageClick", data);
                            return false;
                        }
                    },
                    products: {
                         // Default init parameters
                        params: {id: 64, date: '2019-02-13'},
                          // Allow rewrite default params from url
                        passUrlParams: true,
                        // Show only first three excursions
                        shortProducts: false,
                        // Allow show offline products 
                        offline: false,
                         // Selector for products list   
                        container: '#products-holder',
                        // Allowed callbacks
                        moreClick: function (data) {
                             alert("moreClick");
                             return false;
                        },
                        chooseClick: function (data) {
                            alert("chooseClick");
                            return false;
                        }
                    }
                },
                // Define the components initial state                
                state: {
                     itinerary: {
                         port: 1,
                         date: '2019-04-12'
                     },
                     products: {
                         product: "PMI-011-00-DE"
                     }
                }
            }, true, '<some domain>');
```
- The widget container inside the body tag of document must be added:
```html
<!--Start TripUp Widget Container-->
    <div class="tripup-block">
      <div id="tripup-search"></div>
      <div class="tripup-loader"></div>
      <div class="tripup-container">
          <div class="l-row widget-packages-holder">
              <div id="widget-packages" class="l-col-xs-12 l-col-sm-12 l-col-md-12 l-col-lg-12 trp-hidden"></div>
          </div>
          <div class="l-row widget-nav-holder">
              <div id="widget-navigation" class="l-col-xs-12 l-col-sm-12 l-col-md-12 l-col-lg-12"></div>
          </div>
          <div class="l-row widget-holder">
              <div class="l-col-xs-12 l-col-sm-6 l-col-md-4 l-col-lg-4" id="itinerary-holder"></div>
              <div class="l-col-xs-12 l-col-sm-6 l-col-md-8 l-col-lg-8" id="products-holder"></div>
          </div>
      </div>
    </div>
<!-- End TripUp Widget Container-->
```
### Iframe

- You only need to past this code in your page and change initial parameter.
```html
<div id="tripup-widget-1" data-customer="TA123" data-port-id="90" data-components="search, itinerary, products"></div>
<script async defer type="text/javascript" data-containers="tripup-widget-1, tripup-widget-2" 
        src="https://widget.meine-landausfluege.de/itinerary/js/main.min.js"></script>
```
#### Iframe parameters

In data attribute “containers” of the script tag you can set one or more container id separated by comma. You can use more
than one widget on your page. By default is the id of widget container: “tripup-widget”

In data attributes of your container you can store the start parameter. In the following table you can see the posible initial parameter.

| Name | Description | Example value
| --- | --- | --- |
| ***customer*** | Custopmer ID | TA-80
| ***coupon*** | Custopmer coupon | coupon number
| ***channel*** | Custopmer channel | Some additional information
| ***ship*** | Cruise ship name | *Azamara Pursuit*
| ***date*** | Cruise date | *2019-04-06*
| ***port_id*** | Port ID | *1*
| ***port_name*** | Port name | *1*
| ***port_date*** | Itinerary port date | *2019-04-12*
| ***skus*** | Products's SKU filter | *LIS-001-00-DE,LIS-002-00-DE*
| ***sku*** | Selected product sku | *LIS-001-00-DE*
| ***offline*** | Show offline products | *false*
| ***tags*** | Tags filter for product list (use comma separated tag slags) | *top,new*
| ***limit*** | Show only limited products count | *10*
| ***destination*** | Destination filter for product list| *Ostsee*
| ***shortproducts*** | Show only three first products | *false*
| ***cruisetype*** | It sets cruises and ports type for the search component | *sea* or *river*|

## Search form component

This component helps end user select cruise or port .

### Dispatched events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| **CRUISE_SELECT_EVENT_NAME** | *'TripUpUpdateByCruise'* | `{ date: "2019-02-18", duration: 14, id: 10762, ship: "AIDAbella"}`| **id** - cruiseID |
| **PORT_SELECT_EVENT_NAME** | *'TripUpUpdateByPort'* |```{id: 27,  subid: 42787 }``` | |

## Itinerary component

This component shows itinerary of selected cruise.

### Listened events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| **CRUISE_SELECT_EVENT_LISTENER** | *'TripUpUpdateByCruise'* | ```{ date: "2019-02-18", duration: 14, id: 10762, ship: "AIDAbella"}``` | **id** - cruise id  |
| **SELECT_PORT_EVENT_LISTENER** | *'TripUpSelectItineraryPort'* | ```{port: <port_id or "first" or"last">, [date: <cruise date>]}``` | if **port** = first or last the date value will be ignored |

### Dispatched events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| **ITINERARY_SELECT_EVENT_NAME** | *'TripUpSelectedItinerary'* | `{ship: "Azamara Pursuit", current: {…}, prev: null, next: {…}}` |  Current, prev, next are the itinerary objects |   
| **LOADED_EVENT_NAME** | *'TripUpItineraryLoaded'* | `{ cruise:{date: "2019-04-06", duration: 7, port: "Lissabon", ship: "Azamara Pursuit""}, items: [ Iteinerary objects ]}}` |  |
| **PRODUCT_LOADING_ERROR** | *PRODUCT_LOADING_ERROR* | `{message: <errortext>}`                                       | This event is dispatched when any kind of error occurs.    |

## Products component

This component shows products for selected cruise or port

### Listened events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| **SCROLL_EVENT_LISTENER** | *'TripUpScrollProducts'* | `{product: <firs or last or sku>}` |  |
| **PORT_SELECT_EVENT_LISTENER** | *'TripUpUpdateByPort'* |```{id: 27,  subid: 42787 }``` | |
| **ITINERARY_SELECT_EVENT_LISTENER** | *'TripUpSelectedItinerary'* | `{ship: "Azamara Pursuit", current: {…}, prev: null, next: {…}}` |  Current, prev, next are the itinerary objects |

### Dispatched events

| Constant name | Default value | Payload example                                                | Comments                                                   |
| --- | --- |----------------------------------------------------------------|------------------------------------------------------------|
| **LOADED_EVENT_NAME** | *'TripUpProductsLoaded'* | `{[<product object>]}`                                         |                                                            |
| **PRODUCT_SELECT_EVENT_NAME** | *'TripUpProductSelected'* | `{<product object>}`                                           |                                                            |
| **SELECT_PORT_EVENT_NAME** | *'TripUpSelectItineraryPort'* | `{port: <port_id or "first" or"last">, [date: <cruise date>]}` | if **port** = first or last the date value will be ignored |
| **DETAILS_LOADED_EVENT_NAME** | *'TripUpDetailsLoaded'* | `{sku: <product ID>}`                                          |                                                            |
| **PRODUCT_LOADING_ERROR** | *PRODUCT_LOADING_ERROR* | `{message: <errortext>}`                                       | This event is dispatched when any kind of error occurs.    |
