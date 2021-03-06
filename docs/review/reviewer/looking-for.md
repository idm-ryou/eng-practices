# コードレビューで何を期待するべきか



注意: 以下の各ポイントについて考えるときは、必ず[コードレビューの基準](standard.md)を考慮するようにしてください。

## 設計

レビュー中に取り上げるべき最も重要なことは、CL の全体的な設計をどのようにするかということです。CL 中のさまざまなコードのインタラクションは意味のあるものですか？ その変更はコードベースかライブラリ、どちらに帰属するべきものでしょうか？ システムの残りの部分とよく統合されていますか？ その機能を追加するのは本当に今が適切なタイミングですか？

## 機能性

その CL は開発者が意図した通りのことを行っているでしょうか？ 開発者は、そのコードのユーザーにとって、どんなよいことがあると考えていますか？ ここで言う「ユーザー」とは、通常エンドユーザー (もし変更がエンドユーザーに影響する場合) と開発者 (将来このコードを「使う」必要があるユーザーのこと) の双方のことを指します。

ほとんどの場合、私たちレビュアは、開発者がコードレビューを受ける前に CL が正しく動作していることを十分よくテストしていると期待します。しかし、あなたはレビュアとしてさらに、エッジケースについて考え、平行性の問題を探し、自分がユーザーとなったときのことを想像し、コードを読むだけでわかるようなバグが存在しないことを確認しなければなりません。

必要に応じてレビュア自身で CL の動作を検証**してもよい**ですが、レビュアによる CL の動作チェックが最も重要なのは、**UI の変更**のような、ユーザーに直接影響を与えるような変更が含まれている場合です。コードを読むだけでは、一部の変更がユーザーにどのくらい大きな影響を与えるのかを理解するのは困難です。そのような変更に対しては、もし CL にパッチを当てて自分で試してみるのが大変な場合は、開発者に機能性を確認するためのデモを提供するように依頼することが許されます。

他にも、コードレビュー中に機能性について考えるのが特に重要となることがあります。CL の中で行われているある種の **並列プログラミング** が、理論的にデッドロックやレースコンディションを引き起こす可能性があるような場合です。 ある種の問題は、単にコードを実行しただけでは発見するのが非常に困難です。通常、誰か (開発者とレビュアの双方) がそのような平行性の問題が入り込んでいないことを、コード全体を通して注意深く考え抜く必要があります。(これはまた、レースコンディションやデッドロックが起こりうる並行性モデルを使用しない方が良い理由でもあります。そのような並行性モデルを使用すると、コードレビューやコードの理解が非常に複雑になってしまいます。)

## 複雑さ

CL は必要以上に複雑ではないでしょうか？ これは CL のあらゆるレベルで確認してください。1行1行、どの行も複雑ではないですか？ 「複雑すぎる」とは、通常**「コードの読み手が素早く理解できない」**という意味です。また、**「開発者がこのコードを呼び出したり修正しようとしたときにバグが入り込みやすい」**と言うこともできます。

典型的な種類の複雑さは、開発者が必要以上にコードを一般化してしまったり、現在はシステムに必要ではない機能を追加してしまうといった、**オーバーエンジニアリング**です。レビュアは特に、オーバーエンジニアリングに警戒しなければなりません。将来解決する必要がある**かもしれない**と予想される問題ではなく、**今現在**解決する必要があると知っている問題を解決するよう、開発者を促してください。将来の問題は、問題が現れたときに解決されるべきです。問題の具体的な形や要件を確認することができるのは、予想している頭の中ではなく、物理的な宇宙の中においてなのです。

## テスト

変更に対して適切な、ユニットテスト、インテグレーションテスト、または E2E テストを求めてください。一般に、[緊急事態](../emergencies.md)に扱うコードでない限りは、テストは必ずプロダクションコードと同じ CL に含めなければなりません。

