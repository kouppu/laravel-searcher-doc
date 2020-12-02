# 拡張フィルター

独自のフィルターを実装する場合。<br>
User モデルの 入力値+10 歳間を検索する MyAgeFilter を作成する例で進めていきます。

## Filter クラス

---

拡張フィルタークラスを作成し、編集します。

### Filter クラスを作成

コンソールで MyAgeFilter ファイルを作成します。
App\Searches\User\Filters に作成されます。

- 引数:
  - String $name フィルター名
  - String $model 対象モデル名

```
php artisan make:filter MyAgeFilter User
```

### Filter クラスを編集

apply 関数内に独自の処理実装します。

```php
<?php

namespace App\Searches\User\Filters;

use Illuminate\Database\Eloquent\Builder;
use Suhrr\LaravelSearcher\Search\FilterInterface;

class MyAgeFilter implements FilterInterface
{
    public static function apply(Builder $builder, string $column, $value): Builder
    {
        return $builder->whereBetween($column, [$value, $value + 10]);
    }
}

```

<br>

## Search クラスを編集

params 配列に記述します。

```php
<?php

namespace App\Searches\User;

use Suhrr\LaravelSearcher\Search\AbstractSearch;

class UserSearch extends AbstractSearch
{
    public function __construct()
    {
        parent::__construct();

        $this->params = [
            [
                // input HTMLタグのname属性名
                'name' => 'name',
                // Userテーブルのカラム名
                'column' => 'name',
                // 検索条件
                'type' => 'like'
            ],
            [
                'age' => 'age',
                'column' => 'age',
                // 拡張フィルター名
                'type' => 'MyAgeFilter'
            ],
        ];
        // ページネーションを使用する場合はtrueに
        $this->isPaginate = true;
    }
}

```
