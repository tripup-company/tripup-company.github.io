# TripUp Itinerary Widget

This widget is a frontend client that uses Cruise and Booking APIs. 

## Getting Started

There 2 option how you can use widget: using it as iframe or bind it directly as script into your page. 
The simplst and most secure way to use this widget is to paste the following code into your html. It works well with every site, even with CMS systems. If you use widget in a iframe you can not modify anything inside iframe. The last option is more advanced and needs more technical skills, but it's allow you to change style inside the widget.

#### Domain

We have several domains for different markts. By changing domain you can set from which shop the data should be loaded and which currency and language will be displayed.
The have different shop listed in the following table.

| widget src | shop | currency | language 
| --- | --- | --- | ---
| widget.my-cruise-excursion.com | my-cruise-excursion.com | USD | ENG
| widget.my-cruise-excursion.co.uk | my-cruise-excursion.co.uk | GBP | ENG
| widget.meine-landausfluege.de | meine-landausfluege.de | EUR | GER
| widget.meine-landausfluege.ch | meine-landausfluege.ch | CHF | GER

### iFrame
You only need to past this code in your page and change initial parameter.

```html
<div id="tripup-widget-1" data-customer="TA123" data-port-id="90"
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
| ***The following parametes define the inital state for widget like the cruise, selected date, product etc.***
| components | optional | The widget contains 3 components. By default all 3 components will be loaded. You can define comma separed list of components you want to be shown  | search, itinerary, products
| ship | optional | Cruise ship name | *Azamara Pursuit*
| date | optional | Cruise date | *2019-04-06* Format: YYYY-MM-DD
| port_id | optional | Itinerary item id | *1*
| port_date | optional | Itinerary port date | *2019-04-12* Format: YYYY-MM-DD
| product | optional | Selected product | *PMI-001-01-EN*

## Native Integration into HTML page (without iFrame)

The iFrame technik doesn't allow you to change the style of widget. But you can do it, if you integrate the widget script native in your site. This is less secure, because of script conflicts and you should test it before publish it live. 
You should also choose this integration type if you want to use components separated from each other or define different containers for them.

Requirements:
- The native integration requires jQuery Script: https://jquery.com/
- You need an API Token so the widget is able to access our API. Please ask our it support for access.

Installation:
If you need the loader icon component than you have to add the following stylesheet and JavaScript to the <head> part in HTML document:

```html
<link rel="stylesheet" type="text/css" href="https://widget.meine-landausfluege.de/itinerary/css/tripup-loader.min.css">
<script src="https://widget.meine-landausfluege.de/itinerary/js/loader.min.js" id="tripup-loader">
```

Next the widget core js script must be added:

```html
<script src="https://widget.meine-landausfluege.de/itinerary/js/loader.min.js" id="tripup-loader"></script>
<script src="https://widget.meine-landausfluege.de/itinerary/js/main.min.js" id="tripup-main"></script>
```

The initial part of widget script should be added to the head part of document or before the end of body tag:
 
```html
 <script type="text/javascript">
  TripUpMain.Init({
      KEY: '<API Token>',
      CUSTOMER_ID: "<Your-Partner-Id>",
      toggleComponents: true,
      // Contain all comonents and its default init parameters
      components: {
          search: {
              //selector for search form conteiner
              container: '#tripup-search'
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
              container: '#itinerary-holder'
          },
          products: {
               // Default init parameters
              params: {id: 64, date: '2019-02-13'},
                // Allow rewrite default params from url
              passUrlParams: true,
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
 </script>
```

The widget container inside the body tag of document must be added:

```html
<!--Start TripUp Widget Container-->
    <div class="tripup-block">
      <div id="tripup-search"></div>
      <div class="tripup-loader"></div>
      <div class="tripup-container">
        <div class="l-row">
          <div class="l-col-4" id="itinerary-holder"></div>
          <div class="l-col-8 l-push-4" id="products-holder"></div>
        </div>
      </div>
    </div>
<!-- End TripUp Widget Container-->
```

## Component and events

Component are independent from each other. They communicate over event with each other.
Below you will finde the event, those will be sent and recieved by component.

### Search form component

This component allow end user to select cruise or port .

#### Dispatched events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *CRUISE_SELECT_EVENT_NAME* | *'TripUpUpdateByCruise'* | `{ date: "2019-02-18", duration: 14, id: 10762, ship: "AIDAbella"}`| **id** => internal cruiseID |
| *PORT_SELECT_EVENT_NAME* | *'TripUpUpdateByPort'* |```{id: 27,  subid: 42787 }``` | |

### Itinerary component

This component shows itinerary of selected cruise.

#### Listened events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *CRUISE_SELECT_EVENT_LISTENER* | *'TripUpUpdateByCruise'* | ```{ date: "2019-02-18", duration: 14, id: 10762, ship: "AIDAbella"}``` | **id** => internal cruiseID  |
| *SELECT_PORT_EVENT_LISTENER* | *'TripUpSelectItineraryPort'* | ```{port: <port_id or "first" or "last">, [date: <cruise date>]}``` | if **port** = first or last than date will be ignored |

#### Dispatched events
     
| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *ITINERARY_SELECT_EVENT_NAME* | *'TripUpSelectedItinerary'* | `{ship: "Azamara Pursuit", current: {…}, prev: null, next: {…}}` |  Current, prev, next are the itinerary objects |   
| *LOADED_EVENT_NAME* | *'TripUpItineraryLoaded'* | `{ cruise: { date: "2019-04-06", duration: 7, port: "Lissabon", ship: "Azamara Pursuit""}, items: [ Iteinerary objects ]}}` |  |

### Products component

This component shows products for selected cruise or port

#### Listened events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *SCROLL_EVENT_LISTENER* | *'TripUpScrollProducts'* | ```{product: <firs or last or sku>}``` |  |
| *PORT_SELECT_EVENT_LISTENER* | *'TripUpUpdateByPort'* |```{id: 27,  subid: 42787 }``` | |
| *ITINERARY_SELECT_EVENT_LISTENER* | *'TripUpSelectedItinerary'* | `{ship: "Azamara Pursuit", current: {…}, prev: null, next: {…}}` |  Current, prev, next are the itinerary objects |

#### Dispatched events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *LOADED_EVENT_NAME* | *'TripUpProductsLoaded'* | `{[<product object>]}` |  |
| *PRODUCT_SELECT_EVENT_NAME* | *'TripUpProductSelected'* | `{<product object>}` | |
| *SELECT_PORT_EVENT_NAME* | *'TripUpSelectItineraryPort'* | `{port: <port_id or "first" or"last">, [date: <cruise date>]}` | if **port** = first or last than date will be ignored  |
