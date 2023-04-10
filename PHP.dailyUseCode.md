---
id: 1dbfwp1kwxrjtgl4kuj10k
title: Daily Use Code PHP
desc: ""
updated: 1681133040908
created: 1674306759446
---

> **For Convert numeric value to english Word**

```php
function convertNumberToWord($str)
    {
        if (preg_match('/^\d+/', $str)) {
            $str = str_replace(['0', '1', '2', '3','4', '5', '6', '7', '8', '9'], ['Zero','One', 'Two', 'Three', 'Four', 'Five','Six', 'Seven', 'Eight', 'Nine'], $str);
        }
        return implode(' ', array_slice(explode(' ',$str), 0, 2));
    }
```
