# フィルター

## デフォルト

---

### 条件

| Type | Operator   |
| ---- | ---------- |
| =    | =          |
| >    | >          |
| >=   | >=         |
| <    | <          |
| <=   | <=         |
| like | %{$value}% |
| !=   | !=         |

### ソート

| Type    | Operator   |
| ------- | ---------- |
| orderBy | ASC / DESC |

ソートの場合は value 属性の命名規則が「column_operator」になります。

```html
<label>Sort</label>
<select name="sort">
  <option value="created_at_asc">ASC</option>
  <option value="created_at_desc">DESC</option>
</select>
```
