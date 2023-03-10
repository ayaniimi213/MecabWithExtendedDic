MeCabにwikipediaのタイトルリストを追加する手順

MeCabにwikipediaのタイトルリストを追加する手順のまとめです。

[ツール](https://github.com/miraoto/php.mod-mecab-dic)を公開されている方がいらっしゃるので，それを使う方法です。
公開されているツールでははてなキーワードのタイトルから辞書を作ることもできますが，そちらはnecab-ipadic-NEologdで辞書に取り込んでいるので，こちらではwikipediaのタイトルのみを辞書に追加する方法をまとめます。

# 0. 必要なパッケージのインストール

ツールに必要なパッケージのインストール
```bash
sudo apt install php-common php-dev php-cli php-pear php-mbstring
```

# 1. mecab
まだインストールしていなければ
```bash
sudo apt install mecab
sudo apt install libmecab-dev
sudo apt install mecab-ipadic-utf8
```

# 2. wikipediaのタイトルリストからの辞書
```bash
git clone https://github.com/miraoto/php.mod-mecab-dic.git
cd php.mod-mecab-dic
```

作業ディレクトリの作成
```bash
mkdir mod-mecab-dic/tmp
```

wikipediaのタイトルリストの公開URLと，mecab-dict-indexコマンドの場所，ipadic辞書の場所を修正
```bash
cd mod-mecab-dic
cp models/wikipedia.php models/wikipedia.php.orig
vi models/wikipedia.php
diff models/wikipedia.php.orig models/wikipedia.php
17c17
< define('TARGET_URL', 'http://dumps.wikimedia.org/jawiki/latest/jawiki-latest-all-titles-in-ns0.gz');
---
> define('TARGET_URL', 'https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-all-titles-in-ns0.gz');

cp libs/dictionary.php libs/dictionary.php.orig
vi libs/dictionary.php
diff libs/dictionary.php.orig libs/dictionary.php
24,25c24,25
<         $command  = '/usr/local/libexec/mecab/mecab-dict-index -d ';
<         $command .= '/usr/local/lib/mecab/dic/ipadic/ -u ';
---
>         $command  = '/usr/lib/mecab/mecab-dict-index -d ';
>         $command .= '/usr/share/mecab/dic/ipadic/ -u ';
```

```bash
php .bootstrap.php wikipedia
cd tmp
mv mecab-dic.dic mecab-wikipedia-dic.dic
sudo mkdir /usr/local/lib/mecab
sudo mkdir /usr/local/lib/mecab/dic
sudo cp mecab-wikipedia-dic.dic /usr/local/lib/mecab/dic
```

## 修正箇所の確認
上記の修正に必要な情報の確認方法

- wikipediaのタイトルリスト
今回は，http -> httpsの変更のみだった

- コマンド，辞書の場所
ubuntuならapt-fileを使うと便利.

apt-fileインストール

```bash
sudo apt install apt-file
sudo apt-file update
```

場所の確認
```bash
sudo apt-file search mecab-dict-index
sudo apt-file search pos-id.def
```


# 3. テスト

標準辞書の場合
```bash
echo Androidアプリケーション開発 | mecab
```
wikipedia辞書の場合
```bash
echo Androidアプリケーション開発 | mecab -u /usr/local/lib/mecab/dic/mecab-wikipedia-dic.dic
```

Androidが今回指定した辞書から持ってきていることが確認できる。
効果については，[公式ページ](http://log.miraoto.com/2012/11/703/)を参考に。


# 4. 普段使う辞書の設定
普段使うなら，毎回-uを設定しなくてもよいように/etc/mecabrcを編集する
``` /etc/mecabrc
userdic = /usr/local/lib/mecab/dic/mecab-wikipedia-dic.dic
```

# 参考
- mecabにwikipediaのタイトルリストを追加する | ミラボ http://log.miraoto.com/2012/11/703/
- mecabにはてなキーワードのタイトルリストを追加する | ミラボ http://log.miraoto.com/2012/11/705/
- miraoto/php.mod-mecab-dic: Frequent using module to make mecab dictionary. https://github.com/miraoto/php.mod-mecab-dic
- fukushimuのメモ帳 [mecab] mecab辞書にwikipediaのタイトルリストを追加 [wikipedia][はてなキーワード] http://fukushimu.blog.shinobi.jp/mecab/-mecab-%20mecab%E8%BE%9E%E6%9B%B8%E3%81%ABwikipedia%E3%81%AE%E3%82%BF%E3%82%A4%E3%83%88%E3%83%AB%E3%83%AA%E3%82%B9%E3%83%88%E3%82%92%E8%BF%BD%E5%8A%A0%20-wikipedia--%E3%81%AF%E3%81%A6%E3%81%AA%E3%82%AD%E3%83%BC%E3%83%AF%E3%83%BC%E3%83%89-
