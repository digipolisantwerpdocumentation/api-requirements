## Request

### Request body & query parameters

Resource representations dienen steeds in de request body te worden doorgestuurd en nooit via query parameters.

Query parameters worden steeds gebruikt om bepaalde functionaliteiten zoals paginatie, filtering, sortering, etc aan te spreken.  
**Query parameters worden bijgevolg nooit gebruikt om resource representaties weer te geven.**
``` prettyprint
POST /business-parties?company=Google&website=http://www.google.com/&addressLine1=111 8th Ave&addressLine2=4th Floor&state=NY&city=New York&zip=10011
gebruik je dus NIET
```

Bovenstaand voorbeeld is niet enkel onleesbaar, het laat ook niet toe om hiërarchieën op een eenvoudige manier op te nemen in de representatie.

### HTTP verbs

Gebruik steeds de juiste HTTP verbs voor de bijhorende request zoals weergegeven in onderstaande tabel. HTTP verbs moeten in lijn zijn
met [RFC7231](https://tools.ietf.org/html/rfc7231).

Voor **PATCH** zijn volgende RFC's van toepassing :

- eenvoudige wijzigingen : [RFC7386](https://tools.ietf.org/html/rfc7386)
- complexe wijzigingen : [RFC6902](https://tools.ietf.org/html/rfc6902)


HTTP Verb | Safe | Idempotent | Toepassing                                                                                                                                             | Request
--------- | ---- | ---------- | ----------                                                                                                                                             | ------- 
GET       | Ja   | Ja         | Voor het ophalen van de representatie van een resource. Opeenvolgende calls hebben geen invloed op de state van de resource.                           | Is steeds leeg
HEAD      | Ja   | Ja         | Voor het ophalen van de headers van een resource (om bv de aanwezigheid van een resource te controleren zonder de volledige payload te moeten ontvangen). Opeenvolgende calls hebben geen invloed op de state van de resource.                           | Is steeds leeg
PUT       | Neen | Ja         | Voor het vervangen van een bestaande resource. Indien de resource niet bestaat, wordt deze aangemaakt.                                                 | Representatie van de te vervangen resource
POST      | Neen | Neen       | Voor het aanmaken van een nieuwe resource binnen een collection.                                                                                       | Representatie van de aan te maken resource (optionele velden niet)
POST      | Neen | Neen       | Voor het uitvoeren van een activity of controller (=command) resource.                                                                                 | Representatie van info voor controller
PATCH     | Neen | Neen       | Voor het updaten van een set velden binnen een bestaande resource. Enkel de opgegeven velden worden gewijzigd. Alle andere velden blijven ongewijzigd. | Enkel die velden van de resource die men wenst te updaten
DELETE    | Neen | Ja         | Voor het verwijderen van een resource binnen een collection.                                                                                           | Is steeds leeg


**Safe** methodes zijn methodes dewelke de resource representatie niet wijzigen. Een safe methode mag nooit gebruikt worden om de state van een resource te wijzigen.

**Idempotent** methodes zijn methodes bij dewelke het resultaat niet wijzigt, ongeacht het aantal keer dat ze worden opgeroepen. Het belang van een idempotent methode bevindt zich in het bouwen van fout tolerante APIs.

Veronderstel dat een consumer een update wil doen van een resource door middel van PUT, maar dat deze consumer een timeout of zelfs geen antwoord terug krijgt. In dat geval kan een idempotent methode opnieuw worden uitgevoerd zonder dat de consumer zich moet afvragen of zijn vorige call wel het gewenste resultaat opleverde.

**Optionele velden in de resource representatie dienen op hun default waarde ingesteld te worden op de backend indien deze niet worden
opgegeven in de request.**

### Voorbeelden

#### GET

Voor elk van onderstaande voorbeelden is de request body steeds leeg

-   Haalt een lijst op van alle business-parties. Het resultaat is steeds gepagineerd.
``` prettyprint
GET https://api-gateway/digipolis/business-party/v1/business-parties
```

-   Haalt business-party op met id 6532
``` prettyprint
GET https://api-gateway/digipolis/business-party/v1/business-parties/6532
```

-   Haalt alle contracten die behoren bij business-party met id 6532. Door het feit dat contracts een subresource is van business-parties betekent dit dat beiden met elkaar gerelateerd zijn en dat het hier de contracts betreft van een welbepaalde business-party.
``` prettyprint
GET https://api-gateway/digipolis/business-party/v1/business-parties/6532/contracts
```

-   Haalt contract op met id 42 dat behoort bij business-party met id 6532. Door het feit dat contracts een subresource is van
    business-parties betekent dit dat beiden met elkaar gerelateerd zijn en dat het hier de contracts betreft van een welbepaalde
    business-party.
``` prettyprint
GET https://api-gateway/digipolis/business-party/v1/business-parties/6532/contracts/42
```

#### HEAD

Voor elk van onderstaande voorbeelden is de request body steeds leeg

-   Controleert het bestaan van business-party met id 6532 (zonder de volledige resource op te halen)
``` prettyprint
HEAD https://api-gateway/digipolis/business-party/v1/business-parties/6532
```

#### PUT

Voor elk van onderstaande voorbeelden bevat de request body steeds de volledige resource representatie

-   Updaten business-party met id 6532
``` prettyprint
PUT https://api-gateway/digipolis/business-party/v1/business-parties/6532
```

-   Updaten contract met id 42 dat behoort tot business-party met id 6532
``` prettyprint
PUT https://api-gateway/digipolis/business-party/v1/business-parties/6532/contracts/42
```

#### POST

Voor elk van onderstaande voorbeelden bevat de request body steeds de volledige resource representatie

-   Aanmaken van een nieuwe business-party. Merk op dat de post steeds gebeurt op de collection en niet op een individuele business party. Het id wordt hier immers niet meegegeven met de request, maar wordt bepaald tijdens resource creatie en meegegeven in het response bericht.
``` prettyprint
POST https://api-gateway/digipolis/business-party/v1/business-parties
```

-   Toevoegen van een nieuw contract aan business-party met id 6532
``` prettyprint
POST https://api-gateway/digipolis/business-party/v1/business-parties/6532/contracts
```

#### PATCH

Voor elk van onderstaande voorbeelden bevat de request body enkel die attributen die geupdate dienen te worden. Attributen die niet worden meegegeven blijven bijgevolg ook ongewijzigd. Het formaat waarin de te wijzigen attributen worden meegegeven zijn steeds conform de resource representatie.

-   Updaten van attributen set van contract 42 voor business-party met id 6532
``` prettyprint
PATCH https://api-gateway/digipolis/business-party/v1/business-parties/6532/contracts/42
```

			   
Stel dat contract 42 van business party 6532 volgende resource representatie heeft
``` json
{
	"id": 42,
	"startDate": "2012-01-01T01:00:00+01:00",
	"endDate": "2013-01-01T01:00:00+01:00",
	"contract": {
		"surname": "Doe",
		"name": "John",
		"email": "john.doe@dummy.com"
	},
	"pricing": {
		"currency": "Euro",
		"value": 850
	}
}
```
en je wil de prijs veranderen naar 950 Euro,<br/>
dan kan je dit als volgt doen

``` prettyprint
PATCH https://api-gateway/digipolis/business-party/v1/business-parties/6532/contracts/42
```
met als request body

``` json
{
	"pricing": {
		"value": 950
	}
}
```


#### DELETE

Voor elk van onderstaande voorbeelden is de request body steeds leeg

-   Verwijderen van contract met id 42 voor business-party met id 6532
``` prettyprint
DELETE https://api-gateway/digipolis/business-party/v1/business-parties/6532/contracts/42
```

-   Verwijderen van business-party met id 6532
``` prettyprint
DELETE https://api-gateway/digipolis/business-party/v1/business-parties/6532
```
