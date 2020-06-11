## Paginatie
### Pagineren van collecties
#### Gebruik altijd paginering bij het ophalen van collecties
##### R-PC-001
Paginatie informatie wordt **steeds** terug gegeven bij het ophalen van collections. Dit zorgt er voor dat collections, die in eerste instantie een beperkt aantal entiteiten bevatten en op termijn groot kunnen worden, performant kunnen blijven.

#### Gebruik altijd het vastgelegde pagineringsmodel bij het ophalen van collecties
##### R-PC-002
Vanuit consumer standpunt is het noodzakelijk dat volgende informatie in de response wordt gegeven om voldoende informatie te bekomen rond de pagina's:

| Info                         |                             | Verplicht                       |
| ---------------------------- | --------------------------- | ------------------------------- |
| Link naar de eerste pagina   |                             | Ja                              |
| Link naar de laatste pagina  |                             | Ja                              |
| Link naar de vorige pagina   |                             | Ja, behalve voor eerste pagina  |
| Link naar de volgende pagina |                             | Ja, behalve voor laatste pagina |
| Extra metadata :             |                             |                                 |
|                              | Huidig paginanummer         | Ja                              |
|                              | Aantal elementen per pagina | Ja                              |
|                              | Totaal aantal elementen     | Afhankelijk van paging-strategy |
|                              | Totaal aantal pagina's      | Afhankelijk van paging-strategy |


In veel paginatierichtlijnen wordt gevraagd om het totaal aantal pagina's en/of elementen altijd terug te geven zodat de gebruiker exact kan geïnformeerd worden over de lengte van de opgevraagde lijst. Dit houdt in dat er impliciet altijd een 'count' mechanisme moet geïmplementeerd worden wat bij lijsten van grote aantallen tot een merkbare vertraging kan leiden in het opvragen van de lijst.

Daarom kiezen we met onze API requirements voor een oplossing die 2 strategieën implementeert :
- een snellere opvraging zonder verplichte 'count'
- een potentieel tragere opvraging tengevolge een impliciet 'count'-mechanisme

De client toepassing kiest welke strategie wordt toegepast dmv van een (optionele) query parameter : **`paging-strategy`** (zie verder).

#### Ondersteun beide `paging-strategy` methodes
##### R-PC-003
Een API moet altijd beide `paging-strategy` methodes ondersteunen, zijnde `withCount`en `noCount` (zie verder).  

### Paginatie query parameters
#### Gebruik de vastgelegde query parameters `page`, `pagesize` en `paging-strategy` voor paginering
##### R-PQ-001
Het ophalen van een bepaalde pagina zelf dient te gebeuren door middel van de **`page`** en **`pagesize`** query parameters (behalve voor de `last` link bij `paging-strategy=noCount`, zie verder).
``` prettyprint
/partners?page=1&pagesize=10
```

De paginatie query parameters zijn **optioneel**. Dat maakt dat wanneer deze **niet** zijn opgegeven:

-   Een beperkte set van resources wordt teruggegeven bij het bevragen van collections op basis van een default page size die voor elke API wordt bepaald.
-   Steeds de eerste pagina wordt terug gegeven.

**Bij het ophalen van een collection zonder specificatie van query parameters dient paginatie informatie altijd aanwezig te zijn in de
response message**, gebruik makend van de `withCount` paginatie strategie.    
Het aantal elementen dat in zulk geval wordt teruggegeven (page size) is API specifiek en dient te worden bepaald tijdens de API design fase.

#### Paginering is `1` based
##### R-PQ-002
Paginatie queries starten steeds met *page=1*, niet 0. De keuze hiervoor is gemaakt op basis van gebruiksvriendelijkheid naar de API consumer en gebruiker toe.

#### Gebruik `withCount` of `noCount` waarden voor de paging strategie 
##### R-PQ-003
Om de paging strategie mee te geven, gebruikt de consumer de optionele parameter **`paging-strategy`**. Deze heeft 2 mogelijke waardes : 
- withCount (default als de query parameter niet wordt meegegeven)
- noCount
 
 Bij **`withCount`** worden __`Totaal aantal elementen`__ en __`Totaal aantal pagina's`__ altijd verplicht terug gegeven. Bovendien bevat de __`link naar de laatste pagina`__ het paginanummer (zie verder voor een voorbeeld). 
Bij **`noCount`** worden de beide totalen niet terug gegeven en is de link naar de laatste pagina een link zonder paginanummer met de vermelding **`last`** (zie verder voor een voorbeeld).

### Paginatie response bericht
#### Gebruik de HAL specificatie voor gepagineerde responses
##### R-PR-001
Om paginatie informatie naar de consumer terug te sturen baseren we ons op de HAL specificatie:

<http://stateless.co/hal_specification.html>

<https://datatracker.ietf.org/doc/draft-kelly-json-hal/>

