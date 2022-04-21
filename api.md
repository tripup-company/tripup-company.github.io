# TripUp REST API

All our API's are described with [Swagger](https://swagger.io/) (OpenAPI). By using [Swagger CodeGen](https://swagger.io/tools/swagger-codegen/)  you can generate libraries for different programming langauges.
Our Swagger UI allow you to test API directly in Browser. 
You need API credentials to use our API. Please contact our IT for credentials.

### Booking API

Booking API allow you to find products by cruise or port data. 
Create bookings over the API is not possible(These endpoints are not active right now).

https://api.meine-landausfluege.de/booking/

### Cruise API

Cruise API provide you cruise data like companies, ships, itineraries, ports etc.

https://api.meine-landausfluege.de/cruise/


#### Domains

We have several domains for different markets. By changing domain you can set from which shop the data should be loaded and which currency and language will be displayed.
The are different shops listed in the following table.

| widget src | shop | currency | language 
| --- | --- | --- | ---
| api.meine-landausfluege.de | meine-landausfluege.de | EUR | GER
| api.meine-landausfluege.ch | meine-landausfluege.ch | CHF | GER
