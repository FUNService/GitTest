# Gitを始めよう（チーム開発編）
## 基本的な流れ
1. `git clone <リポジトリURL>`でクローンする
- `git branch <branch name>`で新規ブランチの作成。現在いるブランチから切られるので注意。
- `git checkout <branch name>`でブランチの移動。
 - `git checkout -b <branch name>`とやると。ブランチ作成と移動が一発で出来て楽。
- ゴニョゴニョとファイル編集
- 他のメンバーによってファイル変更があった時は適宜`git pull`して`git merge`する。
- `git add -u(-A)`してインデックスにステージングさせる
- `git commit -m "commit message"`でステージングさせたインデックスを元にコミット作成
- `git push origin <ブランチ名>`でリポジトリにプッシュする

## 注意すべきこと
- `git add`オプションの`-u`と`-A`はきちんと使い分けること！
 - `-u`は編集ファイルのみ。`-A`は`-u`+新規作成と削除したファイルをステージングさせる。
- コミットメッセージは端的且つ分かりやすく **英語**でかくこと。[こちらのサイト](http://qiita.com/magicant/items/882b5142c4d5064933bc)を参考にすること。
- `git status`で現在のブランチ名や編集したファイルの状態（addしたかとか）が確認できる
- `git log`でコミット履歴を確認できる。オプションによって見やすくもできる(`--graph`でコミットのグラフ化、`-p`で差分表示などなど)
- commit やlog の編集エディタのデフォルトは`vi`なので注意。
- コンフリクトが発生しても怯むな

## `rebase` or `merge`
どっちがいいの？とよく議論されている。
[[Git] 使い分けできていますか？マージ（merge）&リベース（rebase）再入門](http://powerful-code.com/blog/2012/11/merge-or-rebase/)
ここで詳しく解説があったので暇な時にでも読んでみてください
### rebase
参考：[初心者でも分かる！git rebaseの使い方を解説します](http://liginc.co.jp/web/tool/79390)

- メリット
 - コミットグラフがきれいな状態で保てる

- デメリット
 - コンフリクトした時、コミットの数だけ解消しなければならない
 - `git push origin`を投げた後に`git rebase`すると、リモートのブランチを消さない限り再度`git push origin`することは不可能

デメリットの原因は`rebase`した際に、自身のコミット番号が変わってしまうからです。`rebase`を行うのであれば、できるだけ少ないコミット数であることをおすすめします。`git rebase -i`でコミットをまとめたり、`git reset --hard(--soft)`でコミット削除などできるのでそれらを使うのもよし。

### merge
- メリット
 - `rebase`と比べて、コンフリクト時の対処が楽
 - コミット履歴を残したままにできる

- デメリット
 - コミットグラフがごちゃごちゃしやすい