CL に含めるテストは、正しく、意味があり、役に立つものであるようにしてください。テストはそのテスト自体をテストするわけではなく、私たちがテスト自体に対するテストを書くこともほとんどありません。そのため、テストが有効なものであることは、人間が保証しなければなりません。

そのテストは、コードが壊れたときに実際に失敗するものになっていますか？ そのコードが変化を受けたとき、フォールス・ポジティブを示すようになっていないでしょうか？ 各テストはシンプルで役に立つアサーションを行っていますか？ テストは異なるテストモジュール間で適切に分離されていますか？

テストもメンテナンスが必要だということを覚えておいてください。単にメインバイナリの一部でないからといって、テストの中に含まれる複雑さを受け入れてはなりません。

## 命名

開発者はあらゆるものによい名前を選んでいますか？ よい名前とは、その要素が何であるか、または何をするものであるかを、完全に伝えられ、かつ読むのが難しくなるほど長すぎない、適切な長さの名前のことです。

## コメント

開発者は、理解できる英語で明確なコメントを書いていますか？ すべてのコメントは実際に必要なものですか？ 通常コメントが役に立つのは、あるコードが存在するのが**なぜかという理由 (why) を説明している**ような場合であり、あるコードが**何をしているのか (what)** を説明するようなものであってはいけません。もしコードがそれ自体で何をしているのかわかるほどクリアでないときは、コードをもっとシンプルにしなければなりません。いくつか例外はあります (たとえば、正規表現や複雑なアルゴリズムの場合、通常は何をしているのかを説明するコメントは大いに助けとなります) が、大部分のコメントは、決定の背後にある理由を説明するような、コード自体に含めるのが困難な情報のために書かなければなりません。

この CL の前に書かれていたコメントを読むことも助けとなることがあります。あるいは今回削除される TODO があるかもしれませんし、この CL が行おうとしている変更はするべきではないというアドバイスとなるコメントなどがあるかもしれません。

コメントは、クラス・モジュール・関数などの**ドキュメント**ではないことに注意してください。ドキュメントは、コードの目的を示したり、どのように使用するべきかを説明したり、使用したときどのように動作するかが書かれますが、コメントは違います。

## スタイル

