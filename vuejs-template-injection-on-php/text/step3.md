## 防御策

VueJS 側で `v-pre` を利用するのが手軽な対策です。  
`safe.php` は `v-pre` を用いて表示しているため、テンプレートインジェクションの影響を受けません。

また、JSON API としてやり取りを行うことが安全な対策です。
