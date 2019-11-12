```
cp vendor\beyondcode\laravel-websockets\config\websockets.php config\websockets.php
cp vendor\beyondcode\laravel-websockets\src\WebSocketsServiceProvider.php app\Providers\WebSocketsServiceProvider.php
cp vendor\beyondcode\laravel-websockets\resources\views\dashboard.blade.php resources\views\vendor\websockets\dashboard.blade.php

```

**bootstrap\app.php**

```
$app->configure('websockets');

$app->register(App\Providers\WebSocketsServiceProvider::class);
```

**app\Providers\WebSocketsServiceProvider.php**

```
Add follow lines
use BeyondCode\LaravelWebSockets\Console\StartWebSocketServer;
use BeyondCode\LaravelWebSockets\Console\CleanStatistics;

Replace
$this->commands([
    Console\StartWebSocketServer::class,
    Console\CleanStatistics::class,
]);
to
$this->commands([
    StartWebSocketServer::class,
    CleanStatistics::class,
]);


Replace all
__DIR__
to
base_path('vendor/beyondcode/laravel-websockets/src')

Update method: registerRoutes()
protected function registerRoutes()
{
    Route::group(['prefix' => config('websockets.path'), 'as' => 'websockets'], function () {
        Route::group(['middleware' => config('websockets.middleware', [AuthorizeDashboard::class])], function () {
            Route::get('/', ShowDashboard::class);
            Route::get('/api/{appId}/statistics', function () {
                return "OK";
            });
            Route::post('auth', AuthenticateDashboard::class);
            Route::post('event', SendMessage::class);
        });

        Route::group(['middleware' => AuthorizeStatistics::class], function () {
            Route::post('statistics', [
                'as' => 'statistics',
                'uses' =>
                WebSocketStatisticsEntriesController::class, 'store'
            ]);
        });
    });
    return $this;
}
```

**vendor\beyondcode\laravel-websockets\src\Statistics\Logger\HttpStatisticsLogger.php**

```
Replace
action([WebSocketStatisticsEntriesController::class, 'store']),
to
route('websockets.statistics'),
```

##Pusher Replacement
https://docs.beyondco.de/laravel-websockets/1.0/basic-usage/pusher.html#pusher-replacement
