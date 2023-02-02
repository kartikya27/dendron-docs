---
id: 
title: Tutorial
desc: ""
updated: 1660931467392
created: 1659721741451
currentStep: 0
totalSteps: 0
---

## Customise Table Name

```php
use Jenssegers\Mongodb\Eloquent\Model;

class Book extends Model
{
    protected $collection = 'my_books_collection';
}
```

For Change Primary Key
```php
class Book extends Model
{
    protected $primaryKey = 'id';
}
```