Google には、主に使用されている主要な言語のすべてだけでなく、マイナーな言語のすべてにも[スタイルガイド](http://google.github.io/styleguide/)が存在します。CL が適切なスタイルガイドに従っていることを保証してください。

スタイルガイドに書かれていない点でスタイルの改善を行いたい場合は、コメントの前に "Nit:" と書いて、必須ではないがコードを改善できるかもしれないポイントを示していることを開発者に知らせてください。個人的なスタイルの好みだけに基づいて、提出された CL をブロックしてはなりません。

CL の作者は、大きなスタイルの変更を、その他の変更と同じ CL に含めるべきではありません。スタイルの変更とそれ以外の変更が同じ CL に含まれていると、CL の中で何が変更されたのかを確認することが難しくなってしまいますし、マージやロールバックの作業が複雑になってしまい、その他にも問題を引き起こしてしまいます。もし作者がファイル全体を再フォーマットしたいと思った時は、最初に再フォーマットを行う1つの CL を送り、その後に機能的な変更を別の CL として送るようにします。

## ドキュメント

CL の変更により、ユーザーのビルド方法、テストの方法、コードの使い方、リリースされるコードなどが変更される場合には、同時に関連するドキュメントが更新されているかどうかもチェックしてください。対象となるドキュメントは、README、g3doc のページ、あらゆる自動生成されるリファレンスドキュメントなどです。もし CL がコードを削除したり廃止にしたりするものであれば、同時にドキュメントも削除するべきではないか考えてください。もしドキュメントが存在しなければ、作成するように依頼してください。

## すべての行 {#every_line}

レビュアは、レビューに割り当てられたコードの**すべての**行を読んでください。データファイル、自動生成コード、巨大なデータ構造などの一部のファイルは、何回か目を通して確認するだけでも構いませんが、人間が書いたクラス、関数、コードブロックなどは、ただ目を通して中に書かれているものが問題ないと判断してはいけません。コードのある部分は、明らかに他の部分よりも注意深い精査が必要になり、それはレビュアとしてのあなたの判断で行わなければなりません。少なくともコードが何を行っているのかを自分で**理解**しなければなりません。

もしコードを読むのが難しくて素早くレビューできない場合には、そのことを開発者に知らせて、レビューしようとする前に開発者がコードをもっと明確にするのを待ちましょう。Google では、優れたソフトウェアエンジニアを雇っており、あなたもその一員です。もしあなたがコードを理解できないのなら、他の開発者もコードを理解できない可能性が非常に高いです。そのため、開発者にコードをもっと明確にするように頼むことは、将来このコードを理解しようとする未来の開発者も同時に助けることになります。

セキュリティ、並行性、アクセシビリティ、国際化などの特に複雑な問題に対しては、もしあなたがコードを理解できたとしても、レビューの一部について自分が対応するのはふさわしくないと感じるなら、その部分にふさわしいレビュアを CL のレビュアに加えるようにしてください。

## コンテキスト

CL を広いコンテキストの上で捉えることは役に立つことが多いです。通常、コードレビューツールでは、変更された部分の周辺の数行しか表示されません。変更が実際に意味のあるものかどうか確認するためには、ファイル全体を読んで確認する必要がある場合もあります。たとえば、新しいコードが4行だけ追加された変更があった時、ファイル全体を読んだ結果、そのコードは50行あるメソッドに追加されており、現在の状態では実際には小さいメソッドに分割する必要があることを発見するかもしれません。

また、CL を全体としてのシステムのコンテキストにおいて考えてみることも役に立ちます。この CL はシステムのコードの健全性を向上させているでしょうか？ それとも、システム全体をより複雑にしたり、システムをよりテストされていない状態に劣化させたりしているでしょうか？ **システムのコードの健全性を劣化させるような CL は、決して受け入れてはなりません。**ほとんどのシステムは、小さな変更の積み重ねにより複雑になって行くものです。そのため、たとえほんの小さな複雑性であっても、新しい変更には入り込まないように防ぐことが重要です。

## よいこと {#good_things}

CL に何か優れたところがあったときには、そのことを開発者に伝えましょう。特に、あなたのコメントに素晴らしい対応をした場合にはなおさらです。コードレビューでは間違いにだけ注意を向けてしまいがちですが、同時に、よいプラクティスに対して、励ましや感謝を与える場でもなければなりません。メンタリングの観点からも、開発者が間違いを犯したことを指摘することよりも、開発者が正しいことをした時に、よいことをしたと声をかけてあげることの方が、ずっと大きな価値のあることです。

## まとめ

コードレビュー中には、以下のことを保証しなければなりません。

-   コードがよく設計されている。
-   機能がコードの利用者にとってもよいものとなっている。
-   すべての UI の変更は、意味があってよいものに見える。
-   並列プログラミングでは、すべての処理を安全に完了させている。
-   コードが必要以上に複雑ではない。
-   開発者が、現時点で必要かどうかは分からないが、将来必要になる**かもしれない**ものを実装していない。
-   コードに適切なユニットテストが存在する。
-   テストがよく設計されている。
-   開発者がすべてのものに明確な名前を使用している。
-   コメントが明確で役に立つものであり、大部分は**コードの動作 (what)** ではなく**そのコードである理由 (why)** を説明するものとなっている。
-   コードに適切なドキュメントが書かれている (一般には g3doc で)。
-   コードが Google のスタイルガイドに従っている。

レビュアは、レビューを依頼されたコードの**すべての行**をレビューし、**コンテキスト**をしっかり捉えて、**コードの健全性が改善されている**ことを確認し、そして、開発者が行った**よいこと**を褒めてください。

Next: [レビューで CL をナビゲートする](navigate.md)
