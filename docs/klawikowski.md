Technologie noSQL - 2013 - Klawikowski
============

Dane
-----------
Moje dane zostały pobrane z serwisu

  http://www.json-generator.com/
  
Dane przedstawiają przykładowe dane ankietowanych osób

Przykład rekordu
-----------
``` json
{
  "age": 27,
  "name": "Zoe Neal",
  "gender": "male",
  "company": "OpKeycomm",
  "phone": "808-525-3729",
  "email": "zoe@opkeycomm.com",
  "address": "30327, Cambridge, Greene Street",
  "about": "Aliquip diam exerci erat ex molestie, exerci eum et minim consequat, aliquip consectetuer velit duis. Aliquip, euismod sed nisl veniam praesent, euismod ut suscipit erat vulputate, ipsum vero tincidunt. Accumsan delenit, amet zzril illum luptatum dolore, ut velit dolore amet facilisi, dolor ipsum. Nulla esse ex, eros luptatum ea erat hendrerit, ut hendrerit facilisi aliquip tation, enim. Dolore euismod volutpat nulla, vulputate eu wisi ut molestie, volutpat duis ea dolor nostrud, ullamcorper et augue facilisi augue, laoreet at dignissim exerci euismod.",
  "registered": "2009-08-10T14:17:32 -02:00",
  "weight": 74
}
```
Oczyszczenie rekordu
-----------
Rekordy te oczyściłem z mało interesujących mnie danych przy użyciu Google Refine:

  http://code.google.com/p/google-refine/
  ----------
  usunięto pola:

``` json
{
  company
  phone
  email
  address
  about
  registered
}
```
  ----------
  po oczyszczeniu rekord wygląda następująco
  
  
``` json
{
  "age": 32,
  "name": "Julia Harrison",
  "gender": "female",
  "weight": 102
}
```

Import do MongoDB
-----------
Aby zaimportowac dane z pliku `*.json` należy uruchomić skrypt `mongoimport.sh` z odpowiednimi argumentami:
``` bash
#$1 - baza danych
#$2 - kolekcja
#$3 - plik json
```
Dane są domyślnie importowane do bazy działającej na `http://localhost:27017`
W moim przypadku wywołanie wygląda następująco:
``` bash
./mongoimport.sh main main generated.json 
```

Komunikat który otrzymałem:
```bash
Importowanie danych z pliku: generated.json do bazy main
connected to: localhost
Mon Jun  3 12:19:29.774 check 9 1000
Mon Jun  3 12:19:29.797 imported 1000 objects
Success
```

Agregacja
-----------
> Przykład nr #1 - Zlicza ilość kobiet i mężczyzn których waga wynosi więcej niż 90

``` bash
db.main.aggregate({$group:{_id:"$name",weight:{$sum:"$weight"}}},{$match:{weight:{$gte:90}}});
```
Wynik agregacji:
``` json
		{
			"_id" : "Brooklyn Campbell",
			"weight" : 98
		},
		{
			"_id" : "Hannah Gerald",
			"weight" : 108
		},
		{
			"_id" : "Peyton Gilson",
			"weight" : 119
		},
		{
			"_id" : "Avery Brown",
			"weight" : 98
		},
		{
			"_id" : "Anna Turner",
			"weight" : 98
		},
		{
			"_id" : "Zoe Carey",
			"weight" : 117
		},
		{
			"_id" : "Evelyn Watson",
			"weight" : 115
		},
		{
			"_id" : "Grace Chandter",
			"weight" : 105
		},
		{
			"_id" : "Layla Sherlock",
			"weight" : 101
		},

```

