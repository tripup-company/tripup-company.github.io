# TripUp JavaScrip Search widget
{% include navigation.html %}

This widget is a client that uses Cruise and Booking APIs. It have two part: 
- native javascript widget to search products by search parameters;
- search form based on jQuery. 

## Development guide

### Local development
To use the project locally, first install dependencies via: 
```shell
npm install 
```
After this we can change styles and javascript sourses in **src** directory. Then we have to make dist version via:
```shell
gulp
```

## Widget
### Input parameters
First, we have to set global params:
```
TripUp.Settings.KEY = "your access key";
TripUp.Settings.CUSTOMER_ID = "your customer id";
```
Widget constructor get settings object that have next fields:

| Name|  Description | Example
|--|--|--|
| params | Search params | ```{ ship:'AIDAaura', date:'2018-09-08', duration: 9} or {id: 27} or {name: 'Mallorca'}```
| container | Id of div container (without #)  | widget_dwq2112
| domain |  Host name |https://dev.meine-landausfluege.de
| MAX_PRODUCT_SIZE | Products list size | 10

For example:
```
TripUp.Widget({
    params: { ship:'AIDAaura', date:'2018-09-08', duration: 9},
    container:'widget_dwq2112',
    domain: 'https://dev.meine-landausfluege.de',
    MAX_PRODUCT_SIZE:3
});
```
### Triggered events

Widget triggers **TripUpLoadDataFinish** event object when data load request is finished. This event is instance of javascript CustomEvent.

## Search form
### Input parameters
Search form is a jQuery plugin. To run it we have to set access key:
```
TripUp.Settings.KEY = "your access key";
```
Search form constructor get settings object that have next fields:

| Name | Descripton  | Default value | 
|--|--|--|
|apiURL|Api Url| https://api.meine-landausfluege.de/cruise/v1/|
|companySelector| Selector to find company select  field|[role="company-list"]|
|shipSelector| Selector to find ship select field|[role="ship-list"]|
|timeSelector|Selector to find company select field|[role="time-list"]|
|portSelector|Selector to find port select field|[role="port-list"]|
|cruiseFormSelector| Selector of search form by cruise params | [role="cruise-list-form"]|
|portFormSelector|Selector of search form by port params|[role="port-list-form"]|
|container| Selector to append form code if value not null| null|
|loaderSelector|Selector to find loader element|.search-forms-loader |
Simple example:
```
TripUp.Search({
   container: '#widget_dwq2112'
});
```

### Triggered events

Search  form triggers several  native js events (CustomEvent instances): 

|Event name|  Description | Event data |
|--|--|--|
|**TripUpUpdateByCruise** | Event triggers when cruise search form has been submitted |``` { ship:'Some ship', date:'20XX-XX-XX', duration: X}```|
|**TripUpUpdateByPort**|Event triggers when port search form has been submitted|```{id: val, subid: subid}```|

Also, search  form triggers several  jQuery custom events: 

|Event name|  Description | Event data |
|--|--|--|
|**change-port.tripup** | Event triggers when port field has been changed |0 or portId|
|**change-company.tripup**|Event triggers when company field has been changed|0 or companyId|
|**change-ship.tripup**|Event triggers when ship field has been changed|0 or shipId|
|**change-time.tripup**|Event triggers when time field has been changed|0 or timeId|

### Search form degault values

There is ability to set default values for parameters of the search form. Set value of initValues object  before search plugin is initialized. For example:
```
TripUp.Settings.Plugin = {
        ...
        initValues: {
            companyID: 0,
            shipID: 0,
            cruiseID: 0,
            portID: 0
        }
        ...
    };
```
### Product Widget

The constructor of this accepts the custom configuration object.
```js
TripUp.Products(
    {
       //Default search parameters
       params: { 
            //Port ID
            id: 27, 
            //Cruise date
            date: '2019-05-13' 
       },
       //ID of html element where the component will be shown
       container: 'products-holder',
       // Show port information block. False by default.
       showPort: false, 
       // The more button callback. Null by default.
       moreClick: function(data){
           // TODO moreClick code
       },
       // The more button callback. Null by default.
       chooseClick: function (data) {
          // TODO: chooseClick code
       }
    })
```
