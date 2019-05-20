# CSSモジュール

## CSSモジュールを利用する

CSSモジュールを利用した方法は、既にcreate-react-appで生成されたアプリの中でご覧になったかと思います。この方法は基本的にコンポーネントと同一名のファイルをcssという拡張子で保存します。保存したCSSはコンポーネントのファイル内でインポートすれば利用できます。

また、stylesディレクトリを別で作成し、ここに既存のCSSファイルや共通でCSSを置くような使い方も一般的です。

### CSSモジュールのメリット

CSSモジュールを利用することの最大のメリットは2つあります。

1. 名前のコンフリクト(混乱)を回避しやすい。
2. スタイルの管理が行いやすい。

通常のCSSでは、プロジェクトの規模が大きくなってくると、異なるスタイルを適用したい場合でも、既に定義されている名前を使用してしまいスタイルが崩れてしまうということが起こりがちでした。

そのため、既にHTML/CSSコースでも学んだ通りオブジェクト指向CSSという方法論やBEMという方法論を利用して、名前の重複を避けてスタイル定義をする方法が生み出されました。

CSSモジュールであれば、コンポーネントごとに固有のCSSを定義出来るため、共通するCSSの定義にさえ気をつければコンフリクトを引き起こす可能性を大幅に減らすことができます。

また、コンポーネントごとにCSSが定義されていれば、通常のCSSと違い、どこにCSSの定義がされているのかが一目瞭然です。

### CSSモジュールを利用する。

CSSモジュールを利用する方法は２つあります。1つは通常どおり例えば`index.css`のようなファイルを作りグローバルスタイルとしてインポートする方法です。

もう一つは、`CssModuleSample.module.css`のように`module.css`という2つの拡張子を利用する方法です。2つ目の方法を取った場合は、名前をつけてCSSをインポートすることが出来ます。

以下の例を見てみましょう。

```css
/* src/index.css */
* {
  box-sizing: border-box;
}

.bodyWrap {
  margin: 40px;
}
```

```css
/* src/CssModuleSample.module.css */

.wrap {
  padding: 20px;
  border: 1px solid #999;
}
```

```js
// src/CssModuleSample.js

import React from 'react'
import './index.css'; // グローバルスタイルとしてインポート
import Css from './CssModuleSample.module.css'; // 名前をつけてインポート

export default class extends React.Component {

  render() {
    console.log(Css);
    return (
      <body className="bodyWrap">
        {/* 以下では名前をつけてインポートしたCSSを利用しています。 */}
        <main className={Css.wrap}>
          ...省略
        </main>
      </body>
    );
  }
}
```

結果、以下の画像のように表示されます。両方のCSSの内容が反映されていますね。

![css-module-sample](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson05%2Fcss-module-sample.png?alt=media&token=309ff138-8f9f-4ccc-936a-e79eb2eedd26)

上記で`bodyWrap`の部分は`index.css`のものを使い、`appCss.wrap`の部分はに`CssModuleSample.module.css`定義されたスタイルを使っています。

## Sassを利用する

さて、これまでSassを利用してきた方であればCSSモジュールでもSassを利用したいと思うのが自然かと思います。create-react-appではconfigファイルにSassをトランスパイルするための設定を加えることでこれを実現することができます。

Sassを利用するのはcreate-react-appでは簡単です。`node-sass`ライブラリをインストールするのみで完了します。

```bash
$ yarn add node-sass
```