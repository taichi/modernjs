この一か月分の学習成果を整理したリポジトリを作ったので、その成果についてまとめておく。

作ったサンプルプロジェクトだけを手軽に欲しければ、このリポジトリをcloneしてほしい。

* [taichi/js-boilerplate](https://github.com/taichi/js-boilerplate)
  * masterブランチには、ミニマムなJavaScript開発環境がサンプルコード付きで入っている
  * frontendブランチには、[React]/[Redux]/[webpack]なウェブアプリケーション用の開発環境が入っている
  * デフォルトブランチにしてあるelectronブランチには、frontendブランチの内容に加えて[Electron]でアプリケーションを開発するための環境が入っている

# はじめに

## 最近のJavaScriptについて

僕は仕事としてJavaScriptを書いている訳ではないけども、この半年くらいの間にちょっとしたツールならいくつか作った。どちらも便利なので是非使ってみてほしい。

* [ci-yarn-upgrade](https://github.com/taichi/ci-yarn-upgrade)
  * [Yarn]を使っているプロジェクトで依存ライブラリの更新があったら自動的にPullRequestを作るツール
* [vscode-textlint](https://github.com/taichi/vscode-textlint)
  * [textlint](https://github.com/textlint/textlint)をVS Codeで動かす拡張

そういうわけで、僕は最先端のJavaScriptを追いかけられている訳ではないけども、全然知らないってわけでもない。

今のJavaScriptは、色んなライブラリやツールが出たり消えたりしているから動きがあるように感じるだけで、やりたい事や、やれるようになった事はそれほど増えていない、と言うのが僕の考えだ。

ここに書いた内容は、過去二年から三年分くらいの動きをできるだけ拾ったので、量としてはかなりある。流石に三年あれば色々と変わるよね。

## JavaScriptにおける主な問題領域

これから、僕が最近学習したJavaScriptに関する知識を説明していくけども、話題にする領域を列挙しておく。

これがさっき書いた「やりたい事や、やれるようになった事」のリストだ。

* 言語を中心とした話題
  * 型システムの導入
  * 静的解析の発展
* テストに関する話題
* 高度なUIに関する話題
* ビルドに関する話題
  * 依存モジュールの管理
  * モジュールバンドル
* Electronに関する話題

## 対象読者について

この文書の対象読者として想定しているのは、十分に使える第一言語があるプログラマ。その言語はJavaやC#のようなコンパイルプロセスのある言語が望ましい。

加えて、最近のJavaScriptに興味があるけども、効率よく学習するために指針となる文書が欲しい人。

あまりコードが書けない人やJavaScriptに興味が無い人に対する配慮のある文章にはなっていない。

最先端のJavaScriptをバリバリ使いこなしている人は対象読者では無いけども、目を通して何かおかしな部分や誤解し易い表現があったら適宜Twitterやはてブで指摘してくれると嬉しい。

## 開発環境
僕はOSとしてWindows10を使っている。

今回の話にプラットフォーム依存性のある部分は少ないから、どんなOSを使っていても関係ないけど念のため。

Nodeは[nvm-windows](https://github.com/coreybutler/nvm-windows)でインストールするのがいいよ。
[nodist](https://github.com/marcelklehr/nodist)よりもトラブルが少ないので僕は気に入っている。
Macなら[nvm](https://github.com/creationix/nvm)を使えば良いと思う、知らんけど。

Nodeのインストールが終わったら、[Yarn](https://yarnpkg.com/)もインストールしておいてほしい。これが何なのか知らなくても後で説明するので問題ない。

エディタとしては[VS Code]を使っていることを前提に環境を作り込んだ。
ただし、[VS Code]にしか無いような拡張は使ってないので、他のエディタで同様の環境を再現するのは難しくないはずだ。

プロジェクト直下の`.vscode/extensions.json`ってファイルにVS Code拡張のIDを並べてある。
リポジトリを`git clone`したディレクトリをVS Codeで開いて、確認ダイアログを承認すれば必要な拡張は全部自動的にインストールされる。

アプリケーションを動作確認するためのブラウザとしては、Chromeを使っている。

### ハードウェアスペック
僕が使ってるマシンのスペックは以下の通り。

* Thinkpad X1 Carbon 2016年モデル
* CPU i7 6600U @ 2.6GHz
* メモリ 16GB
* ストレージ SAMSUNG NVMe SSD 950 PRO 512GB

少し高級かもしれないけど開発者用のマシンとしては普通だよね。

## 僕のバックグラウンドについて

僕のキャリアはSIerで受託のシステムをJavaでゴリゴリ作るソルジャーとして10年くらい過ごした所から始まっている。
だから、Javaに対する知識が多く、一番うまく使えるのはJavaに関連するサーバサイド技術だ。

今はそれなりに色んな言語を使ってプログラミングできるけども、結局は脳内でJavaによく似た謎のオレオレ言語をRubyなりPythonなりに変換して出力している。

そういうわけでJava以外の環境を学ぶ際には、どうしてもそのオレオレJavaに変換し易い環境で作業したいと考える傾向がある。

加えて、SIerでキャリアを積んで今もSIerで働いているので、受託のシステム開発や、パッケージ開発において当該技術をどのように使うかに強い関心がある。

つまり、僕は技術とその改善が収益にはほどんど結びつかない環境を前提に、新しい技術をどう適用していくのか考える傾向がある。

このエントリには、そういうバイアスがあるってことを理解してほしい。

# 言語の話

JavaScriptの実行環境は色々あるけども、ほとんどのブラウザで動くのはES5だ。
そもそもES5で出来る限り頑張るなら、ややこしいトランスパイラがどうのって話、つまり[Babel]とは無縁になれる。

JavaScriptには学ぶべき知識が多いのだから、まず[Babel]と付き合わないという選択肢は最初に考えよう。

僕には[Babel]と付き合わないって選択肢を取るのは無理だった。理由は単純で、もう `function` って沢山書きたくないから。みんなはどうかな？

付け加えるなら、Javaから来た僕はコンパイルって作業に対する忌避感がないのは事実だけどね。

## [Babel]はみんなの砂場

プログラミング言語のデザインは、ある程度孤独な作業だ。今までは少数の言語デザイナがある程度形になるまで丁寧に育ててからリリースしてきた。

ただ、少なくともJavaScriptに関しては、そういうやり方じゃ上手くいかないってことがECMAScript4の失敗で分かっている。

そこで、実験的な言語機能をプラグインという形でカジュアルに実装できる[Babel]だ。[新しい言語機能](https://github.com/tc39/proposals)をガシガシ使いながら議論して次のバージョンのJavaScriptに取り込む機能を決める。

ユーザはちょっと`.babelrc`を書くだけでクールな新機能を試せる。その機能が思った程よく無かったり、動作が不安定だったらサクっと使うのを止めればいいだけのことだ。

言語機能を自分の需要に応じて足したり引いたりするのは結構楽しいよ。

### [Babel]関連の理解すべきモジュール
[Babel]関連のモジュールは沢山あるけども、キチンと理解した方がよいものは意外と少ない。

前半の二つは環境をセットアップするためのモジュールで、残りの二つは他のツールと連携するためのモジュールだ。

#### [babel-preset-env](https://github.com/babel/babel-preset-env)
`babel-preset-env` は、[Babel]で変換したJavaScriptが動作する環境に併せて、利用する[Babel]プラグインを自動的に選択してくれる便利なモジュールだ。

JavaScriptは、ブラウザや[Node.js]などの実行環境がどんどん改善しているので、それに併せてコンパイル時に必要な[Babel]のプラグインが変化していく。
無駄なプラグインが有効化されたところでコンパイル時間が微妙に伸びるだけなので、あまり害は無いけどね。

ただ、意識しないと最新状態に追いつけないというのはあまり健康的でないだろう。

`babel-preset-env` を使うと、そういう億劫になり易い作業から開放される。定期的にモジュールをアップデートするだけで最新環境が付いてくるってわけだ。素晴らしい。

#### [babel-register](https://github.com/babel/babel/tree/master/packages/babel-register)
`babel-register` はNodeの `require` をフックして、[Babel]の処理を差しはさむハッキングモジュールだ。

プロダクションコードでモジュールをロードする時に使うのではなく、主にテストコードを実行する際に使う。

JavaScriptにおけるユニットテストコードの実行では、ファイルの変更検知からテストを実行するといったことがよく行われている。

この時テストコードをちょっと修正しただけで、プロジェクトのコードを全部コンパイルしてたら全く仕事にならないだろう。

そこで変更したファイルと、それに関係があるファイルだけコンパイルするプロセスを、完全に自動化するなら `require` なり `import` なりをフックすればいいのだ。

注意しておくと、テストコードがテスト対象コードを直接的に参照しないタイプのテスト、例えばE2Eテストみたいなものではあまり上手く動かないこともあるよ。

#### [babel-eslint](https://github.com/babel/babel-eslint)
`babel-eslint` は[Babel]で拡張されたJavaScriptのコードを[ESLint]で扱うためのパーザだ。

[ESLint]は[ESLint]でちゃんと進化しているから、普通にESとJSXでコードを書くだけなら `babel-eslint` は必要ない。

何故これが必要なのかと言うと、[Flow]というJavaScriptに型を宣言して検証するためのツールを使うからだ。

[Flow]や[ESLint]については、後でちゃんと説明するから安心してほしい。

#### [babel-plugin-transform-flow-strip-types](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-flow-strip-types)
`babel-plugin-transform-flow-strip-types` は名前の通り、[Flow]のために宣言した型に関する情報を自動的に除去するモジュールだ。

これで型に関する情報を削除した後に、書いたコードを対象のランタイムで実行可能なJavaScriptへ変換する。

[Babel]は[Flow]を使わずに固まった言語仕様だけで動かすなら、ほとんど何の設定もいらない。
だから、そんなに[Babel]を忌避することも無いんじゃないかなぁ。

## 型システムについて
型と言っても、何もそんなに難しい話をしようってわけじゃない。JavaScriptに足りない味をちょっとだけ足そうって話だ。

そもそもJavaScriptには型宣言がほぼ全くないので、単純な話として関数の引数に何が渡ってくるのかを呼び出される側のコードだけ見てても絶対に分からない。

呼び出す側と呼び出される側の両方を、短期間のうちに自分ひとりで書くなら、引数としてどんなものを渡すのかなんて全部分かってるだろう。
それなら、わざわざ宣言することも無いだろって話はある。

でも、冷静に考えてほしい。一週間前の自分は、今の自分か？僕は違うと言える。先週の僕が何考えてコード書いてたのかなんて大まかには分かっても、詳細には分からないってのが正直なところだ。

コードにコメントを書くことで、将来の自分を含む他人に情報提供するってのは悪くないアイディアなんだけど、コメントは動かないし、自動的に検証しようがない。
それに対して、ちょっとしたメモ書きとして型を書いておけば、自動的に検証できて幸せになれる。

### JavaScriptにおける型宣言の派閥
ここで少しJavaScript界隈ではどんな方法で型を宣言する方法があるのか確認しておこう。

#### [Google Closure Compiler]
僕がJavaScriptを使う中で型宣言に初めて取り組んだのは、[Google Closure Compilerの型システム](https://github.com/google/closure-compiler/wiki/Types-in-the-Closure-Type-System)だ。

こいつはドキュメントコメントの中にせっせと型宣言を書くと、コンパイラがアレコレチェックしてくれるってやつだ。

```
/**
 * Some class, initialized with an optional value.
 * @param {!Object=} opt_value Some value (optional).
 * @constructor
 */
function MyClass(opt_value) {
  /**
   * Some value.
   * @private {!Object|undefined}
   */
  this.myValue_ = opt_value;
}
```

このアプローチは何が素晴らしいって、型宣言はコメントの中にあるので、ソースコードの動作に全く影響を与えずにコンパイラが必要な情報をプログラマが提供できるってこと。

一方で、コメントの中にある宣言がないがしろにされ易いって問題がある。分かり易いかわりに表現力が極端に低いって問題もある。

[Google Closure Compiler]の`ADVANCED_OPTIMIZATIONS`はコードがまるで動かなくなるリスクがあるけど、マジで小さくなるから本当に驚いた。

#### [TypeScript]
[TypeScript]は、発展的なJavaScriptとして、新しい言語を作ってしまおうという沢山の取り組みの中の一つとして現れた。

僕にとっては、DelphiやC#をデザインした[Anders Hejlsberg](https://github.com/ahejlsberg)先生の最新作という認識で、[TypeScript]は神からの恩寵の一種だと考えている。

[TypeScript]のコードを少しだけ見てみよう。

```
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```
僕の脳内にあるオレオレJavaに本来のJavaよりも似ているのが[TypeScript]だ。

[TypeScript]の型宣言は、動作するコードの中に型アノテーションを混ぜ込むやり方と、型宣言のための専用のファイルを作るやり方が提供されている。
例えば専用ファイルを作るなら、こう宣言する。

```
declare namespace myLib {
    function makeGreeting(s: string): string;
    let numberOfGreetings: number;
}
```

型宣言のための`d.ts`というファイルがあれば、既存の型宣言が全くないライブラリでも型があるかのようにコードを書ける。

型宣言が無い既存のライブラリに対して必要に応じて後付けで型を付けられるってのは、既存資産を活用する上で非常に重要な機能だ。

OSSのモジュールに対する型宣言ファイルを単一のリポジトリに集めようと活動していた[DefinitelyTyped]プロジェクトは、型宣言ファイルの数が多くなり過ぎて破滅した。

今は、[types](https://github.com/types)というOrganizationで、1モジュールに対して1リポジトリを割り当てるという真っ当な方法で管理している。
とはいえ、[DefinitelyTyped]が参照されなくなった訳ではない。

ちなみに、npmリポジトリから型宣言ファイルを取り出すには `npm install -D @types/lodash` 等とすればよい。専用のツールじゃなくて、npmで取れるって所が素晴らしいよね。

#### [Flow]
そして[Flow]だ。こいつはOCamlで作られた静的解析ツールで、JavaScript内に混ぜ込まれた型宣言をみてアレコレチェックしてくれる。

[Flow]で型付けされたJavaScriptのコードを少しだけ見てみよう。

```
// @flow
function total(numbers: Array<number>) {
  var result = 0;
  for (var i = 0; i < numbers.length; i++) {
    result += numbers[i];
  }
  return result;
}

total([1, 2, 3, 4]);
```

ぱっと見、TypeScriptの型アノテーションと似ているよね。でも、[Flow]は[TypeScript]と明確に違う点がある。
[Flow]は、JavaScriptに付与された型アノテーションを評価するなどの静的解析はやるけど、新しいプログラミング言語を作ったわけではないことだ。

[Google Closure Compiler]により近いアプローチと言えなくもない。でも、[Google Closure Compiler]が作られた頃と違って今は[Babel]があるので、言語の構文を一次的に拡張して用が済んだら[Flow]専用の記述だけを安全に取り除くという実装が可能になった。

コメントの中で行う型宣言は、表現力がどうしても限定的になってしまう。[Flow]はそれを乗り越えた。とは言え、HaskellやOCamlとは比べるまでも無く、Javaと比較しても、それ程豊かな表現力があるわけではない。

[Flow]が型を推論してくれる範囲について理解するのは、それ程難しくない。というか[Flow]は凄く簡単な型推論しかやってくれない。

[Flow]にも[TypeScript]と同様に型宣言を共有するための[flow-typed]というリポジトリがある。
何故[DefinitelyTyped]と同じ轍を踏むのかはよく分からないが、単一リポジトリでやっている。

ただ、こちらはPRを投げると結構厳しくレビューされるようで、格納されている型定義ファイルの中身はしっかりしている代わりに、絶対量が少ない。

型宣言ファイルを取り出す時には、`flow-typed`というコマンドを使うとpackage.jsonの中にある依存モジュールの型宣言ファイルを[flow-typed]のリポジトリから自動的にダウンロードしてくれる。
型宣言の無いものに関しては、全部の型がanyだと宣言した型宣言ファイルを雑に作ってくれる。

何でnpmコマンドで型宣言ファイルを取れるようにしないんだろうね？雑な型宣言ファイルを作るコマンドは別に分けておけばいいんじゃないなぁ。

### どの型システムを使うか
流石に今更[Google Closure Compiler]は無いよね。

[TypeScript]と[Flow]は目的が違うので単純に星取表で評価できるようなものでもない。星取表で比べるなら[TypeScript]が圧勝だ。

じゃあ、どちらを使うのが望ましいのか？

使いたいライブラリやフレームワークの対応状況を見て決めるといいんじゃないかな。

少なくとも[Angular]を使うなら[TypeScript]だろうし、[React]使うなら[Flow]で間違いない。

[Vue.jsには公式のTypeScript用型定義ファイルがある](https://vuejs.org/v2/guide/typescript.html)。でも、これは[TypeScript]の利用を推奨しているわけではなく、一部のユーザによる貢献がマージされた結果らしい。

素のJavaScriptを使うなら[Flow]を使うのが良いし、使おうと思ってるライブラリの型定義ファイルがしっかり揃ってるなら[TypeScript]はかなり良い選択肢になる。
特に[VS Code]は型定義ファイルが揃っていると入力補完の精度が明確に改善するからね。

一応補足しておくと[Flow Language Support](https://marketplace.visualstudio.com/items?itemName=flowtype.flow-for-vscode)には入力補完を強化する機能もあるよ。

## 型システムに関する積み残し課題
このエントリ公開後に識者から貰った指摘を補足しておく。

* [Dart](https://www.dartlang.org/)
  * [TypeScript]よりも強力な型推論をしてくれる言語だ
  * [dart2js](https://webdev.dartlang.org/tools/dart2js)でJavaScriptに変換できる
  * [Angular]2系のGoogle内部における[大規模事例は既に存在する](http://news.dartlang.org/2016/03/the-new-adwords-ui-uses-dart-we-asked.html)
  * 大規模事例は[Angular 2 Dart](https://github.com/dart-lang/angular2)によるものだ


## 静的解析でベストプラクティスを共有しよう
JavaScriptには多くの罠がある。ここでいう罠と言うのは多くのプログラマが誤解し易い言語仕様や、ある種のライブラリの動作のことだ。
JavaScriptは本当に柔軟な言語なので、新しい言語仕様のほとんどは古い言語に直接変換できる。だからこそ、罠も多い。

プログラマがミスした結果記述されるコードや、おおよそ仕様を誤解していない限り書かれないようなコード。大抵の状況でバグだとみなせるコード。
言語仕様には即しているけども、アプリケーションとして動作させても期待通りの結果を得られないようなコードを自動的に見つけるためのツールがLintだ。

もう少しアグレッシブにLintを使うと、インデントのやり方や、変数名の付け方みたいにコードの動作とは関係ないけど、コードの読み易さには関係があるものも検査できる。

### JavaScriptにおけるLintの歴史
JavaScriptはちょっと調べると沢山のLintがある。僕が使ったことがあるもので最も古いのは[JSLint](http://www.jslint.com/)だ。
こいつは、伝説の[Douglas Crockford](https://github.com/douglascrockford)先生が作ったLintで凄く厳しい。一切の甘えを許さないストロングスタイルだ。
JavaScriptの奇妙な動作には絶対に惑わされない、奇妙な動作をしうるコードは奇妙な見た目になるはずだという強い意志を感じるツールなのでヘタレには辛い。

ちなみに、[JSLintのコード](https://github.com/douglascrockford/JSLint)は読むとすごく勉強になる。コンパクトで読み易いよ。

僕のようなヘタレにとって福音となったLintは[JSHint](http://jshint.com/)だ。これは余り厳しくない。設定ファイルを切り出したり出来るんで凄く使いやすい。
ただ、JSHintは組み込まれた機能を有効化したり無効化するのは簡単だけど、新しいルールを追加するのはちょっと面倒だ。

それに誰かのオススメルールを自分のプロジェクトに取り込むには、設定をコピー＆ペーストしないといけない。
Lintのルールを作り込むなんて、僕みたいな一部のマニアがやる作業で大抵の人は誰かのオススメセットをパッと使いたい。
そのオススメセットがメンテナンスされているなら、面倒なことをせずにその最終成果だけを使いたいよね。

そういうわけで、拡張し易く、また設定ファイルを作り込みやすい[ESLint]が今はオススメなのだ。

[ESLint]はプラグインシステムがよく出来ているので、沢山のプラグインがあるし、オススメのルールセットをnpmモジュールとして公開できる。

Airbnbが公開している[eslint-config-airbnb](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb)は、そういうオススメルールセットの一種だ。

Airbnbのは僕の考えと合わないものが多く含まれているので、僕は採用しなかったけどね。

### [ESlint]関連の理解すべきモジュール
[ESLint]関連のモジュールと言えば、おおむねプラグインのことになる。

他にも沢山あるとは思うけど、僕が見つけて使ってみたものの中で特に有用だと感じたものをここでは列挙しておく。

もしあなたが[ESLint]マニアで、僕が知らない便利なプラグインを知っているなら是非教えてほしい。
 
#### [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava)
僕のプロジェクトでは[AVA]というテスティングフレームワークを使っている。[AVA]については後で説明する。

[AVA]は凄くシンプルで使い易いけども、シンプル過ぎてどうテストコードを書けばいいのか慣れるまでは分かりづらい。

そこで、このプラグインを使うと[AVA]を明らかに間違った使い方をしている時に警告して貰えるようになる。

#### [eslint-plugin-import](https://github.com/benmosher/eslint-plugin-import)
`eslint-plugin-import` は `import`文に関するミスを見つけてくれる。

具体的にはプロジェクト内には存在しないモジュールを`import`しようとすると警告がでる。モジュール名をtypoすることくらい誰にでもあるよね。

他にも `require` に対してリテラル以外の文字列を渡しているとエラーになったりする。

#### [eslint-import-resolver-node](https://github.com/benmosher/eslint-plugin-import/tree/master/resolvers/node)
こいつだけは他のモジュールと違った役割を持っている。

`eslint-import-resolver-node` は `eslint-plugin-import` と協調動作して `import` するモジュールの探し方をカスタマイズできる。

どういうことかと言うと、僕の作ったプロジェクトでは、アプリケーションコードとテストコードをそれぞれ`src`と`test`という別なディレクトリに格納している。

このままだとテストコード側で、アプリケーションコードを `import` するときに、`import world from "../../src/hello/world";`とか書かなくちゃいけなくて凄く辛い。
それを避けるために、テストコードを実行するときに `NODE_PATH=src` とすることで、その `../../src/` 部分を記述しなくても良いようにしている。

このことを`eslint-plugin-import`に知らせるために`eslint-import-resolver-node`を使っているのだ。

#### [eslint-plugin-promise](https://github.com/xjamundx/eslint-plugin-promise)
JavaScriptの`Promise`は慣れが必要なタイプのライブラリだ。しかも、正しい使い方が凄く分かりづらい。
ちなみに、[JavaScript Promiseの本](http://azu.github.io/promises-book/)は大変素晴らしいドキュメントなので何度でも読める。

`Promise`に慣れるまでの間は誤った`Promise`の使い方をしている可能性が高い。
近くにJavaScriptのエキスパートがいてコードレビューして貰えるなら、それは大変素晴らしいけど、趣味でちょっとJavaScript書いてるだけなのに、そんなエキスパートに声かけたりは出来ないよね。

そこで、このプラグインを使うことで、明らかにダメな`Promise`のコードにはエラーを出して貰おうってわけだ。
このプラグインのルール詳細を読むだけでも、避けるべき`Promise`の使い方を理解できるので、是非丁寧にルールを設定してみてほしい。

ところで、ESに `async/await` が入ったら`Promise`を直接使うことは無くなるんだろうか？

僕はC#の `async/await` を利用した経験から `async/await` と `Promise`は普通に共存すると考えているが、実際はどうだろうね。
少なくとも、[AVA]では`async/await`を使うと、テストコードが分かり易くなる印象はある。

#### [eslint-plugin-security](https://github.com/nodesecurity/eslint-plugin-security)
あなたはコード書きながらセキュリティについて気にしているだろうか？
僕は静的解析ツールを使ったセキュリティレビューを仕事としてやっているので、それなりに気にしているつもりだ。
でも、常に頭の中にセキュリティに対する考慮があるかと言われると、そんなことはない。

このプラグインでチェックできる脆弱性は、それ程多くは無いけども、ついウッカリ作り込んでしまうような脆弱性をパッと見つけてくれる。

普通にコードを書いてるぶんには、このプラグインによるエラーをみることは余り無いだろうけども、こいつに怒られた時は、ちょっと慎重になってほしい。

#### [eslint-plugin-flowtype](https://github.com/gajus/eslint-plugin-flowtype)
[Flow]を使って型宣言をするのはいいんだけども、しょうもないミスをすることはある。

また、[Flow]による型宣言は通常のJavaScriptではないので[ESLint]にある標準のルールは全く適用されない。
つまり、インデントやら、行末にセミコロン付けろとか、ケツカンマを殺せだのは、[Flow]用に再実装する必要がある。

[Flow]自体にもそれなりに罠はあるし、一貫性のある型宣言をするなら、少々Lintにかけておいた方が良いだろうね。

`Promise`のところでも同じ話をしたけども、[Flow]も新しい技術だから慣れるまでの養成ギブスとして`eslint-plugin-flowtype`を使うのが良いんじゃないかな。
ちゃんと慣れれば、エラーが完全に出なくなるだろうし。

#### [eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)
僕のプロジェクトではUIを構築するためのフレームワークとして[React]を使っている。

[React]は非常に良く使われているので、多くのベストプラクティスと共に、望ましくないコーディングスタイルが発見されている。

ルールとして定義されているもの全てが自分たちのプロジェクトにとって望ましいとは限らないけれども、ルール化されているものについて是非を議論するのは、その知識を生み出すよりもはるかに容易だ。

実際、このプラグインで定義されているようなルールに関する知識を自力で得るには小さなプロジェクトで簡単に使ってみるだけでは足りないだろう。

デファクトスタンダードなフレームワークを使うことのメリットはこういうところにある。

#### [eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y)
自分たちが作っているシステムの `Accessibility` にはどれくらい気を使っているだろうか？

僕は、おろそかになりがちなSecurityよりもさらに優先順位が低いのが `Accessibility` じゃないかなと感じている。

リッチなUIにするのは、より多くのユーザにより良い体験を提供するためなのだから、その延長線上にハンディキャップのあるユーザへの配慮があっても良いんじゃないかな。

とは言え、[WAI-ARIA1.1](https://www.w3.org/TR/2016/CR-wai-aria-1.1-20161027/)や、[MDNのARIAに関するページ](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA)を端から端まで読んでガッツリやるぞと言っても無理があるよね。

[HTML5 Accessibility](http://www.html5accessibility.com/)くらいの使い易いリファレンス片手に最小限の手間で最大限の価値を提供したいものだ。

そういうわけで、まずは、`eslint-plugin-jsx-a11y` のオススメルールを使ってプロジェクト内のJSXが `Accessibility` に関する対応が妥当なのか確認しよう。

# テストの話
一口にテストと言っても色々ある。ここではディベロッパーテスティングについて話をする。

僕は、アプリケーション作る上でプログラマが最も熟達すべき技術はテストだと考えている。
あるべき姿が何なのかをテストという形で表現した上で、対象のソフトウェアを実装することで、より良いものが作れるはずだと信じているからだ。
つまり、本物のテストエンジニアがやってるようなことを出来るようになるべきって話をしたいわけではない。

品質保証には膨大な体系化された知識があり、アプリケーション作るための知識と両方を備えられれば素晴らしいけど、時間は有限だ。
家族がいたり、やりたいゲームがあったりする。長生きするために一日十時間は寝たいし、ゆっくり旨い飯を食いたい。

何をテストすべきか、どんなテストすべきかを学ぶことに時間を使うべきであって、テスティングフレームワークのクールな使い方を学ぶことに時間を割いてはいけない。

そういうわけで、テスティングフレームワークはAPIが出来るだけ簡素なものを選ぼう。

つまり、[AVA]だ。

## [AVA]のススメ

[AVA]で書いたテストコードはこうなる。

```
import test from 'ava';

test('foo', t => {
    t.pass();
});

test('bar', async t => {
    const bar = Promise.resolve('bar');

    t.is(await bar, 'bar');
});
```

僕が何故[AVA]を気に入っているのか簡単に列挙しておこう。

* APIが小さい
* [power-assert]が標準で組み込み済
* 実行速度が速い
* `=>`演算子、`Promise`、`async/await`、[observable](https://github.com/tc39/proposal-observable)へのサポートが完璧

### [AVA]のAPIは小さい
`import test from 'ava';`でimportしたら、`test`変数に定義されている関数を呼び出せばやりたいことが全部できる。

そして、この`test`がデフォルト関数になっていることを含めても関数が11個しかない。列挙してみよう

* **test([title], implementation)**
* test.serial([title], implementation)
* test.cb([title], implementation)
* test.only([title], implementation)
* test.skip([title], implementation)
* **test.todo(title)**
* test.failing([title], implementation)
* test.before([title], implementation)
* test.after([title], implementation)
* **test.beforeEach([title], implementation)**
* **test.afterEach([title], implementation)**

この中で覚えておくべきは、強調した4つのAPIだけだ。それ以外は必要になった時に、マニュアルを参照すればいい。

[AVA]はglobal空間や暗黙の`this`に配置された関数を前提に動作しないってことも大事なことだ。

### [power-assert]が標準で組み込まれている
テストを書く時には、何も考えずに使えば最高にリッチなアサーションエラーをはいてくれる[power-assert]使おう。

こんなテストが…
```
test(t => {
    const a = /foo/;
    const b = 'bar';
    const c = 'baz';
    t.true(a.test(b) || b === c);
});
```

こんなエラーになる。
```
t.true(a.test(b) || b === c)
       |      |     |     |
       |      "bar" "bar" "baz"
       false
```
マジ、最高。

[power-assert]はこのアサーションエラーを出すためにコードを書き換えるんだけども、そのためのセットアップが少々面倒だ。やれば簡単だしマニュアルも揃ってるんだけど、まぁ、面倒は面倒だ。

でも、[AVA]を使っておけば、そのちょっとした面倒すらない。

### 実行速度が速い
テストの実行速度が問題になるのは、本物のアプリケーションをちゃんとリリースして、それがメンテナンスフェーズに入ってからだ。

ちょっとお試しで動かしてるうちは、実行速度が5秒だろうが、1秒だろうが変わりはない。でも、テストコードの量が1000件とか10000件とかなったらどうだろうか？

最早、自分のローカルマシンでは、ちょっとコードを変更するたびにテストを全件走らせるのは無理だ。

自分が変えた部分と直接関係ありそうなテストコードをサッと実行して、後はJenkinsなりCircleCIなりといったCIサーバにブン投げて祈るだけだ。知らんところがコケませんように…。が、コケる。

どうせテストは定期的にコケるんだから、変更→テスト→デバッグ→変更……のサイクルは短い方が良い。

[AVA]はテストの実行速度を確保するために、あまり複雑な構造のテストコードを敢えて書けないようにしてある。
[Mocha](https://mochajs.org/)みたいなテスティングフレームワークで複雑な構造のテストを書いてきた人からすると、奇妙に見えるかもしれないが慣れだ。

テストコードの変数スコープが何重にも積み重なってると、書いてる時はいいけど、どうせ後で苦労することになる。

### 各種サポートが完璧
これは[AVA]のAPIが簡素だから完璧にしうるって話でしかない。

`this`を下手に書き換えないから`=>`演算子が快適に動く。

テストメソッドの戻り値は[AVA]の責任範囲だって決まってるから、`Promise`だろうが、[observable](https://github.com/tc39/proposal-observable)だろうが返しておけば[AVA]がよろしくやっといてくれる。

テストコードのファイルに並ぶ関数呼び出しは入れ子にならない。

ちなみに、`test.beforeEach`で作った変数を`test`で参照するには、こうする。

```
test.beforeEach(t => {
    t.context = 'unicorn';
});

test(t => {
    t.is(t.context, 'unicorn');
});
```
`context`は、複数の`test`間では共有されていないので、存分に触ってよろしい。
ただし、複数の`test`は`test.serial`を使わない限り順序性が保証されないので、`test`の中で`context`を書き換えてはいけない。
そもそも、`test.serial`は何か特別な理由がない限り使ってはいけない。

尚、テストメソッド間で何らかの情報を共有したり、テストの実行順序に意味があるような書き方をするのは悪手なのでやらないように。

## テストに関する積み残し課題
僕が学習すべきだけど、まだ手を付けられていない課題について、誰かが補完してくれることを願って少し書いておく。

* E2Eテストまたはシステムテスト
  * [Protractor](http://www.protractortest.org/)で[Reactのアプリケーションをテストできるらしい](http://www.joelotter.com/2015/04/18/protractor-reactjs.html)
* ミューテーションテスト
  * [Stryker](http://stryker-mutator.github.io/)が良さげだけど、[AVA]サポートが無い
* 負荷テスト
  * 特にJSで作られたリッチなUIの負荷テストの方法論やそのためのツールはあっていいはずだけど、上手く見つけられていない

# 如何にしてUIを作るか
ここからは、JavaScriptでユーザインターフェースを構築する方法について説明していこう。

## 道具の選び方について

10年くらい前にGmailやGoogleMapsを初めて使った時、こんなにもハイパフォーマンスで豊かな画面表現がJavaScriptだけで可能なのかって、本当に驚いたよね。

DHTMLとか言ってJavaScriptを使ってウェブサイトに動きを付けるみたいなことに取り組んでる人々は、それ以前からいたけども大体が忌み嫌われていた。

リッチなユーザインターフェースを持つアプリケーションをブラウザ内で動かすならFlashを使うしかなかった。他にもブラウザプラグインみたいなテクノロジはあったけども、結局ほとんど使われなかった。

GmailやGoogleMapsのように現実的なパフォーマンスで動作するGUIアプリケーションをブラウザ上でJavaScriptだけを使って実装するという取り組みは、あの頃から活発化したけど、ここ数年に至るまで、その取り組みはそれ程上手くいっていなかった。

確かに[YUI Library](http://yuilibrary.com/)や[Ext.js](https://www.sencha.com/products/extjs/)、[Closure Library](https://developers.google.com/closure/library/)といったライブラリが次々と出てきたけれども、
それまでVBやDelphiで出来てたようなUIを作るのはとてつもなく難しかった。

例えば、データを1000件程度表示した上で、各セルがそれなりに複雑な動作をするようなグリッドコントロールを作るのは現実味が無かった。

結果的に、CSSに加えてjQueryで少々頑張るというのが定番になった。少し動きのあるウェブサイトが作りたいだけならjQueryでやればいい。
大人数で作業したり、長期間メンテナンスしたり、物凄い作り込みをしたりしなければ、おおむね上手くいく。
大抵のウェブサイトはGUIアプリケーションでは無いのだから、jQueryとそのプラグインを便利に使えばさっと作れる。

ところで、高度に作り込まれたウェブサイトと、ブラウザ上で動作するGUIアプリケーションに明確な境界線はあるのだろうか？

定量的な判断基準を見たことは無いが、一人か二人のウェブデザイナーが片手間にjQueryを使って動きを付けるだけで破たんせずに作れる範囲は少なくともウェブサイトだろう。

一方で、サーバアプリケーションが存在し、きちんとしたJavaScriptが書けるプログラマが3人も4人もUIを作るために必要で、
UIデザインとその実装は役割として分担しなければ成立しないようなものは、ブラウザ上で動作するGUIアプリケーションだろう。

実際には、こんなに分かり易く区別できないだろうけどね。

この区別が何故重要なのかと言うと、使うべきツールキットや揃えるべきチームメンバーのスキルセットが違うからだ。

[React]や[Angular]のように高度なGUIアプリケーションを実装するためのフレームワークを使ってウェブサイトを作ろうとするなら、工数を無駄に大量消費することになるだろう。

でも、jQueryプラグインを駆使して高度なGUIアプリケーションを実装しようとするなら、その先には破滅が待っているだろう。

道具は適した場所で適切に使うべきなのだ。

## Virtual DOMというブレイクスルー
ブラウザ上で動作するGUIアプリケーションを作るにあたって確実に解決すべき課題はパフォーマンスと操作性だ。

特にパフォーマンスの問題は非常に解決が難しい。ブラウザ上で動作する以上、全てのUI要素は何らかの形でDOMに落とし込まれるからだ。

まず、DOMは汎用的な木構造をしているのでメモリ効率が恐ろしく悪い。
加えて、この木構造は中間の内部ノードの数を限定できず、末端ノードまでの深さも限定できないので走査するコストが高くなり易い。

ブラウザがDOMを画面に描画するためには、全てのノードを何らかの形で走査する必要があるので、この効率の悪さは非常に大きな問題となる。

加えて、CSSというDOM構造を完全に無視して描画要素をレイアウトする仕組みが、パフォーマンスの悪さに拍車をかける。

末端のノードに現れたclass属性一つで画面全体を再描画しなければならなくなることなど、ブラウザの世界では当たり前なのだ。
つまり、描画済みのDOMに対して新しく作ったノードを`appendChild`をするだけで画面全体のロックをとる必要があるのだ。

とは言え、その新しいノードが画面全体に与える影響が十分に小さければロックを確保し続けなければいけない時間は十分に短くなる。

ある種のコンポーネントを画面内で一つ更新しただけで、画面全体に再描画がかかる可能性があり、そうならないよう細心の注意を払う必要がある。
そんなことでは、一部の例外的な組織を除いて、効率よく大規模なGUIアプリケーションを作ることなど出来ないのだ。

この問題への解答となる技術がVirtual DOMだ。

まず、画面を描画するために必要なデータ（モデル）と、そのデータを流し込むテンプレート（ビュー）に分けて考える。
これにユーザ入力からモデルを更新する役割を持つコントローラを追加すれば古典的なMVCモデルとなる。

原始的なやり方では、モデルの構成要素のうちどれが変更されたかに応じて、ビューのどの部分を変更するのかをプログラマが一々決めていた。ごくごく単純化した例はこうだ。

前半の`user`という変数を、後半部分のHTMLっぽいテンプレートに流し込むと考えてほしい。

```
// モデルの例
var user = {
  "name": "John Doe",
  "age" : 22
}
// ビューの例
<html><body>
<div class="name">
Name: ${user.name}
</div>
<div class="age">
Age: ${user.age}
</div>
</body></html>
```

モデルを更新するというのはつまり、`user`という変数のメンバである`name`や`age`の内容を変えるということだ。
原始的なやり方では、どのメンバ変数を変えたかによって、対応するDOMの要素を探すコードをプログラマが書かなければならない。
確かにこれくらい単純な例では、jQueryで`$(".name")`とか書けば対応する要素を見つけ出すことはできる。

考えてみてほしい、画面内に変更可能性のある要素が1000個単位であったらどうだろうか？
ある要素を変更すると、それに影響を受けて推移的に変更が必要になる要素があったらどうだろうか？
自分がコンポーネントを画面に組込んだ時には存在しなかった要素の影響を受けて、自分のコンポーネントが変更されるべきだったら、どうだろうか？

悪夢だ。

雑に要約するとVirtual DOMは現在のDOMと、次に出力されるDOMの差分を取って、その差分だけをブラウザのレンダリングエンジンに渡してくれる仕組みだ。
以前は人間が気合いで部分更新するコードを書いてたのを、自動的にやってくれる仕組みなのだ。
当然、職人が一つずつ更新すべき要素を選定する実装に比べれば動作が遅くなりうる。

現実的には、そういう職人はごく稀にしかいないので、自動的に差分を取ってもらった方が、おおむね高速に動作する。

似たような議論は、C言語とアセンブラの頃、もしくはその前からあった。
アプリケーションを全部ハンドアセンブルしたら、普通のコンパイラが出したバイナリよりも高速なバイナリを作れるとか、そういう話だ。
ハンドアセンブルする対象を小さく限定するとか、アプリケーションが十分に小さくなければ成り立たない話だけどね。

## どのフレームワークを使うか

ざっくりと、僕が選定候補にしたものを挙げるとこうだ。どれも、内部ではVirtual DOMを使っている。

* [React]
* [Angular]
* [Vue.js]
* [Mithril]

### [React]
現状[React]がデファクトスタンダードと言えるので、[React]を選択し学ぶのが最も妥当であろう。
コミュニティが十分に大きく周辺ツールも充実している。
[SurviveJS](https://survivejs.com/)のように質の良いチュートリアルも沢山ある。

そういうわけで、僕のプロジェクトでは[React]を選んだ。

[Inferno](https://github.com/infernojs/inferno)や[Rax](https://github.com/alibaba/rax)といった[React]と互換性のあるフレームワークもあるが、
彼らのやっている事に妥当性があるなら、それはいずれ[React]に取り込まれていくだろう。

### [Angular]
[TypeScript]が第一言語として指定されている[Angular]は僕にとってかなり魅力的な選択肢だ。
ただ、僕が観測する限り[React]に比べてコミュニティが十分に大きいとは言えない。
これは安定的に動作するものがリリースされてからの期間が相対的に短いせいだ。

現行の[Angular]は2.4だけど、[Angular]の1系は僕の好きなタイプのフレームワークだ。
何せ、まず2way bindingがあり、ゴミ置き場としてあらゆる黒魔術的な行為が許されているDirectiveがあり、DIコンテナがあった。
digestループは極めて分かり辛い概念だったが、画面を作り易くするためには、その複雑さは受け入れられるべきだと考えた。

[Angular]の1系は、業務アプリケーションを作るために設計されたとしか思えないような機能群でSIerのためにあるかのようなフレームワークだった。

[Angular]の2系に大きな学習コストを投下するのは、Google内部の本格的な採用事例が公開されてからでも遅くは無いんじゃないかと考えている。

### [Vue.js]
[Vue.js]は今まさに話題のフレームワークだ。[Angular]の1系が好きだった僕が選ぶべきは[Vue.js]かもしれない。

でも、僕には[メインの開発者が一人しかない](https://github.com/vuejs/vue/graphs/contributors)ものを基盤となる部分には採用できない。
[Evan You](https://github.com/yyx990803)一人がほんの少し熱意を失ったり、何らかの政治的取引によって翻意するだけで壊滅的な状況になりうる。
それは恐ろしいことだし、僕には耐えがたいリスクだ。

ざっと読む限りコードベースはそれなりに妥当な印象を受けるし、かなり丁寧なドキュメントがある。
少し使ってみた感じでは機能に不足は無いようだ。
ただ、それなりに大きなアプリケーションを作ろうと思った時に、思いがけないところで根源的な問題を引いてしまい、それに上手く対応できない可能性を考えてしまう。

大きなコミュニティがあり、実稼働しているアプリケーションでキチンと使われているものは、細かい問題の多くは発見され丁寧に対処されている。
一見すると無意味な複雑さを持っているように見えるコードも、ある種の状況では重要な意味があるのかもしれない。

これは僕の粗悪な印象論に過ぎないが、[Angular]の2系と[Vue.js]は本気のプロダクション環境で使われているようには見えないコードベースの綺麗さがある。

ちなみに、[Angular]の1系はプロダクション環境で使われているように感じられるコードベースだった。

### [Mithril]
[Mithril]の良さは小さいこと。フレームワークの内部詳細を完全に理解した上で、足りないものに自分で気が付いて、自分で足せる人が使う道具だ。

小さいからこそ、それができる。

GUIアプリケーションの開発経験が十分にあるエンジニアだけで構成された、少人数のチームで使うなら合うかもしれない。

アプリケーションを作りながら足りないものに気が付いたとして、その足りないものに関する仕様を適切に定めて、適切に実装することは本当に難しい。
特にプロジェクト進行中のコスト圧力が強いとき、上手くやれるだろうか？少なくとも、僕には出来ないだろう。

## [React]の使い方について
[React]の本体はビューの部分に対してのみ役割を持つので、アプリケーションを作る上では色々と不足がある。

Routerについては余り悩まずに、[react-router](https://github.com/ReactTraining/react-router)を選んだ。僕自身がRouterを選定するための基準を持っていないせいだとも言える。

テスト用のユーティリティとして、[enzyme](https://github.com/airbnb/enzyme)は外せないだろう。
フレームワークを使い込んだユーザが作りこんだ品質の高いモジュールを利用できることが、デファクトスタンダードなライブラリを使うことの最大のメリットだ。

### [CSS Modules]について
CSSというのはつまり、グローバル変数しかないプログラミング言語みたいなもので、デフォルトのスコープがローカル変数な世界に住んでいるプログラマからすると、ありえない環境だ。
コードの量に比例して複雑さが上がっていく環境で完全に整合性のとれた仕事なんて出来るわけがないと考えてしまう。

この問題に今まではどう対応してきたのかというと、命名規則を人間が明示的に守ることによって名前空間を分割してきた。
例えば、メジャーな命名規則には[OOCSS](http://oocss.org/)、[BEM](http://getbem.com/)、[SMACSS](https://smacss.com/)がある。

また、[Sass(SCSS)](http://sass-lang.com/)や[less](http://lesscss.org/)と言ったメタ言語を使って、より安全にCSSを記述するという取組みもなされてきた。
これらのメタ言語が持つ機能のうち最も重要なのはスタイルの定義を入れ子にすることで影響範囲を限定する機能だ。

メタ言語はCSS自体の複雑性を制御する仕組みとして優れているけども、そもそもCSSはDOM構造に対して、どのような見た目を与えるのか決定する仕組みなので、DOM構造の設計とCSSの設計は切り離せない。

そう考えると、[React]コンポーネントの設計とCSSの設計は整合性が取れていることが望ましい。それぞれを完全に独立した事象として扱おうとするのは無理があるだろう。

[React]コンポーネントの設計とCSSの設計の整合性をとるには、どちらかのやり方を優先するしかない。それなら、CSSにローカル変数を持ち込み、それをデフォルトの挙動とするのが望ましい。

これを実現するために考え出された概念と取組みが[CSS Modules]だ。

[CSS Modules]は[React]のコミュニティから発生した概念だけど、似た機能は[Vue.jsにもある](https://vue-loader.vuejs.org/en/features/css-modules.html)し、
[Angular2にも同様の機能はある](http://joaogarin.github.io/css-modules-angular2/)ようだ。

[CSS Modules]が解決するCSSの問題に関する詳細な情報は[CSS Modulesチームのメンバーによるブログエントリがある](http://glenmaddern.com/articles/css-modules)ので、そちらを参照してほしい。

要約しておくと、[CSS Modules]は単に新しい命名規則に基づくCSSの設計方法論の一種ではない。また、[CSS Modules]を使ったからといって、CSSのメタ言語を使わなくていいわけでもない。
既存の命名規則に慣れたWebデザイナであっても、大人数で作業分担するようなプロジェクトでUIデザインをするなら[CSS Modules]を学ぶべきだろう。

尚、[CSS Modules]を使ったとしても、アプリケーション全体に対してUIの一貫性を持たせるために設計するCSSは、以前と変わらずグローバルな空間に対して定義していくので、既存のCSSに関する設計知識が不要になるわけでもない。

### [React]における[CSS Modules]
[React]に[CSS Modules]を導入するには[webpack]の[css-loader]を使ってCSSを処理した上で専用の記述を[React]コンポーネント側に行う。[webpack]については、後で説明する。

#### [CSS Modules]の導入
一番簡単に[CSS Modules]を使うコードはこうだ。

```
import React from 'react';
import styles from './table.css';

export default class Table extends React.Component {
    render () {
        return <div className={styles.table}>
            <div className={styles.row}>
                <div className={styles.cell}>A0</div>
                <div className={styles.cell}>B0</div>
            </div>
        </div>;
    }
}
```

これがレンダリングされると、大体こういうHTMLになる。[css-loader]によって自動的に重複が発生しないようなcssのクラス名が命名されるので、奇妙な名前がclass属性に設定されている。

```
<div class="table__table___32osj">
    <div class="table__row___2w27N">
        <div class="table__cell___1oVw5">A0</div>
        <div class="table__cell___1oVw5">B0</div>
    </div>
</div>
```

このやり方は、CSSをimportしたうえでJavaScriptのオブジェクトであるかのように扱っていることが、問題だ。

この場合、スタイルの状態を全く評価する必要のないユニットテスト時にもCSSがコンポーネントに対して適用されなければならない。
このままナイーブな方法でimportを処理するとimport先のCSSをJavaScriptとしてパーズできないのでエラーになってしまう。

中括弧が多いのもコードとして書きづらいので出来れば避けたい。
Shiftが要るので中括弧はタイプしづらいのだ。

#### [react-css-modules](https://github.com/gajus/react-css-modules)による改善
一番簡単な方法で[CSS Modules]を使う際に発生する問題を改善するモジュールとして、[react-css-modules]がある。

これを使うとこんなコードになる。

```
import React from 'react';
import CSSModules from 'react-css-modules';
import styles from './table.css';

class Table extends React.Component {
    render () {
        return <div styleName='table'>
            <div styleName='row'>
                <div styleName='cell'>A0</div>
                <div styleName='cell'>B0</div>
            </div>
        </div>;
    }
}

export default CSSModules(Table, styles);
```

中括弧が無くなった代わりに、[react-css-modules]をimportするようになった。

レンダリングされるとclass属性に追加される、`styleName`という特別なプロパティが導入されている。

コードの書きやすさは改善したけれども、コードの難しさは余り改善していない。

特に最後の行を見ればわかるように、importしたCSSをJavaScriptオブジェクトとして取り扱っているという問題は解決していない。

#### [babel-plugin-react-css-modules]による改善
[react-css-modules]の問題を改善する[Babel]のプラグインとして[babel-plugin-react-css-modules]がある。

ちなみに、[react-css-modules]と[babel-plugin-react-css-modules]の開発者は同じだ。

これを使うとこんなコードになる。

```
import React from 'react';
import './table.css';

export default class Table extends React.Component {
  render () {
    return <div styleName='table'>
      <div styleName='row'>
        <div styleName='cell'>A0</div>
        <div styleName='cell'>B0</div>
      </div>
    </div>;
  }
}
```

これでCSSをJavaScriptオブジェクトとして扱わずに済むようになった。CSSとコンポーネントとの関連を表すためだけにimport文を使っている。

[babel-plugin-react-css-modules]は、[CSS Modules]として必要な処理をコンパイル時に行う。
そのおかげで、コンポーネントのコードが[CSS Modules]のためだけに増えることがない。

こういう形で、コードがスッキリするのはいいことだ。

CSSのimportがコード上でコンポーネントに影響を与えていないので、[ignore-styles](https://github.com/bkonkle/ignore-styles)のようなモジュールで当該部分を外しても悪影響がない。

というわけで、僕のプロジェクトでは[CSS Modules]を実装するのに、[css-loader]と[babel-plugin-react-css-modules]の組み合わせを使うことにした。

### [Flux]実装について
アプリケーションの構造を規定するアーキテクチャとしては[Flux]を採用するのが良いだろう。
[Flux]を実装しているライブラリは数多くあるが、人気があり扱い易いので[Redux]を使う。

[Redux]を使う上で、ActionやActionCreatorにどれだけの役割と責任を持たせるのかは、アプリケーションの規模感によって最適解が変わる。

[redux-thunk](https://github.com/gaearon/redux-thunk)や[redux-promise](https://github.com/acdlite/redux-promise)を使うと動作モデルとしては非常に分かり易いが、ActionCreatorのコードが大きくなり易くなる。
非同期処理に関するコードがActionの中に現れるので、コードベースが大きくなる過程で、何らかの基準を設定してActionからコードを分離する施策が必要になる。

ごく単純化して説明すると、`redux-thunk`はコールバックモデルでActionを実装する。`redux-promise`は`Promise`モデルでActionを実装する。

この単純なやり方の問題点は、タスクのキャンセルが出来ないことだ。
サーバとクライアントの間で何らかの不整合があったり、レスポンスの遅さに苛立ったユーザが処理を中断したくなることは、GUIアプリケーションではよくある。
そういう時に、ユーザ操作を起点に処理中のタスクをキャンセルする方法が無いというのは望ましくない。

[redux-saga]や[redux-observable]は、動作モデルを理解するのがかなり難しいモジュールだ。
代わりにActionCreatorのコードは単純になり、Actionは発生したイベントの内容を格納するかなり単純なオブジェクトになる。

[redux-saga]と[redux-observable]は、それぞれ名前は違うが同様の役割をもつ新しいレイヤーを[Redux]に追加する。

ここでも、単純化して説明すると、[redux-saga]は GeneratorFunctionを使って追加のレイヤーを実装し、[redux-observable]は[RxJS]を使って追加のレイヤーを実装する。

[redux-saga]と[redux-observable]は、どちらもタスクをキャンセルする方法がある。

エラーハンドリングするコードを書く時、[redux-saga]と[redux-observable]の違いは明確になる。
詳細については、それぞれのドキュメントを見てほしいが、要は`try/catch`を使うか、ライブラリ定義のイベントハンドラを使うかが違う。

* redux-sagaの[Error handling](https://redux-saga.github.io/redux-saga/docs/basics/ErrorHandling.html)
* RxJSの[Error Handling](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/gettingstarted/errors.md)

どんなに言いつくろったところで、合法的な`goto`でしかない`try/catch`構文が僕は嫌いだ。仕方なく書くことはあっても、積極的には書きたくない。

ここまでの議論をまとめるとこうだ。

<table><thead>
<tr>
<th align="left">名前</th>
<th align="left">実装技術</th>
<th align="center">難易度</th>
<th align="center">Action</th>
<th align="center">追加レイヤ</th>
<th align="center">キャンセル</th>
<th align="left">エラー処理</th>
</tr>
</thead><tbody>
<tr>
<td align="left">redux-thunk</td>
<td align="left">callback</td>
<td align="center">低</td>
<td align="center">大</td>
<td align="center">不要</td>
<td align="center">×</td>
<td align="left">try/catch</td>
</tr>
<tr>
<td align="left">redux-promise</td>
<td align="left">Promise</td>
<td align="center">低</td>
<td align="center">大</td>
<td align="center">不要</td>
<td align="center">×</td>
<td align="left">イベントハンドラ</td>
</tr>
<tr>
<td align="left">redux-saga</td>
<td align="left">Generator</td>
<td align="center">中</td>
<td align="center">小</td>
<td align="center">要</td>
<td align="center">○</td>
<td align="left">try/catch</td>
</tr>
<tr>
<td align="left">redux-observable</td>
<td align="left">Rx</td>
<td align="center">高</td>
<td align="center">小</td>
<td align="center">要</td>
<td align="center">○</td>
<td align="left">イベントハンドラ</td>
</tr>
</tbody></table>

そういうわけで、僕のプロジェクトでは[redux-observable]を選んだ。

## UIに関する積み残し課題
僕が学習すべきだけど、まだ手を付けられていない課題について、誰かが補完してくれることを願って少し書いておく。

* JavaScriptのGUIアプリケーションにおける操作性の話
  * キーボード中心の操作や、マウス以外のポインティングデバイスを使った操作、タッチディスプレイによる操作などに対応するための方法論の話
  * ウェブアプリケーションにおける操作性の評価基準の話
* [PostCSS]
  * 僕のプロジェクトでは、[babel-plugin-react-css-modules]が要求するので使っている
  * CSS界隈の[Babel]的な何かで、機能をプラグインで付けたり外したり出来る…程度にしか理解していない
  * [power-assertのボツになったロゴ](https://togetter.com/li/823597)と[PostCSS]のロゴはデザインの方向性が一緒だ
* [CSS Flexible Box Layout Module Level 1](https://www.w3.org/TR/css-flexbox-1/)
  * ざっと見る限り、Flexible BoxはGUIアプリケーションのレイアウトエンジンそのものだ
  * MDNにある[CSS Flexible Box Layout](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_Flexible_Box_Layout)の資料は分かり易い
  * 仕様策定中なので最新のブラウザでしか動かない

# ビルドの話
基本的な言語の話と、テストのやり方、UIの作り方について整理したので、次はビルドの話だ。

現代的な開発プロセスにおいて、Continuous Integration(CI)することは必須だ。
つまり、CLIでビルド作業を自動化しなければならないということだ。それは、Windows環境で開発するとしても変わらない。

僕が利用しているCIのSaaSは[CircleCI]と[AppVeyor]だ。

[CircleCI]はLinux環境でビルドするために使っている。特にビルドが失敗した時に[SSHログインできる](https://circleci.com/docs/ssh-build/)のでデバッグしたりログを集めたりできる。

環境がビルドのたびに綺麗になるCIサーバ上でのビルド結果と、色々なものが積み重なっているローカル環境のビルド結果は、一致しないことはよくあるからね。

特に、僕の場合はWindows環境で開発しているので、Linux環境のCircleCIとは環境の差分が発生し易い。つまり、SSHログインは極めて重要な機能だ。

[AppVeyor]はクリーンなWindows環境でビルドするために使っている。少し設定ファイルを細工するだけで、[RDP接続できる](https://www.appveyor.com/docs/how-to/rdp-to-build-worker/)のでデバッグしたりログを集めたりできる。

OSは、Windows Server 2012 R2 (x64)だけど、サーバOSとクライアントOSの差分が問題になるような使い方をしてはいない。

## 依存モジュールの管理について
Nodeにはプロジェクトに関するメタ情報が格納されているpackage.jsonに基づいて、様々なタスクを実行できるnpmというツールが付属している。

npmから実行するタスクの中で特に重要なのは依存ライブラリの自動的なダウンロードだ。

npmは非常によく出来ているけど、依存ライブラリのバージョンを固定するための`npm-shrinkwrap`が非常に使いづらい。
そもそも`npm shrinkwrap`コマンドを明示的に実行しなければ、依存ライブラリの状態を保存するファイルが作成されない。
更に、`production`モードが[事実上機能しないバグ](https://github.com/npm/npm/issues/11189)が直される気配が無い。

そこで、僕のプロジェクトでは、npmに代わるツールとして[Yarn]を使うことにした。
[Yarn]はコマンドラインオプションで明示的に指定しない限り、依存ライブラリの状態を保存するファイルが作成される。
勿論、`production`モードは適切に動作する。

[Yarn]で依存ライブラリのバージョンを固定した後は、僕の[ci-yarn-upgrade](https://github.com/taichi/ci-yarn-upgrade)を使って欲しい。
これは、依存ライブラリの状態を定期的に監視して必要があれば、package.jsonとyarn.lockを更新するプルリクエストを自動的に生成してくれる。

## タスクランナーについて
複数のプラットフォームで動作するビルドスクリプトを書くには、基本的にシェルスクリプトを使えないという制限が発生する。

僕がLinuxバイナリのある[PowerShell](https://github.com/PowerShell/PowerShell)ならできるんじゃね？とか考えるのはWindowsユーザだからだ。

プロジェクトで利用している言語はJavaScriptだし、Nodeは複数のプラットフォームで適切に動作するので、ビルドスクリプトはJavaScriptで記述するのが望ましい。

JavaScriptでビルドスクリプトを書くためのモジュールは沢山あるが、僕の考え方は単純だ。

[gulp]を使うか、使わないか？

確かに、[Grunt](http://gruntjs.com/)や[broccoli](https://github.com/broccolijs/broccoli)のようなタスクランナーはある。

これらが考慮対象になり得ない理由は簡単で、npm scriptを大きく超える利便性を提供してくれないからだ。
ただでさえ学習すべき対象が多いのでタスクランナーには、出来るだけ学習コストをかけたくない。

それでも、[gulp]が考慮対象になりえる理由はパフォーマンスだ。[gulp]はNodeのStream APIをタスク構築の中心に置いたタスクランナーで高速に動作する。

数十万行レベルのコードベースになれば、テストの実行時間が短いほうがいいのと同様に、ビルドの実行時間も出来る限り短縮することが期待される。

ところで、パフォーマンスに関する格言で僕が好きなものに「予測するな、計測せよ」というものがある。
つまり、[gulp]の採用理由が本当にパフォーマンスだけなら、その採用はコードベースが十分に大きくなり、ビルド時間が十分に長くなってからでも遅くは無いということだ。

というわけで、僕のプロジェクトでは、とりあえずnpm scriptで出来る限り頑張ることにした。

### npm scriptで使える便利なモジュール
npm scriptを使って複数のプラットフォームで動作するビルドスクリプトを書くにあたって便利なモジュールがいくつかある。

#### [cross-env](https://github.com/kentcdodds/cross-env)
これは、簡単に環境変数を設定するためのモジュールだ。

環境変数の定義方法は、LinuxとWindowsで微妙に違う。これは、その差分を吸収するために使う。
とは言え、`NODE_ENV`と`NODE_PATH`くらいしか設定するものは無いけどね。

#### [npm-run-all](https://github.com/mysticatea/npm-run-all)
これは、npm script内で他のnpm scriptをまとめて呼び出したり、複数のnpm scriptをそれぞれ別なプロセスとして並列に動かしたりできるモジュールだ。

このモジュールがあるから、npm scriptで頑張れると言っても過言ではない。

例えば、こんな使い方ができる。

```
{
  "name": "sample",
  "scripts": {
    "compile": "run-p compile:*",
    "compile:main": "cross-env NODE_ENV=production webpack --profile --config config/webpack.main.js",
    "compile:renderer": "cross-env NODE_ENV=production webpack --profile --config config/webpack.renderer.js",
  }
}
```

これを、`yarn compile` で実行すると、`compile`タスクには、`run-p compile:*`とあるので、`compile:main`と`compile:renderer`が同時に実行される。

#### [rimraf](https://github.com/isaacs/rimraf)
これは、指定したディレクトリの中にあるファイルやディレクトリの全てを消せるモジュールだ。

これも、LinuxとWindowsの差分を吸収するために使っている。

## モジュールバンドラについて
タスクランナーは使わないがモジュールバンドラは使う。JavaScriptにおけるモジュールバンドラは、Cコンパイラに対するリンカみたいなものだ。

役割と責任範囲に応じてバラバラに分割されたJavaScriptが[Babel]にコンパイルされてES5で動くように変換される。
この時点で、それぞれのファイルサイズは少々大きくなったりするが、ファイル数に変化はない。
出来たファイル群を単に結合すれば動くと思うかもしれないが、そうではない。

コンパイルしなければ動作しないファイルは他にもCSSのメタ言語がある。SassやLessといったメタ言語も同様にCSSへコンパイルする。
CSSのメタ言語はコンパイル時点でファイルが一つに結合されることが多い。

[CSS Modules]を使うなら、CSSとJavaScriptの整合性を取りながらコンパイルする必要もある。

これらのコンパイル済みリソースをブートストラップエントリとなるHTMLに統合することで、JavaScriptのアプリケーションは動作するモジュールバンドルになるのだ。

### モジュールバンドラの選び方
候補になるモジュールバンドラは[Browserify](http://browserify.org/)、[webpack]、[Rollup](http://rollupjs.org/)、[Backpack](https://github.com/palmerhq/backpack)など、
いくつかある。でも、[webpack]以外の選択肢はない。

何故なら僕のプロジェクトでは[CSS Modules]を使うからだ。

加えて、[webpack-dev-server](https://github.com/webpack/webpack-dev-server)に強く依存して実装されている[React Hot Loader](https://github.com/gaearon/react-hot-loader)を使うからだ。

[webpack2.2が正式リリースされた](https://medium.com/webpack/webpack-2-2-the-final-release-76c3d43bf144#.suepi2729)が、
[extract-text-webpack-plugin](https://github.com/webpack/extract-text-webpack-plugin/releases)のβが取れていないので、まだ使っていない。

かなり丁寧な[移行ドキュメント](https://webpack.js.org/guides/migrating/)が用意されているので、その気になればすぐに移行できるだろう。

### [webpack]関連の理解すべきモジュール
そもそも[webpack]は、かなり大きなアプリケーションで丁寧に学習する必要がある。だから、あまり多くを追加しないようにと考えている。

#### [webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)
これは[webpack]のビルド生成物を簡易的に動かすためのサーバだ。

[webpack]用の設定ファイルをもとに動作するので、[Express](http://expressjs.com/)なんかで簡易サーバ作るよりもさらに簡単にアプリケーションを動かせる。

プロキシとして動かしたりもできるなど機能は豊富なので、ドキュメントを一通り読んでおくと便利に使える。

ちなみに、ウェブブラウザは `file://` や `localhost` を特別扱いするので、ブラウザの新しい機能をふんだんに使うアプリケーションを書いている時には、ちゃんとしたホスト名を割当てて動かすこと。

#### [webpack-merge](https://github.com/survivejs/webpack-merge)
[webpack]の設定ファイルは `NODE_ENV` 毎にオブジェクトを作ることになる。
複数の環境で同一の部分と、環境ごとに違う部分を分けて記述するなら、設定オブジェクトを合成する必要がある。

その時に使えるのが、このモジュールだ。特に高度なことをしているわけではないので使わなくても良いけどね。

#### [webpack-validator](https://github.com/js-dxtools/webpack-validator)
[webpack]の設定ファイルが正しく記述されているのかを確認できるモジュールだ。

webpack2では、同等の機能が実装されているので、こういうモジュールは必要ない。

# [Electron]の話
[Electron]は複数のOSで動作するGUIアプリケーションを作るためのフレームワークだ。内部的には[Node.js]と[Chromium]が動く。

[Electron]は、Chrome由来の富豪モデルで動作する。どういうことかと言うと、スレッドが無く、そういうことをしたいなら[Node.js]のプロセスを起動する。タブというかWindow一つに一つにも、それぞれプロセスが起動する。

プロセスをどんどん起動するモデルでは、状態を共有するためにプロセス間通信するしかない。つまり、スレッド競合のようなややこしいバグは起きない。その代わり、メモリを大量に消費する。

この手のツールキットは昔から色々ある。僕がそれなりに知っているものだと、[JavaFX](http://www.oracle.com/technetwork/jp/java/javafx/overview/index.html)、[Qt](https://www.qt.io/)なんかがそうだ。

これらに対して、[Electron]が優れている点はウェブアプリケーションを書くための知識セットでGUIアプリケーションを作れることだ。

## 何故Electronに取り組むのか
僕にとっては、[VS Code]が[Electron]で実装されているというのが大きい。

僕の20代を捧げたeclipseのプラグインフレームワークを設計した[Erich Gamma](https://github.com/egamma)先生の最新作が[VS Code]だ。
十年以上の間、彼を全能なる神の一種だと考えていたが、[vscode-tslint](https://github.com/Microsoft/vscode-tslint)のダメなコードを読んで少し親近感が沸いた。

[Erich Gamma](https://github.com/egamma)先生は、アーキテクトとしては素晴らしい業績を残しているが、プログラマとしては凡人かそれ以下だ。
コードはその辺から質の悪いのをコピペしてるし、まともなユニットテストも無い。規模が小さいし、ほぼ一人で作業してるんだから、少々雑でも良いだろって？まぁ、そうだよね。

こういうことがカジュアルに見えるようになったのは、GitHub時代の素晴らしさだと思う。

話を戻すと、他に普段使いしている[Electron]で実装されているアプリケーションとしては、[GitKraken](https://www.gitkraken.com/)や、[Curse](https://www.curse.com/)、[Insomnia](https://insomnia.rest/)、[Marp](https://yhatt.github.io/marp/)がある。
SlackのWindowsクライアントも[Electron]だけど、特に使い勝手が良くないので使っていない。蛇足だけど、KindleのWindowsクライアントはQtだ。

昔はUIがOSネイティブなものとかけ離れているアプリケーションは嫌われたものだ。
最近はOSネイティブなUIとウェブアプリケーションのUIが徐々に近づいているので、[Electron]ベースのアプリケーションが放つ違和感は小さい。

[Electron]に取り組む最後の理由は、各OSで動作する実行バイナリを作るためのビルドプロセスが簡単だからだ。

[electron-builder](https://github.com/electron-userland/electron-builder)なら、package.jsonに少々設定を書いてCIサーバでちょいちょいとビルドすれば、インストーラ付きの実行バイナリがパッと出てくる。

# まとめ
これでやっと、作りたかったアプリケーションの開発に取り掛かれる。

本来なら正月休みに最初のプロトタイプが出来てるはずだったんだけども、腕が鈍ってるのか見積もりに失敗して、今やっと開発環境が整ったのだ。

余りにも沢山のことを学習したし、何かと理由を付けて切り捨てたものもある。

アプリケーションを作ってると、生みの苦しみみたいなものがあって、それから逃れるために取らなかった選択肢に意識が向いたりする。
切り捨てたものとその理由を忘れて、時間を浪費したくなるのだ。

そういう精神状態に自分がなった時に、ちょっとググるだけで、自分のエントリが出てくれば思い直せるだろう。そういう目的で、このエントリを書いた。

僕がJavaScriptによるアプリケーション開発の専門家と議論する際にも、このエントリがあれば状況認識を合わせるための基準にできるだろう。

こんなに長いエントリを最後まで読んでくれたなら、あなたには感謝の言葉しかない。ありがとうございます、そして、お疲れさまでした。

[VS Code]: https://code.visualstudio.com/
[Babel]: https://babeljs.io/
[Google Closure Compiler]: https://developers.google.com/closure/compiler/
[DefinitelyTyped]: https://github.com/DefinitelyTyped/DefinitelyTyped
[TypeScript]: https://www.typescriptlang.org/
[Flow]: https://flowtype.org/
[flow-typed]: https://github.com/flowtype/flow-typed
[ESLint]: http://eslint.org/
[AVA]: https://github.com/avajs/ava
[power-assert]: https://github.com/power-assert-js/power-assert
[Yarn]: https://yarnpkg.com/
[CircleCI]: https://circleci.com/
[AppVeyor]: https://www.appveyor.com/
[gulp]: https://github.com/gulpjs/gulp
[Grunt]: http://gruntjs.com/
[React]: https://facebook.github.io/react/
[Angular]: https://angular.io/
[Vue.js]: https://vuejs.org/
[Mithril]: http://mithril.js.org/
[CSS Modules]: https://github.com/css-modules/css-modules
[react-css-modules]: https://github.com/gajus/react-css-modules
[babel-plugin-react-css-modules]: https://github.com/gajus/babel-plugin-react-css-modules
[Flux]: https://facebook.github.io/flux/
[Redux]: https://github.com/reactjs/redux
[redux-saga]: https://github.com/redux-saga/redux-saga
[redux-observable]: https://github.com/redux-observable/redux-observable
[RxJS]: https://github.com/Reactive-Extensions/RxJS
[PostCSS]: http://postcss.org/
[webpack]: https://webpack.github.io/
[css-loader]: https://github.com/webpack/css-loader
[Electron]: http://electron.atom.io/
[Node.js]: https://nodejs.org
[Chromium]: https://www.chromium.org/Home
