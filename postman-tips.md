# Postman tips #
Ok, even wat context. Een dev team produceert regelmatig een nieuwe Swagger file voor hun API dat ze aan't bouwen zijn. Als tester, Product Owner of afnemer van deze API werk je met Postman.

## Wat doe je gewoonlijk ##
Dit is wat er typisch gedaan wordt:
1. Je importeerd de swagger file in een Postman collection. 
2. Je maakt een `Ènvironment file` in Postman aan.
3. Je voegt variabelen toe
4. Je gaat naar de Postman Collection en je begint her en der variabelen in te passen.
5. Nu kan je testen en spelen door API calls uit te voeren met telkens andere variabele waardes.

## Het Probleem ##
Als het Dev team met een nieuwe swagger komt, ben je enerzijds blij. Nieuwe features, bugfixes, etc. Anderzijds zie je ertegen op om heel de collection opbnieuw te importeren en alles terug variabel te maken.

## De Oplossing ##
Als het Dev team op bepaalde plaatsen in de swagger extra details invult, dan kan je puntje 4 overslaan en kan je met nagenoeg geen extra werk een nieuwe swagger file importeren. 

### host, basePath en scheme ###

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

In het bovenstaande is:
* voorzie een goed uitgewerkt info object (zie [Swagger goede documentatie](swagger-docs/md)
* Voeg een `host` element toe met `"{{baseurl}}"`. (Ik heb gemerkt dat je best enkel lowercase karakters gebruikt, bij de Postman import worden dit allemaal gelower cased)
* Voeg een `basePath` toe met waarde `"/"` of `"/myapi"` of ...
* Voeg een `schemes` toe met waarde `["https"]`

### query veriabelen ###
Goed nieuws. In de recentste versies van Postman hoef je niets meer te doen als je volgende syntax hanteert:

```js
todo
```


