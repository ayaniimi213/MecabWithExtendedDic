辞書作成環境を構築．
以下は，AWS EC2上に環境を構築する例です．

# 0. instance立ち上げ

EC2上にinstanceを立ち上げる。
とりあえず、お試しでt2.microでも良いかも。

AWS t2.large
OS: ubuntu
ストレージ: 8GB

t2.microでもよいですが，mecab-ipadic-NEologdを利用する場合，メモリ不足になる可能性があります．


# 1. パッケージ更新

```bash
sudo apt-get update && sudo apt-get -y upgrade && sudo apt-get -y dist-upgrade && sudo apt-get -y autoremove && sudo apt-get -y autoclean
```

# Ubuntu を最新版にアップグレード
比較的新しいバージョンからなら

```bash
sudo do-release-upgrade
```

# 2. swap設定

EC2instanceがt2.microのときなど、メモリが少ない環境の場合，swap領域を設定する

```bash
$ sudo dd if=/dev/zero of=/swap.img bs=1M count=2048
$ sudo chmod 600 /swap.img
$ sudo mkswap /swap.img
$ sudo bash -c 'echo "/swap.img swap swap defaults 0 0" >> /etc/fstab'
```

swapを有効にする

```bash
$ sudo swapon -a
```
