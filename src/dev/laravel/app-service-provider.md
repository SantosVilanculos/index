### /app/Providers/AppServiceProvider.php

```php
<?php

namespace App\Providers;

use Carbon\CarbonImmutable;
use Illuminate\Support\Facades\DB;
use Illuminate\Pagination\Paginator;
use Illuminate\Support\Facades\Date;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Support\ServiceProvider;
use Illuminate\Validation\Rules\Password;

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
        DB::prohibitDestructiveCommands($this->app->isProduction());

        Model::shouldBeStrict(!$this->app->isProduction());
        Model::unguard();

        Password::defaults(fn() => $this->app->isProduction() ? Password::min(8) : null);

        Date::use(CarbonImmutable::class);

        RedirectIfAuthenticated::redirectUsing(fn() => '/');
    }
}
```
