# Searchable Plugin
Searchable for any model plugin for OctoberCMS. This plugin is inspired by [Searchable Trait](https://github.com/nicolaslopezj/searchable) and modified into OctoberCMS' behavior style.

## Usage

Add the behavior to your model and your search rules.

```php
class Product extends Model
{
    public $implement = [
        'Octobro.Searchable.Behaviors.Searchable'
    ];

    /**
     * Searchable rules.
     *
     * @var array
     */
    protected $searchable = [
        /**
         * Columns and their priority in search results.
         * Columns with higher values are more important.
         * Columns with equal values have equal importance.
         *
         * @var array
         */
        'columns' => [
            'users.first_name' => 10,
            'users.last_name' => 10,
            'users.bio' => 2,
            'users.email' => 5,
            'posts.title' => 2,
            'posts.body' => 1,
        ],
        'joins' => [
            'posts' => ['users.id','posts.user_id'],
        ],
    ];

}
```

If you want to implement to another plugin, you have to extend it by adding on `boot()` plugin method like this.

```php
//
// Extend RainLab.User
//
User::extend(function($model) {
    $model->implement[] = 'Octobro.Searchable.Behaviors.Searchable';

    $model->addDynamicProperty('searchable', [
        'columns' => [
            'users.name' => 20,
        ],
    ]);
});

```

That's all, you can search using this query easily.

```php
// Simple search
$users = User::search($query)->get();

// Search and get relations
// It will not get the relations if you don't do this
$users = User::search($query)
          ->with('posts')
          ->get();
```
