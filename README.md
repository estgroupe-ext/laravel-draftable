# Laravel-Draftable

Easily add status to your models in Laravel 5.

## 修改说明

1. `config/draftable.php` 配置增加 `status` 选项，增强扩展性。
2. Model 增加 `drafts` 方法，仅查询所有草稿。

## 安装及使用

1). 修改 `composer.json`, 在 `repositories` 数组内追加如下数据

```
    "repositories": [
        ...
        {
            "type": "vcs",
            "url":  "https://github.com/estgroupe-ext/laravel-draftable.git"
        }
    ]
```

2). 添加 `seriousjelly/laravel-draftable` package

```shell
    composer require estgroupe-ext/laravel-draftable
```

3). 修改 `config/app.php`, 在 `providers` 数组内追加如下数据

```php
    'providers' => array(
        ...
        'Seriousjelly\Draftable\ServiceProvider',
    )
```

4). 生成 migrations, 使其包含 `status` 字段

```
class AddModeratioColumnsToPostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('posts', function (Blueprint $table) {
            $table->enum('status', [
                config('draftable.status.drafts'),
                config('draftable.status.publish')
            ])->default(config('draftable.status.publish'));
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('posts', function(Blueprint $table)
        {
            $table->dropColumn('status');
        });
    }
}
```

5). 更新 Eloquent Models

```php
    use Seriousjelly\Draftable\DraftableTrait as Draftable;

    class Post extends Model
    {
        use Draftable;
    }
```

6). 使用

- 返回已发布的 posts
```php
    Post::get();
```

- 返回包含草稿和已发布的 posts
```php
    Post::withDrafts()->get();
```

- 返回只是草稿的 posts
```php
    Post::drafts()->get();
```

## Copyright and License

Laravel-Draftable was written by Chris Bratherton and released under the MIT License. See the LICENSE file for details.

Copyright 2015 Chris Bratherton
