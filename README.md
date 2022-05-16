# PHP wrapper for the google-search-console api

Wrapper for google/apiclient to access and retrieve data from Google Search Console using PHP.

This is a fork of  https://github.com/hkirsman/hkirsman-google-search-console

MODIFIED for private use, this might not suite your requirements, it is not a fully featured wrapper!!!

## Installation (using Composer)

* Clone this repository
* Enter composer update to install dependencies

## Setup google api credentials

* Go to  https://console.developers.google.com. 
* There you have to create a project if you don't have it already
* Enable the 'Google Search Console API' 
* Create a 'Service account key' and download the keyfile in json format.

## Add service account to your gsc-account

* Get a Service account ID like XXXX@developer.gserviceaccount.com in the developer console.
* Add it as a new user in the https://www.google.com/webmasters/tools/.

## Initiate the script

If you add the keyfile ("Service account key") in the root folder, then initiate the script with

```
$searchConsole = new SearchConsoleApi();
```

You can also add the keyfile to another folder. Then you have to define a custom path like this:

```
$searchConsole = new SearchConsoleApi('/foo/bar/service-account.json');
```

## Example

Try this example file. 

Replace 

* `http://www.example.com/` with your url 
* `'expression' => '/SUBPATH/',` with a subpath
* `'expression' => 'FRA',` with a language code

See API documentation from google: https://developers.google.com/webmaster-tools/v1/searchanalytics/query

```php
<?php

require_once 'vendor/autoload.php';

use HannesKirsman\GoogleSearchConsole\SearchConsoleApi;

$searchConsole = new SearchConsoleApi();
$options = SearchConsoleApi::getDefaultOptions();
$options['site_url'] = 'http://www.example.com/';
$options['start_date'] = date('Y-m-d', strtotime("-3 days"));;
$options['end_date'] = date('Y-m-d', strtotime("-3 days"));;
$options['setDimensionFilterGroups'] = [
  'filters' => [
      [
        'dimension'   => 'page',
        'operator'    => 'contains',
        'expression'  => '/SUBPATH/',
      ],
      [
        'dimension'   => 'country',
        'operator'    => 'equals',
        'expression'  => 'FRA',
      ]      
  ]
];

$rows = $searchConsole->getRows($options);
print_r($rows);
```
