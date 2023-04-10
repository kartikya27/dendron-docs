---
id: ft2d7n40oscooq954zy5rsn
title: Excel Import
desc: ""
updated: 1681132998909
created: 1681132569666
---

## For Import Data from Excel

> In Controller

```php
    public function excelImportLead(Request $request)
    {
        $file = $request['csv'];

        try {
            $collection = Excel::import(new LeadsImport, $file, 'public', \Maatwebsite\Excel\Excel::CSV);
        } catch (\Maatwebsite\Excel\Validators\ValidationException $e) {
            //
        }
    }
```

> In LeadImport.php

```php
<?php

namespace App\Imports;

use App\Models\Lead;
use App\Models\LeadImportLog;
use Illuminate\Support\Collection;
use Maatwebsite\Excel\Concerns\ToCollection;
use Maatwebsite\Excel\Concerns\Importable;
use Maatwebsite\Excel\Concerns\WithHeadingRow;
use Maatwebsite\Excel\Concerns\WithValidation;
use Maatwebsite\Excel\Concerns\SkipsOnFailure;
use Maatwebsite\Excel\Concerns\SkipsEmptyRows;
use Maatwebsite\Excel\Validators\Failure;
use Illuminate\Support\Str;
use Maatwebsite\Excel\Concerns\WithChunkReading;
use Maatwebsite\Excel\Events\AfterImport;
use Maatwebsite\Excel\Concerns\RegistersEventListeners;
use Maatwebsite\Excel\Concerns\WithEvents;

class LeadsImport implements ToCollection, SkipsEmptyRows, WithHeadingRow, WithValidation,  SkipsOnFailure, WithChunkReading,  WithEvents
{
    use Importable, RegistersEventListeners;
    private $row = 1;
    private $rowsCompleted = 0;
    private $errors = [];
    /**
     * @param array $row
     *
     * @return \Illuminate\Database\Eloquent\Model|null
     */
    public function collection(Collection $rows)
    {
        foreach ($rows as $row) {
            ++$this->row;
            $form_id = Str::squish($row['form_id']);

            Lead::create([
                'form_id' => $form_id,
            ]);

            ++$this->rowsCompleted;
        }
    }

    public function rules(): array
    {
        return [
            'form_id' => ['required', 'min:2', 'max:120'],
        ];
    }

    public function onFailure(Failure ...$failures)
    {
        foreach ($failures as $failure) {
            $this->errors[] = [
                'row' => $failure->row(),
                'attribute' => $failure->attribute(),
                'errors' => $failure->errors(),
                'values' => $failure->values()
            ];
        }
    }

    public function registerEvents(): array
    {
        return [
            // Handle by a closure.
            AfterImport::class => function (AfterImport $event) {

                $totalRows = $event->getReader()->getTotalRows();

                LeadImportLog::create([
                    'rows_completed' => $this->rowsCompleted,
                    'total_rows' => ($totalRows['Worksheet'] - 1),
                    'errors' => json_encode($this->errors)
                ]);
            },
        ];
    }

    public function chunkSize(): int
    {
        return 1000;
    }
}

```

> Model For Log Information

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;

class LeadImportLog extends BaseModel
{
    use HasFactory;

    protected $fillable = [
        'rows_completed',
        'total_rows',
        'errors',
    ];
}
```
