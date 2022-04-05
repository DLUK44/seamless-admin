# Seamless Admin Panel

A seamless Django-like admin panel setup for Laravel. Simple, non-cms table manager for admins.

## Installation steps

1. Require the Package

Run the following command to install the package into your Laravel application:

```shell
composer require advaith/seamless-admin
```

2. Publish assets and config files with:

```shell
php artisan vendor:publish --provider="Advaith\SeamlessAdmin\SeamlessAdminServiceProvider"
```

This will create a `seamless.php` configuration file within your config folder. This will also publish all the assets
related to this package.

**Note**: If you find the UI or the admin functionalities are not working properly after a `composer update`, run this
command again with the flag `--tag="assets"` to republish the assets. This will override the existing assets with the
new ones.

## Usage

Use the provided trait, `SeamlessAdmin`, in any of your models to get started with the package. Example:

```php
<?php

namespace App\Models;

use Advaith\SeamlessAdmin\Traits\SeamlessAdmin;
...

class Post extends Model
{
    use SeamlessAdmin;
    ...
}
```

Et Voila! That's all you have to do to get started. Visit `/admin` to access the admin dashboard after logging in.

![](Screenshot.png)

## Configuration

### `seamless-admin.php` config file

- `prefix`: This option can be set to change the prefix of the admin routes. However, this will not change the name of
  the route. **Default**: `/admin`

### Model specific configuration

- `protected $primaryKey`: Primary key of the model will be used even if it is not `id` wherever it is needed.
- `adminIndexFields(): array`: Admin index page will display all the contents of `protected $fillable`
  excluding `protected $hidden` by default. To override this, use the method `adminIndexFields` and return an array of
  fields.
  ```php
  public function adminIndexFields(): array
  {
      return [
          'title',
          'content'
      ];
  }
  ```
- `adminOnCreate(array $fields): array`: Use the method to change the data saved on create. An example is for Users
  model:
  ```php
  public function adminOnCreate(array $fields): array
  {
      return [
          ...$fields,
          'password' => bcrypt($fields['password'])
      ];
  }
  ```
- `adminOnEdit(array $fields): array`: Use the method to change the data saved on edit.

#### Hooks

- `public function adminEdited(): void`: This hook is fired when a model is edited
- `public function adminCreated(): void`: This hook is fired when a model is created

More configuration options will be added soon. For requesting a new feature, create a new issue with the
label `feature-request`.
