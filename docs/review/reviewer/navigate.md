# レビューで CL をナビゲートする



## まとめ

ここまでで[コードレビューで期待するべきこと](looking-for.md)が理解できたと思います。それでは、複数のファイルに分散したレビューを最も効率的に行うには、どのような方法で行えばよいのでしょうか？ 要点は以下のとおりです。

1.  その変更は全体として意味のあるものとなっているでしょうか？ よい [CL の説明](../developer/cl-descriptions.md)が書かれているでしょうか？
2.  変更の最も主要な部分を最初に読んでください。CL 全体としてよく設計されていますか？
3.  CL の残りの部分を読み、論理的に適切な順序で書かれているか確認しましょう。

## ステップ1: 変更に対して広い視野を持つ {#step_one}

まず、[CL の説明](../developer/cl-descriptions.md)をよく読み、全体として CL が何を行っているのかを理解します。この変更は本当に意味のあるものでしょうか？ そもそもこの変更が行われるべきものでないのなら、すみやかにその変更が行われるべきではない理由を説明する返事を書いてください。こうした変更のリジェクトを行う場合には、開発者が代わりに行うべきことも提案するのがよい考えです。

たとえば、次のように返事を書きます。「このコンポーネントに対するあなたの仕事は素晴らしいと思います。ありがとう！ ですが、あなたがこの CL で変更を行っている FooWidget システムは削除する予定になっているため、今は新しい変更を加えたくないと考えています。代わりに新しい BarWidget クラスに対してリファクタリングを行うのはいかがでしょうか？」

レビュアは現在の CL をリジェクトして別の代替案を提案していますが、それを**丁寧に**行っていることに注意してください。このような丁寧さはとても大切なことです。たとえ相手に賛成しないときでも、私たちはお互いに開発者として尊敬の気持ちを表したいと考えているからです。

望まない変更を行う CL が少数ではなく、もっと頻繁に送られて来てしまう場合、CL を書き換える前にもっとコミュニケーションが密に行えるように、あなたのチームの開発プロセスや、外部のコントリビュータの投稿のプロセスについて再編成することを検討しなければなりません。無駄になってしまったり大幅な書き直しが必要になる大量の作業を行わせてしまうより前に、「No」と伝えた方ががよいからです。

## ステップ2: CL のメインとなる部分を確認する {#step_two}

この CL の「メイン」となる部分を構成するファイル (複数の場合もあります) を探しましょう。多くの場合、ロジックの変更数が最も多い1つのファイルがメインとなり、CL の大部分の変更が含まれます。このメイン部分が、CL のすべての小さな変更にコンテキストを与える助けとなります。それにより、コードレビューが素早く行えるようになります。もし CL が大きすぎて、どの部分がメイン部分なのかが理解できない場合には、開発者に最初にどの部分を見るべきかを尋ねるか、[CL を複数の部分に分割する](../developer/small-cls.md)ようにお願いしましょう。

もしあなたが CL のメイン部分に設計の問題があると考えた場合には、たとえ残りの部分をすぐにレビューする時間がなかったとしても、すみやかにコメントを送らなければなりません。実際、設計の問題が十分明らかな場合には、レビュー対象の他のコードの多くが無くなり、問題とはならなくなるため、CL の残りの部分のレビューは時間の無駄となる可能性もあります。

こうした主要な設計に関するコメントをすみやかに送るのが重要である理由としては、以下のような主に2つの理由があります。

-   通常、開発者は CL をメールで送信した後、レビューを待っている間に、その CL を元にすぐに新しい仕事に取り掛かります。もしあなたがレビューしている CL に大きな設計上の問題があった場合、後の CL にも作業のやり直しが必要になることがあります。問題のある設計の上に多くの追加作業を行ってしまう前に、開発者を引き止めた方が良いです。
-   主要な設計の変更には小さな変更よりも長い時間がかかります。ほとんどすべての開発者には締め切りがあります。締め切りを守れるように助け、なおコードベースのコードの品質を高く保てるようにするためには、開発者に CL の再作業にできるだけ早く取り掛かってもらう必要があります。

## ステップ3: CL の残りの部分が論理的な順序になっているか最後まで見る {#step_three}

全体として CL に主要な設計の問題がないことが確認できたら、すべてのファイルを読み通して、変更の論理的な順序を理解するように努めましょう。同時に、レビューを忘れているファイルがないかも確認しましょう。通常、主要なファイルを確認した後であれば、コードレビューツールが表示した通りの順番にファイルを読み通すのが一番簡単でしょう。テストを最初に読むのが役に立つこともあります。それにより、開発者が変更により行おうとしている考えを理解できることがあるからです。

Next: [コードレビューのスピード](speed.md)
