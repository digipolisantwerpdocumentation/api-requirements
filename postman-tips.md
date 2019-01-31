# Postman tips #
Ok, even wat context. Een dev team produceert regelmatig een nieuwe Swagger file voor de API die ze aan't bouwen zijn. Als tester, Product Owner of afnemer van deze API werk je met Postman.

## Wat doe je gewoonlijk ##
Dit is wat er typisch gedaan wordt:
1. Je importeert de swagger file in een Postman collection. 
2. Je maakt een `Environment file` in Postman aan.
3. Je voegt variabelen toe aan de environment file
4. Je gaat naar de Postman Collection en je begint her en der vaste stukken in de url en headers te vervangen door de  gemaakte variabelen.
5. Nu kan je testen en spelen door API calls uit te voeren met telkens andere variabele waardes.

## Het Probleem ##
Als het Dev team met een nieuwe swagger komt, ben je enerzijds blij omdat er nieuwe features zijn toegevoegd, bug zijn opgelost, etc. Anderzijds zie je ertegen op om heel de collection opbnieuw te importeren en alles terug variabel te maken. Punt 4 van hierboven moet je dan helemaal opnieuw doen.

## De Oplossing ##
Als het Dev team op bepaalde plaatsen in de swagger extra details invult, of bepaalde syntax hanteert dan kan je puntje 4 overslaan en heb je nagenoeg geen extra werk een nieuwe swagger file importeren. 

Er zijn verschillende plaatsen waar variabelen tot stand komen
1. in het basis gedeelte van de url (scheme, host en basepath)
2. verderop als variabele delen in het path
3. variabele in het query gedeelte van het path


### 1. variabelen in scheme, host en basePath ###

Laten we een voorbeeld toevoegen:

```js
"info": {
        "title": "Deliverable Management API",
        "version": "1.0.2",
        "description": "Use this API to work with Deliverables, Assignments, Addons, etc on the OCAPI platform",
        "contact": {
            "name": "Studio Hyperdrive",
            "url": "http://www.studiohyperdrive.be",
            "email": "info@studiohyperdrive.be"
        }
    },
    "host": "{{baseUrl}}",
    "basePath": "/",
    "schemes": ["https"],
    "swagger": "2.0",
    "paths": {
        "/addons": {
```

Uit bovenstaand voorbeeld kan je de volgende richtlijnen destilleren :
* Voorzie een goed uitgewerkt swagger `info` object (zie ook [Swagger  documentatie tips](swagger-docs.md))
* Voeg een `host` element toe met `"{{baseurl}}"`. 

    *(Ik heb gemerkt dat je best enkel lowercase karakters gebruikt. Postman gaat bij een Swagger import enkel lowercased variabelen maken)*
* Voeg een `basePath` toe met waarde `"/"` of `"/myapi"` of ...
* Voeg een `schemes` toe met waarde `["https"]` (of andere volgens de swagger spec)

### 2. variabelen in het path ###
Goed nieuws. In de recentste versies van Postman hoef je niets meer te doen. In de Swagger 2.0 spec kan je gebruik maken van [Path Templating](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#pathTemplating) voor de opbouw van het path (zie ook [Path Item object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#path-item-object))

Het komt erop neer dat je variabele stukken in het pad - gewoonlijk voor een **id** - tussen brackets `{}` zet.

```js
    "paths": {
        "/deliverables/{deliverableid}/addons": {
            "get": {
                "description": "Get a list of addons from the deliverable"
            }
        }
    }
```

### 3. query variable ###
Nog wat verderop in het path heb je het query gedeelte, ofwel alles achter het vraagteken `?`. In een Swagger file gebruik je hiervoor het [Parameters Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#parameterObject). 

Als je onderstaande syntax hanteert, zal Postman de parameter `q` automatisch als variabele herkennen bij de import.

```js
    "paths": {
        "/addons": {
            "get": {
                "description": "Get a list of addons that a user can select.",
                "summary":"Find addons with an optional search query",
                "parameters": [
                    {
                        "name": "q",
                        "in": "query",
                        "description": "Search query",
                        "required": false,
                        "type": "string"
                    }
                ],
```


### 4. header variable ###
In hetzelfde [Parameters Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md#parameterObject) van de Swagger spec kan je ook parameters definieren voor in de header. Ook deze gaat Postman herkennen als variabele.

```js
    "paths": {
        "/addons": {
            "get": {
                "description": "Get a list of addons that a user can select.",
                "summary":"Find addons with an optional search query",
                "parameters": [
                    {
                        "name": "authorization",
                        "description": "Security token to access the service",
                        "in": "header",
                        "required": true,
                        "type": "string"
                    }
                ],

```
