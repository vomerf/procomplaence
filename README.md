# ООО «ПроКомплаенс»
Реализовано два endpoint-та при вызове первого вернется список заголовков всех csv файлов,
во втором реализована фильтрация и сортировка, сортировка выполняется по указанному загаловку(столбцу),
фильтрация происходит по указанному столбцу и значению в нем.
Технологии:
- Python 3.8
- Flask
- Docker

Можно запустить приложение без использования докера и с ним. Чтобы запустить приложение в контейнере докера<br>
нужно выолнить сборку образа из Dockerfile командой:<br>
`docker build -t <имя образа> .` точка значит что файл Docker находится в текущей директории.
После того как соберется образ нужно создать и запустить контейнер, выполнить это можно командой:<br>
`docker run -d -p 5000:5000 <имя образа>`.
Все образы можно посмотреть командой `docker images`. После того как контейнер запуститься будут доступны два endpount-та.<br>
Если вы хотите запусить без докера просто выполните команду `flask run` или `python app.py` из дирктории где находится файл app.py.
Примеры запросов(Доступны только GET запросы):<br>
Делаем запрос на endpoint /api/data/files<br>
При успешном выполнении вернется JSON со списком заголовком всех имеющихся файлов<br>
```
[
  {
    "columns": [
      "id",
      "firstname",
      "lastname",
      "email",
      "email2",
      "profession"
    ],
    "file": "1.csv"
  },
  {
    "columns": [
      "id",
      "firstname",
      "lastname",
      "email",
      "email2",
      "profession"
    ],
    "file": "2.csv"
  }
]
```
Если файлов нету вернется пустой список.<br>

Делаем запрос на endpoint /api/data/<имя файла без формата><br>
Если выпольнить запрос таким образом то вернутся все данные файла.<br>
Тут доступны query параметры котороые отвечают за фильтрацию и за сортировку.<br>
Параметр filters, передать можно следующим образом 127.0.0.1:5000/data/<имя файла без формата>?filters=<имя колонки>,<значение по которому идет фильтрация><br>
Если мы сделаем запрос 127.0.0.1:5000/data/<имя файла без формата>?filters=firstname,Darci<br>
Вернет все значения у которых в колонке firstname есть имя Darci<br>
Если такого значения нету вернет пустой спискок.<br>
```
[
  {
    "email": "Darci.Douglass@yopmail.com",
    "email2": "Darci.Douglass@gmail.com",
    "firstname": "Darci",
    "id": "58",
    "lastname": "Douglass",
    "profession": "worker"
  }
]
```
Параметр sort_by, передать можно следующим образом 127.0.0.1:5000/data/<имя файла без формата>?sort_by=<имя колонки><br>
Отсортирует по конкретному столбцу в лексикографическом порядке.<br>
К сожалению числа тоже будут сортировтся таким способом в данной реализации.<br>
Если мы сделаем запрос 127.0.0.1:5000/data/<имя файла без формата>?sort_by=firstname<br>
```
[
  {
    "email": "Adore.Ehrman@yopmail.com",
    "email2": "Adore.Ehrman@gmail.com",
    "firstname": "Adore",
    "id": "67",
    "lastname": "Ehrman",
    "profession": "police officer"
  },
  {
    "email": "Anestassia.Yorick@yopmail.com",
    "email2": "Anestassia.Yorick@gmail.com",
    "firstname": "Anestassia",
    "id": "57",
    "lastname": "Yorick",
    "profession": "firefighter"
  },
  {
    "email": "Annabela.Carolin@yopmail.com",
    "email2": "Annabela.Carolin@gmail.com",
    "firstname": "Annabela",
    "id": "87",
    "lastname": "Carolin",
    "profession": "worker"
  },
  {
    "email": "Averyl.Dreda@yopmail.com",
    "email2": "Averyl.Dreda@gmail.com",
    "firstname": "Averyl",
    "id": "88",
    "lastname": "Dreda",
    "profession": "developer"
  }
]
```
Query параметры filters и sort_by можно совмещать 127.0.0.1:5000/data/<имя файла без формата>?filters=firstname,Dora&sort_by=lastname<br>.
```
[
  {
    "email": "Dora.Ehrman@yopmail.com",
    "email2": "Dora.Ehrman@gmail.com",
    "firstname": "Dora",
    "id": "67",
    "lastname": "Ehrman",
    "profession": "police officer"
  },
  {
    "email": "Dora.Yorick@yopmail.com",
    "email2": "Dora.Yorick@gmail.com",
    "firstname": "Dora",
    "id": "57",
    "lastname": "Yorick",
    "profession": "firefighter"
  }
]
```
