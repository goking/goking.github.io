---
layout: post
title:  "Lightning Network の Paper を読む - 3.1.1"
categories: blog
---

マイクロペイメントチャネルとかが気になるこの頃。

本命と言われながら、分かりそうでわからない Lightning Network を基礎から理解すべく、Paperを少しづつ[訳して](https://github.com/goking/lightning-network-paper-ja/blob/master/lightning-network-paper-ja.md
)みる。


1章と2章は概要なので飛ばして、3章あたりから進めたいと思う。

まずは [3.1.1](https://github.com/goking/lightning-network-paper-ja/blob/master/lightning-network-paper-ja.md#311-creating-an-unsigned-funding-transaction) あたりから。

### 解説

「署名していないFunding Transactionを作る」のがこの節の主題。Funding Transactionは「出資トランザクション」と訳してみたが、具体的なトランザクションの絵がないので実際の中身はどうなるのかは分からない。想像で書くと以下のようになると思われる。

* インプット: 双方のUTXO?
* アウトプット: 双方の公開鍵による2-of-2マルチシグスクリプト(P2SH)

こう仮定すると、Aliceが0.5BTC、Bobが0.5BTCをそれぞれのUTXOからインプットに出資したのであれば、アウトプットはそれを合計した1.0BTCとなる。

ここで、出資トランザクションのインプットに署名して交換してしまうと、協力関係が破綻した場合に事故が起こってしまう可能性がある。つまり、出資トランザクションをブロードキャストされると、相手から必要な署名が集められないので双方とも出資金を遣えなくなってしまう。

なお、文中の人質云々の部分は直訳気味なので分かりにくいが、恐らく出資額に差がある場合に問題が発生するということが言いたいのだと思われる。

例えばAliceが0.5BTC、Bobが0.1BTC出資していたとして、Bobが「このアドレスに身代金を0.3BTC送らないと出資トランザクションをブロードキャストしちゃうぞ！！」と脅す。Aliceは0.5BTC失うよりは0.3BTCで済んだ方がマシだし、Bobは身代金0.3BTCが入ってくる可能性がある。これが「人質シナリオ(hostage scenarios)なのだろう。

鍵の交換のところは今後の手順で必要なのだろうけど、訳に自信がない。

### 脱線

* 入り口なのでまだのP2SHの話だけです。
* インプットは本当に**双方のUTXO**なのか？その場合実際の署名などはどうやって行うのか気になる。
* "exchange one key to use to sign with later" の "one key" を「一方の鍵」と訳したが意味が通らない。もしくは一つの鍵なのか、誰かの鍵なのか？交換するので、公開鍵なんだろうけど。
