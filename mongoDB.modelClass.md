---
id: me94zik25625571tovttfiv
title: Model Class
desc: ''
updated: 1675491501748
created: 1675491482707
---

## Customise Table Name

```php
use Jenssegers\Mongodb\Eloquent\Model;

class Book extends Model
{
    protected $collection = 'my_books_collection';
}
```

### For Change Primary Key

```php
class Book extends Model
{
    protected $primaryKey = 'id';
}
```

### Soft Deletes

```php
class User extends Model
{
    use SoftDeletes;

    protected $dates = ['deleted_at'];
}
```


---
id: me94zik25625571tovttfiv
title: Model Class
desc: ""
updated: 1675491501748
created: 1675491482707
---

## Customise Table Name

```php
use Jenssegers\Mongodb\Eloquent\Model;

class Book extends Model
{
    protected $collection = 'my_books_collection';
}
```

### For Change Primary Key

```php
class Book extends Model
{
    protected $primaryKey = 'id';
}
```

### Soft Deletes

```php
class User extends Model
{
    use SoftDeletes;

    protected $dates = ['deleted_at'];
}
```