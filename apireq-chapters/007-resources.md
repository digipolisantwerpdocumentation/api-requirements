## Resources

### URI structuur

Resources dienen steeds te worden benaderd volgens onderstaande URI structuur
```prettyprint
https://{hostname}/{namespace}/{vx}/{resource-URI}
```

API URI's dienen steeds te worden geversioneerd (**/vx**). Aangezien wordt gekozen voor het root namespace versioneringsmodel, dient de major versie steeds te worden opgenomen in de URI en dit achter de namespace.  
De **namespace** is een enkelvoudig zelfstandig naamwoord dat het onderwerp van de API omschrijft, gezien vanuit het standpunt van de API consumer.

Opmerking : op de API gateway bestaat de namespace uit organization/apiname

De **resource-URI** duidt op het eigenlijke resource model

Dit resulteert in onderstaand totaal voorbeeld :
``` prettyprint
https://api-gateway/digipolis/business-party/v1/…
```

### Naming conventions

-   Resources worden steeds in het meervoud gedefinieerd (uitzondering : controllers, status resource)
``` prettyprint
GET /partners
```

-   Gebruik steeds lowercase voor URI en query parameters. Dit vermijdt technologie afhankelijke problemen met casing.
``` prettyprint
GET /partners?page=10&pagesize=20
```

-   Gebruik geen underscores "\_" of dots "." in de URI. Gebruik hyphenation om woorden van elkaar te scheiden. Dit verhoogt de
    leesbaarheid van de URI.
``` prettyprint
GET /business-parties?page=10&pagesize=20
```

-   Gebruik geen trailing slash in de URI. Een trailing slash heeft geen toegevoegde waarde en verlaagt bovendien de leesbaarheid van de URI.
``` prettyprint
GET /partners/
gebruik je dus NIET
```

-   Gebruik geen fragments (\#) in de URI. Fragments worden gebruikt om te navigeren binnen een web context pagina, maar mogen niet gebruikt worden in een API URI.
``` prettyprint
GET /partners#name/
gebruik je dus NIET
```

### Media types en content negotiation

Media types en content negotiation dient **steeds** te gebeuren voor elke API call. Media types worden steeds gecommuniceerd via de HTTP Content-Type en Accept headers.

**Gebruik nooit file extenties in de URI om media types te communiceren.**
``` prettyprint
GET /partners.json of GET /partners.xml
gebruik je dus NIET
```

Als standaard media type gebruiken we **application/json**. Dit maakt dat de resource representatie standaard JSON is.

Definitie van een locale is optioneel. Dit zorgt er voor dat locatie specifieke formaten zoals datums of geldeenheden op een correcte manier worden voorgesteld.

Communicatie van de locale gebeurt steeds door middel van de Accept-Language en Content-Language headers.

De keuze van locale is API specifiek.

**Om onze services zo breed mogelijk bruikbaar te maken, is gekozen om alle API's in het Engels te maken**.

**Content-Type**: definiëert het formaat van het request & response bericht. Communicatie is zowel van consumer naar provider als van provider naar consumer.  
**Content-language**: definiëert de locale van het request & response bericht. Communicatie is zowel van consumer naar provider als van provider naar consumer.  
**Accept**: definieert de formaten van het response bericht dewelke de API consumer begrijpt en die door de API provider kunnen worden teruggestuurd. Communicatie is steeds van consumer naar provider.  
**Accept-language**: definieert de locale van het response bericht dewelke de API consumer begrijpt en dat door de API provider kan worden teruggestuurd. Communicatie is steeds van consumer naar provider.  
**application/json** is hierbij het geprefereerde formaat.
