## tabler

### installation

```sh
pnpm install @tabler/core \
        apexcharts \
        autosize \
        choices.js \
        countup.js \
        flatpickr \
        imask \
        nouislider \
        sweetalert2 \
        tom-select
```

### configuration

#### /app/Providers/AppServiceProvider.php

```php
<?php

namespace App\Providers;

use Illuminate\Pagination\Paginator;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     */
    public function register(): void
    {
        //
    }

    /**
     * Bootstrap any application services.
     */
    public function boot(): void
    {
        Paginator::useBootstrapFive();
    }
}
```

#### /resources/js/app.js

```js
import "@tabler/core/dist/js/tabler.min.js";
```

#### /resources/css/app.css

```css
@import "@tabler/core/dist/css/tabler.min.css";
@import "@tabler/core/dist/css/tabler-flags.min.css";
@import "@tabler/core/dist/css/tabler-payments.min.css";
@import "@tabler/core/dist/css/tabler-socials.min.css";
@import "@tabler/core/dist/css/tabler-vendors.min.css";
```

#### /resources/views/components/layouts/app.blade.php

```blade
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>{{ config('app.name') }}</title>

  <link rel="icon" href="{{ asset('favicon.ico') }}" type="image/x-icon">

  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">

  @if (file_exists(public_path('build/manifest.json')) || file_exists(public_path('hot')))
    @vite(['resources/css/app.css', 'resources/js/app.js'])
  @endif
</head>

<body>
  {{ $slot }}
</body>

</html>
```

---

## +livewire

### installation

```sh
composer require livewire/livewire
php artisan livewire:publish --config
```

### configuration

#### /config/livewire.php

```php
<?php

return [
    'pagination_theme' => 'bootstrap'
]
```
