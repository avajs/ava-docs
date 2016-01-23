---
**訳注**

これは[contributing.md](https://github.com/sindresorhus/ava/blob/master/contributing.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/sindresorhus/ava/compare/b76f3d513a41edd876f70fc3e61d7d968cd531a6...master#diff-cb1a0a79c5c751cd6b2568e604d23237ff8eb85f)になります(このリンクをクリックして、`contributing.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
---

# AVAにコントリビュート

✨ AVAにコントリビュートしてくれてありがとう! ✨


翻訳: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/contributing.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/contributing.md)

## どうやってコントリビュートできますか？

## ドキュメントの改善

AVAのユーザーとして、あなたはドキュメントを改善して私たちを手助けするのに完璧な候補です。タイポの修正、エラーの修正、より良い説明、より多くの例、など。改善できることについてissueを作成してください。[私たちのドキュメントを翻訳するのを手伝ってください。](https://github.com/sindresorhus/ava-docs)何でもいいです。このドキュメントの改善でも。

# issueの改善

いくつかのissueは、再現できない、もしくは正しくないもので、ドキュメントの欠落によって作成されています。それらを解決しやすくするために手伝ってください。バグの修正や機能追加に費やせるはずのたくさんの時間が、issueの処理に対してかかります。

### issueでフィードバックをください

私たちはいつもissueトラッカーでより多くの意見や議論を求めています。それはAVAの将来の方向性に影響を与える良い機会です。

### チャットに参加してください

[チャット](https://gitter.im/sindresorhus/ava)があります。参加して、話しかけて、みんなを手助けしてください。

### issueを作成

- issueトラッカーはissueのためのものです。サポートが必要なら[chat](https://gitter.im/sindresorhus/ava)か[Stack Overflow](https://stackoverflow.com/questions/tagged/ava)を利用してください。
- issueを作成する前にissueトラッカーで検索してください。
- 最新バージョンのAVAを使用しているか確認してください。
- 明確で叙述的な見出しをつけてください。
- 可能な限りの情報を含めてください: issueを再現するステップ、エラーメッセージ、Node.jsのバージョン、OS、など。
- issueにより多くの時間をかけてください。私たちはより多く費やします。
- [最善のissueの報告はテストが失敗することを証明することです。](https://twitter.com/sindresorhus/status/579306280495357953)

### プルリクエストの提出

- 不要な作業をしてしまわないように、小さくない変更は最初にissueで議論するのが一番良いことが多いです。
- 壮大なタスクであれば、可能な限りフィードバックを早く得るためにコミュニティに作業を見えるようにするべきです。アイディアを実証するのに必要最小限のことが完了したらすぐプルリクエストを出してください。この段階で完璧である必要も100%の状態である必要もありません。[WIP]プレフィックスを見出しに追加して、これからまだ必要なことを記述してください。そうすることで、詳細について小うるさくしなくても、また、すでにあなたが分かっている改善点を指摘しなくてもいいとレビューアーに知らせることになります。
- 新たな機能はテストとドキュメントを含めるべきです。
- 関連性のない変更を含まないでください。
- プルリクエストを送る前に`$ npm test`を実行することでlintとテストを行ってください。
- masterからではなく[トピックブランチ](https://github.com/dchelimsky/rspec/wiki/Topic-Branches)からプルリクエストを作成してください。
- プルリクエストとコミットには明確で叙述的な見出しを使用してください。
- なぜプルリクエストを確認すべきなのか納得のいく理由を書いてください。私たちを納得させるのはあなたの役目です。"なぜ"それが必要なのかと、使用例を見せてください。
- あなたのプルリクエストに対して変更を求めることもあると思います。その場合新たに他のプルリクエストを作る必要は決してありません。[すでに存在するプルリクエストを更新してください。](https://github.com/RichardLitt/docs/blob/master/amending-a-commit-guide.md)
