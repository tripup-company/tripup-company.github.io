# TripUp Reiseplan Widget

Dieses Widget ist ein Frontend-Client, der TripUp Cruise- und Booking-APIs verwendet 
und den Reiseplan mit allen Produkten darstellen kann.
[**Ein Demo können Sie sich hier ansehen** ](https://widget.meine-landausfluege.de/itinerary/iframe.html)

## Beginnen

Es gibt zwei Möglichkeiten, wie Sie dieses Widget verwenden können: 
1. Verwenden Sie es als Iframe 
2. oder binden Sie es direkt als Skript in Ihre Seite ein.

Die einfachste und sicherste Möglichkeit, dieses Widget zu verwenden, besteht darin, den iframe-Code in Ihr HTML einzufügen (siehe iframe-Beschreibung unten). 
Es funktioniert gut mit jeder Webseite, auch mit CMS-Systemen. Wenn Sie Widget in einem Iframe verwenden, können Sie nichts im Iframe ändern. 
Die zweite Option ist fortgeschrittener und erfordert mehr technische Fähigkeiten, aber Sie können den Stil innerhalb des Widgets ändern.

#### Domänen

Wir haben mehrere Domains für verschiedene Märkte. Durch einen Domainwechsel können Sie festlegen, aus welchem Shop die Daten geladen werden sollen und welche Währung und Sprache angezeigt wird.
Die verschiedenen Märkte sind in der folgenden Tabelle aufgeführt.

| Widget Src | Shop | Währung | Sprache 
| --- | --- | --- | ---
| widget.meine-landausfluege.de | meine-landausfluege.de | EUR | Deutsch
| widget.meine-landausfluege.ch | meine-landausfluege.ch | CHF | Deutsch
| widget.mycruiseexcursion.com | mycruiseexcursion.com | USD | Englisch
| widget.mycruiseexcursion.co.uk | mycruiseexcursion.co.uk | GBP | Englisch

### iFrame
Sie müssen nur diesen Code in Ihre Seite einfügen und ggf. die Initialisierungsparameter anpassen. 
Siehe nachfolgende Parameterbeschreibung.

```html
<div id="tripup-widget-1" data-customer="TA123" data-port_id="90"
     data-components="search, itinerary, products"></div>
<script async defer type="text/javascript" 
        data-containers="tripup-widget-1, tripup-widget-2" 
        src="https://widget.my-cruise-excursion.com/itinerary/js/main.min.js"></script>
```

#### iFrame Parameter

Im Datenattribut "containers" des Script-Tags können Sie eine oder mehrere durch Komma getrennte Container-IDs festlegen. 
Sie können mehr als ein Widget auf Ihrer Seite verwenden. Standardmäßig ist die ID des Widget-Containers: "tripup-widget"

In den Datenattributen Ihres Containers können Sie den Startparameter hinterlegen. In der folgenden Tabelle sehen Sie den möglichen Anfangsparameter.

| Name | Erforderlich | Beschreibung | Beispiel 
| --- | --- | --- | ---
| customer | erforderlich | Ihre Partner-ID | TA123
| coupon | optional | Sie können einen Gutscheincode festlegen | TA123-10p-ASDF
| channel | optional | In diesem Feld können Sie zusätzliche Informationen senden, die in Ihren Berichten angezeigt werden | My-Custom-Channel
| components | optional | Das Widget enthält 3 Komponenten. Standardmäßig werden alle 3 Komponenten geladen. Sie können eine durch Kommas getrennte Liste der Komponenten definieren, die angezeigt werden sollen  | search, itinerary, products
| ***Mit der folgenden Parameterkombination können Sie die gesamte Route mit Produkten zu jedem Hafen anzeigen. Sie müssen mindestens die Reisedaten einstellen***
| ship | erforderlich | Schiffsname | *Azamara Pursuit*
| date | optional | Reisedatum | *2019-04-06* Format: YYYY-MM-DD
| duration | optional | Reisedauer in Tagen | 7
| ***Wenn Sie nur Produkte nach Hafen ohne Reiseroute anzeigen möchten, müssen Sie die folgende Kombination verwenden***
| port_id | required* | Entweder Port ID aus unserer Datenbank | *90*
| port_name | required* | Oder Portname sollte gesetzt sein | *Mallorca*
| port_date | optional | Gewähltes Datum. Standardmäßig ist heute  | *2019-04-12* Format: YYYY-MM-DD
| shortproducts | optional | Zeige nur 3 erste Produkte und mehr auf Knopfdruck an  | false
| showtitles | optional | Überschrifte über Komponenten-Blöcken anzeigen  | false
| showport | optional | Portname und Beschreibung anzeigen | false

## Native Integration in HTML-Seite (ohne iFrame)

Mit der iFrame-Technik können Sie den Stil des Widgets nicht ändern. 
Sie können dies jedoch tun, wenn Sie das Widget-Skript nativ in Ihre Site integrieren. 
Dies ist aufgrund möglicher Skriptkonflikte weniger sicher, und Sie sollten es testen, bevor Sie es live veröffentlichen.
Sie sollten diesen Integrationstyp auch wählen, wenn Sie voneinander getrennte Komponenten verwenden oder unterschiedliche Container für diese definieren möchten.

Voraussetzungen:
- Die native Integration erfordert jQuery Script: https://jquery.com/
- Sie benötigen ein API-Token, damit das Widget auf unsere API zugreifen kann. Bitte fragen Sie unseren IT-Support nach dem Zugang.

Installation:

1. Das TripUp-Widget enthält eine eigene Loader-Komponente, die beim Laden eingeblendet wird.
  <br>Zunächst sollte der folgende Stylesheet-Link zum head des HTML-Dokuments hinzugefügt werden:
   ```html
   <link rel="stylesheet" type="text/css" href="<some domain and subpath>/css/tripup.min.css">
   ```
   Wenn Sie die Loader-Symbolkomponente benötigen, müssen Sie das folgende Stylesheet und JavaScript zum Teil head im HTML-Dokument hinzufügen:
   ```html
   <link rel="stylesheet" type="text/css" href="https://widget.meine-landausfluege.de/itinerary/css/tripup-loader.min.css">
   <script src="https://widget.meine-landausfluege.de/itinerary/js/loader.min.js" id="tripup-loader">
   ```

2. Als nächstes muss das js-Skript des Widget-Kerns hinzugefügt werden:
   ```html
   <script src="https://widget.meine-landausfluege.de/itinerary/js/main.min.js" id="tripup-main"></script>
   ```

3. Der erste Teil des Widget-Skripts sollte zum head des Dokuments oder vor dem Ende des Body-Tags hinzugefügt werden.
  <br>Sie können nur eine bestimmte Komponente anzeigen. Konfigurieren Sie einfach keine Komponenten, die Sie nicht benötigen.

   ```html
   <script type="text/javascript">
    TripUpMain.Init({
      KEY: '<API Token>',
      CUSTOMER_ID: "<Your-Partner-Id>",
      // Contain all comonents and its default init parameters
      components: {
          search: {
              //selector for search form conteiner
              container: '#tripup-search'
          },
          itinerary: {              
              params: { // Default init parameters
                  date: "2019-04-06",
                  duration: 7,
                  ship: "Azamara Pursuit"
              },
              showTitle: false, // Show Title above container. Default false                                          
              container: '#itinerary-holder' // Selector for itinerary list
          },
          products: {
              // Those parameters can be added if you only want to show products by port without itinerary.
              // params: { 
              //    date: "2019-04-06",
              //    id: 90,
              // },
              showTitles: false, // Show titles above containers. Default false
              shortProducts: false, // Show only first 3 products and display more on button click. Default false.
              container: '#products-holder', // Selector for products list
          }
      },
    }, true, '<some domain>');
    </script>
   ```

4. Die Widget-Container müssen innerhalb des Body-Tags des Dokuments hinzugefügt werden. Jeder Komponentencontainer sollte die ID haben, die oben im Init-Skript für diese Komponente definiert wurde. In diesem Beispiel haben wir die IDs "tripup-search", "route-holder", "products-holder" definiert, die Sie auch unten sehen können.
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

## Komponenten und Ereignisse

Komponenten sind voneinander unabhängig. Sie kommunizieren durch Ereignisse miteinander.
Nachfolgend finden Sie die Ereignisse, die von Komponenten gesendet und empfangen werden.

### Suchformular-Komponente

Mit dieser Komponente kann der Endbenutzer eine Kreuzfahrt oder einen Hafen auswählen.

#### Ausgelöste Ereignisse

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *CRUISE_SELECT_EVENT_NAME* | *'TripUpUpdateByCruise'* | `{ date: "2019-02-18", duration: 14, id: 10762, ship: "AIDAbella"}`| **id** => internal cruiseID |
| *PORT_SELECT_EVENT_NAME* | *'TripUpUpdateByPort'* |```{id: 27,  subid: 42787 }``` | |

### Reiseplan-Komponente

Diese Komponente zeigt die Reiseroute der ausgewählten Kreuzfahrt an.

#### Zuhörende Ereignisse

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *CRUISE_SELECT_EVENT_LISTENER* | *'TripUpUpdateByCruise'* | ```{ date: "2019-02-18", duration: 14, id: 10762, ship: "AIDAbella"}``` | **id** => internal cruiseID  |
| *SELECT_PORT_EVENT_LISTENER* | *'TripUpSelectItineraryPort'* | ```{port: <port_id or "first" or "last">, [date: <cruise date>]}``` | if **port** = first or last than date will be ignored |

#### Gesendete Ereignisse
     
| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *ITINERARY_SELECT_EVENT_NAME* | *'TripUpSelectedItinerary'* | `{ship: "Azamara Pursuit", current: {…}, prev: null, next: {…}}` |  Current, prev, next are the itinerary objects |   
| *LOADED_EVENT_NAME* | *'TripUpItineraryLoaded'* | `{ cruise: { date: "2019-04-06", duration: 7, port: "Lissabon", ship: "Azamara Pursuit""}, items: [ Iteinerary objects ]}}` |  |

### Produkt-komponente

This component shows products for selected cruise or port

#### Zuhörende Ereignisse

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *SCROLL_EVENT_LISTENER* | *'TripUpScrollProducts'* | ```{product: <firs or last or sku>}``` |  |
| *PORT_SELECT_EVENT_LISTENER* | *'TripUpUpdateByPort'* |```{id: 27,  subid: 42787 }``` | |
| *ITINERARY_SELECT_EVENT_LISTENER* | *'TripUpSelectedItinerary'* | `{ship: "Azamara Pursuit", current: {…}, prev: null, next: {…}}` |  Current, prev, next are the itinerary objects |

#### Gesendete Ereignisse

| Constant name | Default value | Payload example | Comments |
| --- | --- | --- | --- |
| *LOADED_EVENT_NAME* | *'TripUpProductsLoaded'* | `{[<product object>]}` |  |
| *PRODUCT_SELECT_EVENT_NAME* | *'TripUpProductSelected'* | `{<product object>}` | |
| *SELECT_PORT_EVENT_NAME* | *'TripUpSelectItineraryPort'* | `{port: <port_id or "first" or"last">, [date: <cruise date>]}` | if **port** = first or last than date will be ignored  |

## Fehlerbehebung

### IE 10 and 11 compatibility
Für die Kompatibilität mit dem InternetExplorer sollte zunächst das folgende Skript hinzugefügt werden

```javascript
<script type="text/javascript">
      if (/MSIE \d.|Trident.*rv:\d./.test(navigator.userAgent)) {
          var el = document.createElement('script');
          el.src = "https://cdnjs.cloudflare.com/ajax/libs/babel-polyfill/7.2.5/polyfill.min.js";
          el.addEventListener('load',function(){el.id="tripup-polyfill";});
          document.head.appendChild(el);
      }
</script>
```
