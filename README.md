# 一連の流れ
- データを読み込む
- 明らかに関係のない特徴量の列、欠損値が1万異常あるかつ、よくわからない特徴量を削る
- 欠損値の補間(order_of_finish, odds)
- 文字列をワンホットベクトルに変換(sex, weahter)
- order_of_finishを目的変数、それ以外の特徴量を説明変数とみなして重回帰分析にかける


# input data
今回読み込んだデータは、Kiwataさんがにスクレイピングしていただいたデータのsqlite3ファイルからfeatureというテーブルをcsvとして出力したものを使用した。
その出力したcsvファイルがこのREADME.mdと同じ階層にあるfeature.csvである
参考までにsqlite3で出力するまでのコマンドを記録する($,sqlite>はプロンプトなので実際に入力することはありません)
```sqlite
//起動
$ sqlite3 ./race.db
//DBの内容表示
sqlite> .schema
//csv出力
sqlite> .headers on
sqlite> .mode csv
sqlite> .output feature.csv
sqlite> select * from feature
```
# 欠損値の補間について
## order_of_finish
欠損値を含んでいた行のindexをNan_order_of_finis.csvに記録してある。
具体的な補間方法
欠損値を含むレコードと同じrace_idのものを抜き出す。
- 欠損値が1つの場合
    - 同じrace_idの中身を見比べて足りないものをつける

- 欠損値が2つ以上の場合
    - ランダムに補間だが、データを見てみると最後が欠損値になっているところしか確認できなかったので（全ての欠損値を確認したわけではない),埋められている順位に1足したもので補間している
## odd
全てのレコードの中央値で補間した。


# 文字の特徴量を数値に変化
そのまま、変化

# 特徴量を正規化
重回帰分析だと、特徴量のスケールの違いが寄与率の大きさに影響すると考えられるので、全ての特徴量を正規化、平均0分散1

# 重回帰分析
statsmodelsをそのまま使用
