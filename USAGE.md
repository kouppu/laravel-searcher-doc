# 使用方法

## はじめに

---

User モデルを検索する例で進めていきます。

<br>

## config/app.php

---

プロジェクトの app.php に下記を追加してください。

```php
'providers' => [
    Suhrr\LaravelSearcher\ServiceProvider::class,
],

'aliases' => [
    'Searcher' => \Suhrr\LaravelSearcher\Facade::class
],
```

<br>

## Search クラス

---

検索条件等を設定する Search クラスを作成し、編集します。

### Search クラスを作成

コンソールで UserSearch ファイルを作成します。
App\Searches\User に作成されます。

```
php artisan make:search User
```

### Search クラスを編集

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
        ];
        // ページネーションを使用する場合はtrueに
        $this->isPaginate = true;
    }
}

```

<br>

## コントローラー

---

Searcher クラスを呼び出し、User モデルを引数にセットし検索結果を取得します。

```php
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use Searcher;

class UserController extends Controller
{
    public function index(Request $request, User $user)
    {
        if ($request->has('reset')) {
            // reset
            $users = Searcher::reset($user);
        } else {
            // search
            $users = Searcher::search($user);
        }
        return view('users.index', compact('users'));
    }
}
```
