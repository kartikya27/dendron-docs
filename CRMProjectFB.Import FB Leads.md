---
id: 1eic19ue77qin2i6rt2u5qt
title: Import FB Lead
desc: ""
updated: 1681132949485
created: 1681132774879
---

> For FB Campaign Leads Import and save in DB with each Reference of campaign ads and leads.

```php
 public function importLeads(FacebookService $facebookService)
    {
        $importLeads = $facebookService->importLeads();

        foreach ($importLeads['adaccounts']['data'] as $key => $adAccounts) {
            foreach ($adAccounts['campaigns']['data'] as $key => $campaigns) {
                if (isset($campaigns['ads']['data'])) {
                    foreach ($campaigns['ads']['data'] as $key => $ads) {
                        if (isset($ads['leads']['data'])) {
                            foreach ($ads['leads']['data'] as $key => $lead) {
                                if (empty(Lead::where('lead_id', $lead['id'])->exists())) {
                                    $forLeadLayout = Lead::create([
                                        'type' => "Bulk Lead",
                                        'user_id' =>  ObjectId(auth()->user()->id),
                                        'fb_account_id' =>  $importLeads['id'],
                                        'fb_account_name' =>  $importLeads['name'],
                                        'ad_account_id' => $adAccounts['account_id'],
                                        'business_name' => $adAccounts['business_name'],
                                        'ad_account_name' => $adAccounts['name'],
                                        'campaign_id' => $campaigns['id'],
                                        'campaign_name' => $campaigns['name'],
                                        'ad_id' => $ads['id'],
                                        'ad_name' => $ads['name'],
                                        'adset_name' => $lead['adset_name'],
                                        'fb_lead_id' => $lead['id'],
                                        'lead_id' => $lead['id'],
                                        'lead_date' => $lead['created_time'],
                                        'fb_lead_form_id' => $lead['form_id'],
                                        'data' => $lead['field_data'],
                                    ]);

                                    //* getting Form Layout here
                                    $templateSet = FormLayout::where('ad_id', $ads['id'])->exists();
                                    if (empty($templateSet)) {
                                        foreach ($lead['field_data'] as $key => $field) {
                                            // dd($field);
                                            FormLayout::create([
                                                'type' => 'fb',
                                                'ad_id' => $ads['id'],
                                                'form_id' => ObjectId($forLeadLayout->id),
                                                'is_required' => 'yes',
                                                'field_name' => $field['name'],
                                                'original_field' => $field['name'],
                                                'value' => $field['values'][0],
                                            ]);
                                        }
                                    }

                                    // End here
                                }
                            }
                        }
                    }
                }
            }
        }
     return success("Bulk Leads", $importLeads)->response();
    }
```

---
