# Laravel Html table builder
Coming soon

# Installation
The preferred way to install extension is via composer. Check the composer.json for this extension's requirements and dependencies.

To install, either run
```php
$ php composer.phar require aayaresko/laravel-table
```
or add
```php
"aayaresko/laravel-table": "^1.0"
```
to the require section of your composer.json.

# Configuration
It does not require any additional configuration.

# Usage
Simply create a new instance of <code>TablesFacade</code> and pass to it all required parameters:
```php
$table = new TablesFacade($data_provider, $attributes, $default_actions_route);
```
<code>$data_provider</code> is used as a models source. 
It should be either an instance of <code>Illuminate\Contracts\Pagination\LengthAwarePaginator</code> or <code>Illuminate\Database\Eloquent\Collection</code>.

<code>$attributes</code> array holds list of attributes, that should be rendered in each table row.
You can use 'dot' syntax to target attribute value of related models:
```php
$table = new TablesFacade(
    $data_provider, 
    [
        'nickname',
        'email',
        'profile.full_name',
        'created',        
    ], 
    $default_actions_route
);
```
In your view you should place something like this:
```blade
<div class="table-responsive">
    {{ $table->renderTable() }}
    {{ $table->renderLinks() }}
</div>
```
You scan use <code>renderLinks()</code> method only when you use <code>Illuminate\Contracts\Pagination\LengthAwarePaginator</code> as <code>$data_provider</code>.

You can specify custom column name for any attribute:
```php
$table = new TablesFacade(
    $data_provider, 
    [
        'Alias' => 'nickname',
        'email',
        'Full name' => 'profile.full_name',
    ], 
    $default_actions_route
);
```
That custom name will be translated automatically.

You can attach a callback to render any attribute value:
```php
$table = new TablesFacade(
    $data_provider, 
    [
        'nickname',
        'email',
        'profile.full_name',
        'created',   
        'alias' => function ($model) {
            return $model->alias;
        }
    ], 
    $default_actions_route
);
```
Function signature is pretty straightforward: <code>function ($model) {}</code>.

<code>$default_actions_route</code> is route which will be used as 'parent' to generate links for all action buttons.

You can configure your own list of action buttons via <code>$action_buttons</code> property.
```php
'show' => [
    'title' => '<i class="glyphicon glyphicon-eye-open"></i>',
    'route' => 'backend.accounts',
],
'edit' => [
    'title' => '<i class="glyphicon glyphicon-pencil"></i>',
    'route' => 'frontend.accounts',
],
'destroy' => [
    'title' => 'My button content',
    'route' => 'site',
    'options' => [
        'class' => 'delete-ajax',
    ]
]
```
You can specify any html options for any button via button <code>options</code> array.

You can specify attributes values for the table itself, tr and td tags of the table via <code>$table_options</code>, <code>$row_options</code> and <code>$item_options</code> respectively.

You can specify text, which will be displayed in case of no models via <code>$not_found_text</code>.