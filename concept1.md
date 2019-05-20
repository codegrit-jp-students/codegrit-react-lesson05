# Reactとスタイリング

## サンプルコード

サンプルコードは以下のリポジトリにあります。

- [codegrit-react-lesson05-samples](https://github.com/codegrit-jp-students/codegrit-react-lesson05-samples)

## Reactコンポーネントとスタイリング

create-react-appを利用してアプリを作成すると、App.jsと並んでApp.cssというファイルが生成されていることに気づいたかと思います。Reactではこのように一般的にコンポーネントごとに異なるCSSファイルを作成します。このCSSファイルのことをCSSモジュールと呼びます。

Reactでのスタイリングには、このCSSモジュールを使う方法、インラインスタイルを利用する方法、そして最後にCSS in JSライブラリを利用した方法の3つがあります。ここでは、それぞれの特徴と使用のメリット・デメリットを確認していきましょう。

## インラインスタイルを利用する

レッスン2でも紹介しましたが、通常のHTMLのようにインラインスタイルを利用して、スタイリングを行うことができます。この場合以下の例のように、スタイリングの情報をオブジェクトに格納して利用します。

この時に、値の部分は数値の場合を除き文字列とすることに注意しましょう。またプロパティ名では`-`の部分は、ハイフン自体はなくしその後に続く文字を大文字にします。

例:

- font-size: fontSize
- font-weight: fontWeight

インラインスタイルを利用する場合、`:hover`や`:active`などのイベント時のスタイルを定義することが出来ず、単純なスタイル定義のみで利用出来ます。

```js
// src/InlineStylesSample.js

import React from 'react';

const InlineStylesSample = () => {

  const linkStyle = {
    color: 'red',
    fontSize: '16px',
    '&:hover': { // これは動きません。
      color: 'blue'
    }
  }

  const listStyle = {
    marginTop: '16px',
    color: 'red',
    fontSize: '16px',
  }

  return (
    <ul>
      <li style={listStyle}>
        <a style={linkStyle}>
          リンク1
        </a>
      </li>
      <li style={listStyle}>
        <a style={linkStyle}>
          リンク2
        </a>
      </li>
    </ul>
  );
}

export default InlineStylesSample;
```

またインラインスタイルは、CSSモジュールを利用する場合に比べてパフォーマンスが落ちるため、総じて言って非推奨と言える方法です。スタイルを手軽に確認したい時や、画像などで高さや幅を画面の幅によって変えたいなどの場合に使用を制限しましょう。ほとんどの場合は以下のCSSモジュールの利用で問題ないはずです。