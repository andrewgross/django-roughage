[![Build Status](https://secure.travis-ci.org/Yipit/django-roughage.png)](http://travis-ci.org/Yipit/django-roughage)


### Install
First install roughage:  `pip install django-roughage`

Assume that your app/models.py looks like this
```python
class Book(models.Model):

    title = models.CharField(max_length=255)

class BookReport(models.Model):

    book = models.ForeignKey(Book)
    grade = models.IntegerField()
```

### Defining seeds.py
Then you define a seeds.py like the following:
```python
from roughage import Seed
from app.models import Book, BookReport

class BookSeed(Seed):

    model = Book

    def querysets(self):
        return [
            Book.objects.filter(id__lt=3)
        ]

class BookReportSeed(Seed):

    model = BookReport

    def trim_app__book(self, queryset):
        return queryset[:2]

```

### Planting and Reaping
Then run
```
$ ./manage.py plant --seeds=seeds.py --file=dev_dump.json
```

dev_dump.json will contain all books with id < 3 and the first two book reports attached to those books.

You can now import this into another database with
```
$ ./manage.py reap dev_dump.json
```