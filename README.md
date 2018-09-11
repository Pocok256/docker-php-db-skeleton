* Az adatbázist másoljuk a local_dev mappába.
* docker-compose.override fájlból csináljunk még egy példányt, és nevezzük át docker-compose.override.yml re
* futtasuk a "docker-compose up php" parancsot.
* futtasuk a "docker-compose up db
* Esetlegesen adjuk hozzá a hosts fájlhoz a megfelelő elérési útvonalat, magyar dev site esetén:
127.0.0.1 mysite.hu.
* Docker futtatása után lehetséges, hogy a containerbe lévő fájlok nem lesznek megfelelő jogosultsággal ellátva,
ezt a következő paranccsal tudjuk javítani: 'docker-compose exec php chmod -R 777 . '