De HAL specificatie is gedefinieerd voor clients die doorheen hun resources navigeren door middel van links en die dus willen streven naar een HATEOAS (Hypermedia As The Engine Of Application State) hypermedia API ([Richardson Maturity Model Level
3](http://martinfowler.com/articles/richardsonMaturityModel.html#level3)).

Voordelen van de HAL specificatie:

-   In de toekomst kunnen links per record van de collection worden gedefiniëerd (HATEOAS)
-   Biedt de meeste mogelijkheden, ook indien in de toekomst eventueel naar hypermedia wordt gekeken.

Aangezien Digipolis deze optie open wilt houden voor de toekomst, werd gekozen om reeds gebruik te maken van deze structuur voor paginatie.

Het reserved keyword **\_links** wordt gebruikt om links naar andere pagina's te definiëren.

In de toekomst kan dit worden uitgebreid om links naar andere objecten aan te duiden indien hypermedia APIs worden gedefinieerd. Binnen
Digipolis wordt gekozen om volgende link objecten te gebruiken voor paginatie:

| link object | omschrijving                                                | opmerking |
| ----------- | ----------------------------------------------------------- | --------- |
| self        | bevat een link naar de pagina zelf                          |                                                                     |
| first       | bevat een link naar de eerste pagina binnen de collection   | page=1                                                              |
| last        | bevat een link naar de laatste pagina binnen de collection  | bij **`noCount`** paging strategie : page=last                                   | 
| prev        | bevat een link naar de vorige pagina binnen de collection   | wordt weggelaten indien er geen vorige pagina is (eerste pagina)<br />page=huidige pagina - 1    |
| next        | bevat een link naar de volgende pagina binnen de collection | wordt weggelaten indien er geen volgende pagina is (laatste pagina)<br />page=huidige pagina + 1<br />bij **`noCount`** paging strategie kan dit naar een lege pagina verwijzen |

De HAL specificatie definieert dat elk van deze link objecten verplicht de href property moet hebben. Daarnaast zijn er ook nog een aantal optionele properties gedefinieerd binnen de standaard. Deze hebben geen toegevoegde waarde bij het weergeven van pagina informatie en worden dan ook niet toegevoegd voor elk van bovenvermelde link objecten.

Dit alles resulteert in volgende structuur in de response message om pagina links weer te geven.
```json
{
 "_links": {
   "self": {
       "href": "..."
     },
     "first": {
       "href": "..."
     },
     "last": {
       "href": "..."
     },
     "prev": {
       "href": "..."
     },
     "next": {
       "href": "..."
     }
 }
}
```

Bij het ophalen van collections dient bovenstaande structuur steeds aanwezig te zijn in een response message. Dit maakt dat de eigenlijke resources in een wrapper object komen te zitten in het response bericht.  
Dit kan door middel van het **\_embedded** reserved keyword. Dit \_embedded object bestaat uit property namen dewelke een link relation
type voorstellen en wiens waarde 1 of meerdere resource objecten zijn.  
Dit resulteert in volgende structuur.
```json
{
"_embedded": {
       "resourceList": [{
      },
 {
   }]
  }
}
```

Het **\_page** reserved keyword vormt geen onderdeel van de HAL specificatie, maar is extra metadata die in de response message komt om
een indicatie te krijgen van de huidige paginanummer, aantal elementen per pagina, het totaal aantal pagina's en het totaal aantal elementen en vereenvoudigt de bewerkingen langs consumer kant om deze informatie te bekomen. Bij **`paging-strategy=noCount`** worden `totalElements` en `totalPages` weg gelaten.  
  
Voorbeeld bij `paging-strategy=withCount` : 
```json
{
"_page": {
    "size": 10,
    "totalElements": 73853,
    "totalPages": 7386,
    "number": 1
  }
}
``` 
  
Voorbeeld bij `paging-strategy=noCount` : 
```json
{
"_page": {
    "size": 10,
    "number": 1
  }
}
``` 

Alle aspecten van paginatie samenvoegend geeft dit volgende response wrapper message voor paginatie:
```json
{
   "_links": {
     "self": {
         "href": "..."
     },
     "first": {
         "href": "..."
     },
     "last": {
         "href": "..."
     },
     "prev": {
         "href": "..."
     },
     "next": {
         "href": "..."
     }
   },
   "_embedded": {
     "resourceList": [{
        },
    {
        }]
   },
   "_page": {
     "size": 10,
     "totalElements": 73853,
     "totalPages": 7386,
     "number": 1
   }
}
```

Zoals reeds vermeld vallen `totalElements` en `totalPages` weg bij `paging-strategy=noCount`.  

#### Gebruik media type `application/hal+json` voor gepagineerde responses
##### R-PR-002
Het voorziene media type is steeds **`application/hal+json`**


#### Voorbeelden

Het ophalen van business parties (withCount)  

``` prettyprint
https://api-gateway/digipolis/business-party/v1/business-parties?paging-strategy=withCount  
```

Geeft als resultaat
```json
{
   "_links": {
     "self": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties"
     },
     "first": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties?page=1&pagesize=10"
     },
     "last": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties?page=7386&pagesize=10"
     },
     "next": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties?page=2&pagesize=10"
     }
   },
   "_embedded": {
     "business-parties": [{
        },
    {
        }]
   },
   "_page": {
     "size": 10,
     "totalElements": 73853,
     "totalPages": 7386,
     "number": 1
   }
}
```

Het ophalen van business parties (noCount)  

``` prettyprint
https://api-gateway/digipolis/business-party/v1/business-parties?paging-strategy=noCount  
```

Geeft als resultaat
```json
{
   "_links": {
     "self": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties"
     },
     "first": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties?page=1&pagesize=10"
     },
     "last": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties?page=last&pagesize=10"
     },
     "next": {
         "href": "https://api-gateway/digipolis/business-party/v1/business-parties?page=2&pagesize=10"
     }
   },
   "_embedded": {
     "business-parties": [{
        },
    {
        }]
   },
   "_page": {
     "size": 10,
     "number": 1
   }
}
```

In de **last** link wordt gebruik gemaakt van **page=last** ipv het exacte paginanummer om te vermijden dat een count moet uitgevoerd worden bij het ophalen van de lijst. Dit houdt wel impliciet in **dat de API deze waarde (=last) ook verplicht moet ondersteunen**. Pas wanneer deze wordt gebruikt zal de API een count uitvoeren om op dat moment de laatste pagina te berekenen en terug te geven.  



