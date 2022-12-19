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


mecab-dict-indexが上記パッケージに入っていないようなので，いったんソースコードからコンパイル

ソースコードの場所は、公式ページ(MeCab: Yet Another Part-of-Speech and Morphological Analyzer https://taku910.github.io/mecab/#download)から

```bash
wget https://drive.google.com/uc?export=download&id=0B4y35FiV1wh7cENtOXlicTFaRUE
tar zxvf ~//mecab-0.996.tar.gz
cd ~//mecab-0.996
./configure
make
```
うまくgoogledriveからダウンロードできなければ，ブラウザで団ロードした後、AWS EC2にアップロード。


# 2. wikipediaのタイトルリストからの辞書
```bash
git clone https://github.com/miraoto/php.mod-mecab-dic.git
cd php.mod-mecab-dic

mkdir mod-mecab-dic/tmp
cd mod-mecab-dic/tmp
wget https://dumps.wikimedia.org/jawiki/latest/jawiki-latest-all-titles-in-ns0.gz

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
>         $command  = '/home/ubuntu/mecab-0.996/src/mecab-dict-index -d ';
>         $command .= '/var/lib/mecab/dic/ipadic/ -u ';

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
