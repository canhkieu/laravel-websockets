# Setup for lumen

```
composer require canhkieu/laravel-websockets
```

## Clone files

```
cp vendor\beyondcode\laravel-websockets\config\websockets.php config\websockets.php
cp vendor\canhkieu\laravel-websockets\src\resources\views\dashboard.blade.php resources\views\vendor\websockets\dashboard.blade.php

```

## Config

**bootstrap\app.php**

```
$app->configure('websockets');

$app->register(CanhKieu\LaravelWebSockets\WebSocketsServiceProvider::class);
```
