MeCabに辞書を追加する

# このドキュメントは
[FUN Part3 Advent Calendar 2022 - Adventar](https://adventar.org/calendars/7655)の21日目の記事です。


# とりま、3行でまとめると
- 形態素解析エンジンMeCabに辞書を追加する方法をまとめました。
- mecab-ipadic-NEologdによるシステム辞書の変更の方法
- wikipediaのタイトルリストからユーザ辞書を追加の方法

# 辞書追加の方法

- [辞書作成環境](https://github.com/ayaniimi213/MecabWithExtendedDic/blob/main/environment.md)
 - このドキュメントでは，AWS EC2上のubuntu 22.04 LTSにて行いました。
 - 他の環境でも実行可能ですが，環境に合わせて，適宜読みかえてください。

- [mecab-ipadic-NEologdをインストールする手順](https://github.com/ayaniimi213/MecabWithExtendedDic/blob/main/neologd.md)
 - システム辞書として、mecab-ipadic-NEologdに切り替えて使うための手順です。

- [wikipediaのタイトルリストを追加する手順](https://github.com/ayaniimi213/MecabWithExtendedDic/blob/main/wikipedia.md)
  - ユーザ辞書として、wikipediaのタイトルリストから作成した辞書を使うための手順です。

# 書くきっかけ
形態素解析を行うときに，思ったように形態素に分割できないときがあります。その時、すぐに思いつくのが辞書を追加する方法です。
形態素解析エンジンMeCabでは、システム辞書を変更したり，ユーザ辞書を追加することができます。

ぱっと思い付くのは，wikipeidaやはてなキーワードから辞書を作成する方法です。だれでも思い付くので，ネット上で辞書追加の方法の記事が公開されています。
また、Web上の多数の言語資源からシステム辞書を作成するmecab-ipadic-NEologdも有名です。

ただ、どちらもちょっと古い方法なので，ネット上で情報が探しにくくなってきました。

私もまとめた記事を書きましたが，すでに古くなっています。
（記事を書いたことも忘れていました。）
[形態素解析ツールの辞書を追加する - Qiita](https://qiita.com/ayaniimi213/items/94bfa86ec73d7625aef4)

今回，アドベントカレンダーきっかけで，ドキュメントをGitHub上に公開してしまえば，内容が古くなっても誰かが更新してくれるかもと思い、ドキュメントにまとめました。
（Qiitaは自分で更新しないといけないですが、GitHubなら誰かが引きついでくれるかも！！）

# ところで
ところで、辞書追加だけでは、形態素分割の問題を解決できないことが多いです。
むしろ、辞書追加は問題解決のスタートラインで、これ以降の工夫が研究として面白いところです。

このドキュメントが研究のよいきっかけになれば幸いです。
