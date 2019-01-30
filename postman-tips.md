# Postman tips #
Ok, even wat context. Een dev team produceert regelmatig een nieuwe Swagger file voor hun API dat ze aan't bouwen zijn. Als tester, Product Owner of afnemer van deze API werk je met Postman.

## Wat doe je gewoonlijk ##
Dit is wat er typisch gedaan wordt:
1. Je importeerd de swagger file in een Postman collection. 
2. Je maakt een `Ènvironment file`in Postman aan.
3. Je voegt variabelen toe
4. Je gaat naar de Postman Collection en je begint her en der variabelen in te passen.
5. Nu kan je testen en spelen door API calls uit te voeren met telkens andere variabele waardes.

## Het Probleem ##
Als het Dev team met een nieuwe swagger komt, ben je enerzijds blij. Nieuwe features, bugfixes, etc. Anderzijds zie je ertegen op om heel de collection opbnieuw te importeren en alles terug variabel te maken.

## De Oplossing ##
