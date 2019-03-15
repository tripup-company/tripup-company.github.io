# TripUp Itinerary Widget

This widget is a frontend client that uses Cruise and Booking APIs. 

## Getting Started

There 2 option how you can use widget: using it as iframe or bind it directly as script into your page. 
The simplst and most secure way to use this widget is to paste the following code into your html. It works well with every site, even with CMS systems. If you use widget in a iframe you can not modify anything inside iframe. The last option is more advanced and needs more technical skills, but it's allow you to change style inside the widget.

### Iframe
You only need to past this code in your page and change initial parameter.

```html
 <script type="text/javascript">
   var tripUpWidgetId = "tripUpWidget",
       tripUpWidgetParameter = "[b]customer_id=TEST-123&ship=AIDAprima&date=2019-03-05&duration=7&port_id=26&port_date=2019-03-10&sku=MCT-001-00-DE[/b]",
       tripUpWidgetSrc = "https://widget.meine-landausfluege.de/itinerary/iframe-src.html?"+tripUpWidgetParameter;
   function tuWidgetReceiveMessage(e) {
       var t = e.data.split(':'), i = t[0];
       -1 !== [tripUpWidgetSrc].indexOf(e.origin) && 'widgetResize' === i && (iframe = document.getElementById(tripUpWidgetId)) && (iframe.contentWindow || iframe.documentWindow) == e.source && (iframe.style.height = t[1] + 'px')
   }
   window.addEventListener ? window.addEventListener('message', tuWidgetReceiveMessage, !1) : window.attachEvent && window.attachEvent('onmessage', tuWidgetReceiveMessage), document.write("<iframe id='" + tripUpWidgetId + "' src='" + tripUpWidgetSrc + "' style='width: 100%; height: 100%; border: 0;' frameborder='0'></iframe>");
 </script>
```

In tripUpWidgetParameter you can store the start parameter like the cruise, selected date, product etc. In the following table you can see the posible search parameter.
You can set parameter by in this format parameter=value. Multiple parameters should be bind by ampersand.
#### Iframe parameters

The options in the following table as GET-parameters can be passed. This options define components initial state.

| Name | Required | Description | Example value
| --- | --- | --- | ---
| ***customer_id*** | required | Your Partner-ID | TA-123
| ***ship*** | optional | Cruise ship name | *Azamara Pursuit*
| ***date*** | optional | Cruise date | *2019-04-06* Format: YYYY-MM-DD
| ***port_id*** | optional | Itinerary item id | *1*
| ***port_date*** | optional | Itinerary port date | *2019-04-12*
| ***sku*** | optional | Selected product sku | *10*

### Native Integration into HTML page (without iFrame)

The iFrame technik doesn't allow you to change the style of widget. But you can do it, if you integrate the widget script native in your site. This is less secure, because of script conflicts and you should test it before publish it live. 

Requirements:
- The native integration requires jQuery Script: https://jquery.com/
- You need an API Token so the widget is able to access our API. Please ask our it support for access.

Installation:
- If you need the loader icon component than you have to add the following stylesheet and JavaScript to the <head> part in HTML document:
```html
<link rel="stylesheet" type="text/css" href="https://widget.meine-landausfluege.de/itinerary/css/tripup-loader.min.css">
<script src="https://widget.meine-landausfluege.de/itinerary/js/loader.min.js" id="tripup-loader">
```
- Next the widget core js script must be added:
```html
<script src="https://widget.meine-landausfluege.de/itinerary/js/loader.min.js" id="tripup-loader"></script>
<script src="https://widget.meine-landausfluege.de/itinerary/js/main.min.js" id="tripup-main"></script>
```
- The initial part of widget script should be added to the head part of document or before the end of body tag:
```javascript
  TripUpMain.Init({
      KEY: '<API Token>',
      CUSTOMER_ID: "<customer id>",
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
```
- The widget container inside the body tag of document must be added:
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

## Search form component

This component allow end user to select cruise or port .

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

## Products component

This component shows products for selected cruise or port

### Listened events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| **SCROLL_EVENT_LISTENER** | *'TripUpScrollProducts'* | `{product: <firs or last or sku>}` |  |
| **PORT_SELECT_EVENT_LISTENER** | *'TripUpUpdateByPort'* |```{id: 27,  subid: 42787 }``` | |
| **ITINERARY_SELECT_EVENT_LISTENER** | *'TripUpSelectedItinerary'* | `{ship: "Azamara Pursuit", current: {…}, prev: null, next: {…}}` |  Current, prev, next are the itinerary objects |

### Dispatched events

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| **LOADED_EVENT_NAME** | *'TripUpProductsLoaded'* | `{[<product object>]}` |  |
| **PRODUCT_SELECT_EVENT_NAME** | *'TripUpProductSelected'* | `{<product object>}` | |
| **SELECT_PORT_EVENT_NAME** | *'TripUpSelectItineraryPort'* | `{port: <port_id or "first" or"last">, [date: <cruise date>]}` | if **port** = first or last the date value will be ignored |
