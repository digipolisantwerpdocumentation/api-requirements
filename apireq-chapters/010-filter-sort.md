## Filteren en sorteren

Als algemene richtlijn geldt dat, indien filtering gewenst is, dit steeds dient te gebeuren door middel van query parameters.

### Filteren op resources
Elke resource in een collection is uniek identificeerbaar via een unieke sleutel (id).
De unieke sleutel kan leesbaar zijn zoals bv een rijksregisternummer, maar kan ook een id zijn (bv UUID).

#### R-FR-001
##### Filter op een resource via id
Filteren op id, dwz. selectie van een specifieke resource binnen de collection op basis van een unieke identifier, gebeurt door deze mee op te nemen als node in de URI en niet door middel van een query parameter.

Filteren op id's doe je als volgt:
``` prettyprint
GET /{resource}/{id}
GET /partners/2365
```
#### R-FR-002
##### Filter op een resource via andere unieke sleutel
Filteren op een andere unieke sleutel gebeurt op dezelfde manier als filteren op een id.

Filteren op een andere unieke sleutel doe je als volgt:
``` prettyprint
GET /{resource}/{id}
GET /partners/93051822361  # rijksregisternr
```

#### R-FR-003
##### Gebruik geen gecombineerde sleutels voor een unieke sleutel
Het is niet toegelaten om via een combinatie van sleutels een unieke sleutel voor een resource te definiëren.

``` prettyprint
GET /{resource}/{id}/{subid}
GET /partners/man/001
gebruik je dus NIET
```

#### R-FR-004
##### Filter op een resource via query parameters
Filteren op individuele velden is mogelijk door voor elk veld in de resource representatie, dat filtering implementeert, een unieke query parameter te definiëren.

**Opgelet, dit doen we niet voor id's.**  

Belangrijk hierbij is dat de structuur van de query parameter zo vlak mogelijk wordt gehouden zonder dat hierbij business betekenis verloren gaat. 

Indien je op andere velden dan de id's wil filteren, dan dienen de query parameters zo te zijn opgesteld dat deze ondubbelzinnig kunnen worden geinterpreteerd.

Zo kies je bijvoorbeeld best voor `city` ipv `address-city` als query parameter indien je wil filteren op onderstaand antwoord:
```json
{
  "company": "Google",
  "website": "http://www.google.com/",
  "address": {
    "line1": "111 8th Ave",
    "line2": "4th Floor",
    "state": "NY",
    "city": "New York",
    "zip": "10011"
  }
}
```

De URI om enkel die resources terug te krijgen waarvoor city gelijk is aan New York ziet er als volgt uit
``` prettyprint
https://api-gateway/digipolis/business-party/v1/business-parties?city=New%20York
```

Indien de query parameter waarmee je gaat filteren meerdere waarden kan bevatten, dan gebruik je volgende conventie (comma separated) : **resource?parameter=waarde1,waarde2,waarde3** 

### Filteren op resource representatie
#### R-FP-001
##### Filter op een resource representatie via de `fields` query parameter
Een API dient steeds een minimale hoeveelheid informatie terug te sturen in zijn response. Soms is het wenselijk dat een API consumer slechts een subset (1 of meerdere velden) van een resource representatie wenst op te vragen. Dit kan door middel van een reserved query parameter genaamd `fields`.

Deze query parameter definieert een door middel van komma's gescheiden lijst van velden dewelke de API consumer wenst te verkrijgen.

De naam van deze velden dient net zoals bij resource filtering zo te zijn gekozen dat deze een zo vlak mogelijke structuur aanhouden zonder dat hierbij business betekenis verloren gaat. Een veld dat men wenst te verkrijgen wordt geëncapsuleerd door het bovenliggende object en ().

vb. als de normale response het volgende is :
```json
{
  "company": "Google",
  "website": "http://www.google.com/",
  "address": {
    "line1": "111 8th Ave",
    "line2": "4th Floor",
    "state": "NY",
    "city": "New York",
    "zip": "10011"
  }
}
```

``` prettyprint
https://api-gateway/digipolis/business-party/v1/business-parties?fields=company,address(city,zip)
```

``` json
{
  "company": "Google",
  "address": {
    "city": "New York",
    "zip": "10011"
  }
}
```

``` prettyprint
https://api-gateway/digipolis/business-party/v1/business-parties?fields=company,address
```

```json
{
  "company": "Google",
  "address": {
    "line1": "111 8th Ave",
    "line2": "4th Floor",
    "state": "NY",
    "city": "New York",
    "zip": "10011"
  }
}
```

``` prettyprint
https://api-gateway/digipolis/business-party/v1/business-parties?fields=company,city
```

``` prettyprint
HTTP Response code 404 (city wordt niet gevonden)
```

### Sorteren
#### R-SR-001
##### Sorteer response door middel van de `sort` query parameter
Sorteren van antwoorden gebeurt steeds door middel van de reserved query parameter `sort`.

Standaard worden resultaten in oplopende volgorde gesorteerd. Indien aflopend gesorteerd dient te worden, dan dient "-" te worden toegevoegd vooraan de parameter dewelke aflopend gesorteerd dient te worden. Indien gesorteerd dient te worden op meerdere parameters, dan kan dit door deze met komma's van elkaar te scheiden.

Een voorbeeld van een URI met sortering:

``` prettyprint
https://api-gateway/digipolis/business-party/v1/business-parties?sort=-zip,company
```

Dit geeft als resultaat een lijst van business parties aflopend volgens zip code. Voor een specifieke zip code zullen business parties alfabetisch gerangschikt worden volgens company naam in oplopende volgorde.
