---
layout: post
title:  "Lightning Network の Paper を読む - 3.1.2"
categories: blog
---

今日は[3.1.2](https://github.com/goking/lightning-network-paper-ja/blob/master/lightning-network-paper-ja.md#312-spending-from-an-unsigned-transaction) です。

### 解説

署名していないFunding Transactionをどうやって使うのか気になるところ。

ここで登場するのが、`SIGHASH_NOINPUT`トランザクション。`SIGHASH_NOINPUT`はSegwitで使えるようになるっぽいが、[BIP-143](https://github.com/bitcoin/bips/blob/master/bip-0143.mediawiki)を見ても、sighash typeとして明確に定義されているわけではないようで、実際にどう使うのかはこの節だけでは正直わからない（いろいろと柔軟性が増すようではあるが…）。また、詳細に書いているという付録Aも、ぱっと見はその効果を説明しているだけで、具体的な使い方を書いている訳ではないようだ。

しかしながら、必要性は分かった。
ようは文中にあった*契約書のジレンマ*を回避するためなのだ。

`SIGHASH_NOINPUT`がない場合、チャネルを閉じようとして、最終的に親トランザクションに署名するとそのトランザクションIDが変わってしまう。そうすると、子は親のトランザクションIDを参照しているので、関連する子孫トランザクションの全ての署名をまたやり直さないといけないのである。

最初から親に署名をしておけばそんな面倒なことは発生しないのだが、しかし、親に署名をして相手と交換しまうと、今度は前節で述べた[人質シナリオ](http://www.scabla.net/blog/2016/06/28/lightning-network-paper-reading-3-1-1.html)が発生してしまう。出資トランザクションという契約書を作るために、人質問題に巻き込まれては元も子もない。

そこで、`SIGHASH_NOINPUT`を使うことで、入力への署名有無がトランザクションIDの値に影響しないようにできる（はず）。そうすれば署名前後で親（出資トランザクション）のトランザクションIDは変わらないため、ステップ7で親をブロードキャストする前に、子のトランザクションの署名をやりなおすなんてことはせずに済むというわけだ。

### 脱線

節の最後の文章の意味がちょっと取れていない。

```
Further, if one party fails during Step 6, the parent can either be spent to become the parent transaction or the inputs to the parent transaction can be double-spent (so that this entire transaction path is invalidated).
```

二重使用云々はわかるのだが、ここまでの内容だけだと裏切られたあとに出資トランザクションをそのまま使うってできない気がする。相手の署名がないので、このままでは使うに使えないよね。

まあ、この後を読めば分かるのだろう。