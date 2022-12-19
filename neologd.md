mecab-ipadic-NEologdをインストールする手順

mecab-ipadic-NEologd は、多数のWeb上の言語資源から得た新語を追加することでカスタマイズした MeCab 用のシステム辞書です。

# 0. 必要なパッケージのインストール

```bash
sudo apt install git make curl xz-utils file
```

# 1. mecab
```bash
sudo apt install mecab
sudo apt install libmecab-dev
sudo apt install mecab-ipadic-utf8
```

# 2. mecab-neologd

```bash
git clone https://github.com/neologd/mecab-ipadic-neologd.git
cd mecab-ipadic-neologd
sudo bin/install-mecab-ipadic-neologd -n -a
```

install-mecab-ipadic-neologdのオプションは公式ページ(https://github.com/neologd/mecab-ipadic-neologd/blob/master/README.ja.md)を参考に．
上記の例は，なにも考えずに全部入りmecab-ipadic-NEologdをインストールする場合.

# 3. テスト

標準辞書の場合
```bash
echo "8月3日に放送された「中居正広の金曜日のスマイルたちへ」(TBS系)で、1日たった5分でぽっこりおなかを解消するというダイエット方法を紹介。キンタロー。のダイエットにも密着。" | mecab
```

mecab-ipadic-NEologdの場合
```bash
echo "8月3日に放送された「中居正広の金曜日のスマイルたちへ」(TBS系)で、1日たった5分でぽっこりおなかを解消するというダイエット方法を紹介。キンタロー。のダイエットにも密着。" | mecab -d /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd
```

効果については，公式ページ(https://github.com/neologd/mecab-ipadic-neologd/blob/master/README.ja.md)を参考に．

# 4. 普段使う辞書の設定
普段使うなら，毎回-dを設定しなくてもよいように，/etc/mecabrc を編集する

``` /etc/mecabrc
dicdir = /usr/lib/x86_64-linux-gnu/mecab/dic/mecab-ipadic-neologd
```

# 参考
ubuntu 18.04 に mecab をインストール - Qiita https://qiita.com/ekzemplaro/items/c98c7f6698f130b55d53
mecab-ipadic-NEologdインストール[Ubuntu 16.04 LTS] - Qiita https://qiita.com/spiderx_jp/items/7f8cbfd762c9abab660b
mecab-ipadic-neologd/README.ja.md at master · neologd/mecab-ipadic-neologd https://github.com/neologd/mecab-ipadic-neologd/blob/master/README.ja.md
