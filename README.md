# wow

# 20240328
IDEAの文献を読んでいます。
シュナイアーの言うことによると、ブロック暗号で最強のようだ。
複数の異なる群演算を組み合わせることでS-boxなしで強力な暗号ができるという。
AESの計算は４ステップに分解できるが、それしかないので暗記も容易である。
今はその意味を説明できるように仕込んでいる。

目指せオリジナル暗号！ｗ

# 20240327
とりあえず自分の作った暗号に似ているものがないか探してみた。
SPNみたいにS-box変換と置換を交互に繰り返すタイプと、DESみたいに暗号化関数が入れ子になっているもの、
またはIDEAみたいに複数の代数演算が混ざっているものなど色々あって面白かった。
自分の暗号はあれこれ取り入れて日本社会のようになってしまった。

SPNはS-boxと置換をしていることから明らかだけど、置換群を固定しないという点がより複雑だといえる。
Feistel構造はAESのMixColumnの代わりに、平文ブロックの半分を残りの半分に排他的論理和をとるという事で使っている。
また、IDEAなどのリー・マッシー型のように２つ以上の代数系演算を取り入れることで暗号解析が複雑になるようにしている。

まあ、これだけで安全だといえないところが悲しい所ではある。
勉強しよう。

-------

何も考えずに作ったのでこれからなにか考えます。

S-box以外は全部根拠不明です。
MDS行列を使ってますがCauchy行列でもいいようです。

# 設計指針
S-box:非線形置換
$x^3+1 \pmod {257}$  

最大距離分離符号：Vandermonde Matrix over GF(256)
或いは、コーシー行列を使ってmix_columnを実現する。

expand-key:2つの置換の共役を取り、鍵自身にGF(2)上の演算をして拡張鍵を生成する。
2つの置換が秘密鍵から生成される秘密置換である。
$\tau^{i+1}=\pi^{i-1}\tau^{i}\pi^{i-1}、　key[i]\oplus=key[\tau[i]]$

addkey:256を法とする10進加法
$m[i]=(m[i]+key[i]) \pmod {256}$
