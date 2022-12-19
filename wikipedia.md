MeCabにwikipediaのタイトルリストを追加する手順

MeCabにwikipediaのタイトルリストを追加する手順のまとめです。
公開されているツールでははてなキーワードのタイトルから辞書を作ることもできるが，そちらはnecab-ipadic-NEologdで辞書に取り込んでいるので，こちらではwikipediaのタイトルのみを辞書についかする方法をまとめる．

# 0. 必要なパッケージのインストール

```bash
sudo apt install php-common php-dev php-cli php-pear php-mbstring
```

# 1. mecab
```bash
sudo apt install mecab
sudo apt install libmecab-dev
sudo apt install mecab-ipadic-utf8
```

# 2. wikipediaのタイトルリストからの辞書
```bash
git clone https://github.com/miraoto/php.mod-mecab-dic.git
cd php.mod-mecab-dic

mkdir mod-mecab-dic/tmp
cd mod-mecab-dic/tmp
wget https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-all-titles-in-ns0.gz
```

wikipediaのタイトルリストの公開URLと，mecab-dict-indexコマンドの場所を修正
```bash
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
>         $command .= '/var/lib/mecab/dic/ipadic/ -u ';
```

```bash
php ./mod-mecab-dic/bootstrap.php wikipedia
cd mod-mecab-dic/tmp
mv mecab-dic.dic mecab-wikipedia-dic.dic
sudo mkdir /usr/local/lib/mecab
sudo mkdir /usr/local/lib/mecab/dic
sudo cp mecab-wikipedia-dic.dic /usr/local/lib/mecab/dic
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

Androidが今回指定した辞書から持ってきていることが確認できる


# 4. 普段使う辞書の設定
普段使うなら，毎回-uを設定しなくてもよいように/etc/mecabrcを編集する
``` /etc/mecabrc
userdic = /usr/local/lib/mecab/dic/mecab-wikipedia-dic.dic
```
