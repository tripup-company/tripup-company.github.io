# TripUp REST API

*WORK IN PROGRESS*

All over API's are described by [Swagger](https://swagger.io/) (OpenAPI). By using swagger generator you can gernerate libraries for different programming langauges.
Our UI allow you to test API directly in Browser. 
You need API Creadetials to use our API. Please contact our IT for creadentials.

### Booking API

Booking API allow you to find products by cruise or port data. 
It's also possible to create bookings over the API (beta).

https://api.meine-landausfluege.de/booking/

### Cruise API

Cruise API provide you cruise data like companies, ships, itineraries etc.

https://api.meine-landausfluege.de/cruise/


#### Domains

We have several domains for different markets. By changing domain you can set from which shop the data should be loaded and which currency and language will be displayed.
The have different shop listed in the following table.

| widget src | shop | currency | language 
| --- | --- | --- | ---
| api.my-cruise-excursion.com | my-cruise-excursion.com | USD | ENG
| api.my-cruise-excursion.co.uk | my-cruise-excursion.co.uk | GBP | ENG
| api.meine-landausfluege.de | meine-landausfluege.de | EUR | GER
| api.meine-landausfluege.ch | meine-landausfluege.ch | CHF | GER
