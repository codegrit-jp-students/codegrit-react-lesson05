# CSS in JS

## "CSS in JS"系のライブラリを利用する

さて、最後に多くの議論を呼んでいる方法として"CSS in JS"と呼ばれる方法があります。CSSモジュールを利用した方法では、CSSファイルを別のファイルとして作成し、読み込むという方法でした。利用方法もは通常のHTMLとほぼ同じでした。

これに対して"CSS in JS"という方法では、`Styled Component`と呼ばれる、CSS定義のみを行ったコンポーネントを作成します。最も人気の高いライブラリはその名の通り、`Styled Component`というものなのですが、この領域はオープンソースの開発が盛んで、Styled Componentの特徴を活かしながら他の機能を加えたものなど多くのライブラリが存在します。

例えば、CodeGritではStyled Componentと人気を二分しているライブラリである**emotion**を利用しています。

### CSS in JSを利用するメリット

- スタイルの再利用が行いやすい
- propsを利用して、ダイナミックにスタイルを変更出来る。
- テーマごとに複数のスタイルを同時に変更することが出来る。

CSS in JSを利用するメリットとして、Propsを利用してスタイルを変更することが簡単に出来ることが挙げられます。

例えばですが、ボタンを考えてみましょう。dangerボタンは`background-color: red`、infoボタンは`background-color: blue`を使うことを考えます。この時にCSSモジュールを使う場合は通常のCSSファイルと同様に以下のように定義します。(SASSを利用しています。)

```scss
// common.scss
.btn {
  padding: 10px 20px;
  color: white;
  font-size: 15px;
  background-color: gray;
  border: none;
  &:hover {
    opacity: 0.8;
  }
  .btn-danger {
    background-color: red;
  }
  .btn-info {
    background-color: blue:
  }
}
```

```js
import React from 'react'
import 'common.css'

const btnEx = (props) => {
  let btnClass = 'btn'
  if (props.btnType) {
    btnPart = `btn ${btnType}`
  }
  return (
    <button className={btnClass}>
      ボタン
    </button>
  );
}

export default btnEx;
```

これを、emotionを使って書くと以下のようになります。

```js
// src/Button.js

import styled from '@emotion/styled'

export const Button = styled.button(
  {
    padding: '10px 20px',
    color: 'white',
    fontSize: '15px',
    backgroundColor: 'gray',
    border: 'none',
    '&:hover': {
      opacity: 0.8,
    },
  },
  props => {
    let styles = []
    switch (props.type) {
      case 'danger':
        styles.push({ backgroundColor: 'red' })
        break;
      case 'info':
        styles.push({ backgroundColor: 'blue' })
        break;
      default:
        styles.push({ backgroundColor: 'gray' })
    }
    return styles;
  }
)
```

```js
// src/EmotionButtonSample.js

import React from 'react';
import { Button } from './Button'

const EmotionButtonSample = () => {
  return (
    <div>
      <p>infoスタイルのボタンを表示します。</p>
      <Button type="info">
        ボタン
      </Button>
    </div>
  );
}

export default EmotionButtonSample;
```

結果は次の画像のようになります。

![emotion-button-sample-normal.png](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson05%2Femotion-button-sample-normal.png?alt=media&token=aa679b4b-e3c8-4d8f-829c-3d5bd2f29440)

このように、コンポーネントに渡すPropsによってスタイルを変えることが簡単にできるため、CSSモジュールを利用するのに比べて、メインのコンポーネントを少しすっきりさせることができました。

また上記にもあげましたが、CSS in JSではThemeProviderのようなテーマ利用のためのコンポーネントが含まれており、それを利用してサイトのテーマを簡単に変えることができたりします。

例えばですが、メディア系のサイトであれば、夜になったときに目に優しいように背景色を黒色に出来るナイトモードをつけることがあるでしょ。こうした時には、CSS in JSのほうが便利です。

## CSS in JSを利用するデメリット

- Reactを利用していないサイトへ流用するのが難しい
- コンポーネント数が増える

CSS in JSでは、スタイルをコンポーネントに格納するため、どうしてもコンポーネントの数が増えます。これらのコンポーネントを管理するのは数が増えると大変になってきます。

またスタイルの定義をReactコンポーネントとして行っているため、Reactではなく他のライブラリを使いたい場合や、同じスタイルをWordpressなどのサイトにも使いたいという場合などには手間が発生します。

## CSS in JSとCSSモジュールを使い分ける

CSS in JSとCSSモジュールはどちらか一方だけを使わないといけないわけではなく、同じアプリで両方つかっても全く問題ありません。

例えば、Bootstrapなどの外部ライブラリをcommon.cssで読み込んで利用する。 ボタンなどPropsによる変更が多いパーツのみCSS in JSを利用する。など自分の好みによって使い分けていくのがよいと思います。

