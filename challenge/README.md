# チャレンジ5

## 目的

- emotionを利用してCSSを定義する
- emotionの利用方法に慣れる

## チャレンジの取り組み方

1. スターターファイルのリポジトリを、フォークして自分のGitHubアカウントにリポジトリを作りましょう。
2. マイルストーンごとに要件に合うようにファイルを編集していきます。
3. 分からない部分があれば、テキストを復習して、再度チャレンジしてみましょう。
4. 再チャレンジしてしばらく考えても分からない場合はチャットでメンターに質問しましょう。
5. 完了したら、GitHubリポジトリをメンターにシェアしてください。(質問時に既にシェアしている場合は、レビューを直接依頼しましょう。)
6. メンターから課題レビューが届きます。
7. ビデオチャットの際は、分からない点を更に突っ込んで聞いたり、より良い書き方を聞いてみましょう。

## 概要

このチャレンジでは、チャレンジ4で作成したインスタカード風コンポーネントにemotionを導入していきます。

## 完成イメージ

完成イメージはチャレンジ4と同一です。

![react_ch05_image](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson04%2Fchallenge%2Fcodegrit-react-ch04-image.gif?alt=media&token=af581e34-da00-45ff-a31c-5da0079f035a)

## スターターファイル

- [codegrit-jp-students/react-ch05-starter](https://github.com/codegrit-jp-students/codegrit-react-ch05-starter)

上記のURL先のリポジトリをフォークして、コードを書き進めて行きましょう。

## マイルストーン１

全てのCSSに対してemotionを適用していきましょう。

CSSの定義をJSX用に変換するには以下のページを利用すると便利です。Formatというチェックボックスにチェックを入れて見やすくして使いましょう。

- [CSS to React](https://staxmanade.com/CssToReact/)

### 要件

- index.cssをindex.scssへと変更しましょう。index.js内でのインポート内容も変更することを忘れないでください。

- index.cssの内容を参考に全てのコンポーネントにemotionを適用してください。

StyledComponentと、css propsをうまく使い分けましょう。必要があれば、テーマpropsを、子コンポーネントに渡していってください。css propsを利用する場合は必ずラベルをつけてください。

- Babel Macroまたはbabel-plugin-emotionを利用する

上記のいずれかを導入してください。

- よく使うStyledComponent用のフォルダとファイルを作成する

`.flex-container`というCSS定義については、src直下にstyledというフォルダを作り、その中に`flexboxUtils.js`というファイルを作りその中にいれましょう。名称はulを扱うので`FlexListContainer`という名前にします。

その際に、`direction`というpropsが与えられた場合、それに応じて`flex-direction`を変更するようにしましょう。

## 評価

課題の後、以下の２つについてメンターにフィードバックをお願いします。

1. 要件のカバー度: 1.全く出来なかった 2.ほとんど出来なかった 3. 半分ほどは出来た 4.8割ほどは出来た 5. 全部出来た
2. 難易度: 1. とても難しかった 2. 難しかった 3. ちょうど良かった 4. 簡単だった 5. とても簡単だった
