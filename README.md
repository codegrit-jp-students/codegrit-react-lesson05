# レッスン5. スタイリングとUIコンポーネント

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

## emotionを利用する

emotionを利用するにはまずパッケージをインストール必要があります。

```bash
$ yarn add @emotion/styled @emotion/core
```

これだけで準備完了です。試しにボタンを作成してみましょう。ここまでの説明で、CSS in JSではスタイリング用のコンポーネントを作成すると説明してきましたが、emotionでは、css propsを利用する方法も使えます。これらの2つは好みの問題でどちらを使用しても問題ありません。

### css propsを利用する場合

まずは、いつものようにcreate-react-appを利用してアプリを作成しましょう。

```bash
npx create-react-app styling-test
```

App.jsを開いて、Appコンポーネントの中身を一旦消して次のように状態にします。ボタンが見えやすいようにmarginを50px入れています。

```js
import React, { Component } from 'react';

class App extends Component {
  render() {
    return (
      <main style={{ margin: 50 }}>

      </main>
    );
  }
}

export default App;
```

では、emotionを利用してボタンを作成してみましょう。まずはemotionのcoreパッケージからcssファンクションとjsxファンクションをインポートします。またjsxを利用することを示すための記述をファイルの最初に加える必要があります。

```js
/** @jsx jsx */
import { css, jsx } from '@emotion/core'
```

次にボタンのスタイルを定義します。定義はテンプレートリテラル、またはオブジェクトを利用できます。

```js
// テンプレートリテラルを利用する場合
const buttonCss = css`
  color: '#fff';
  backgroundColor: '#D9728E';
  marginBottom: '7px';
  padding: '10px 20px';
`

// オブジェクトを利用する場合
const buttonCss = css({
  color: '#fff',
  backgroundColor: '#D9728E',
  marginBottom: '7px',
  padding: '10px 20px',
})
```

定義ができたら、これを利用してみましょう。

```js
// src/EmotionButtonSample.js

/** @jsx jsx */
import { Component } from 'react';
import { css, jsx } from '@emotion/core'

const buttonCss = css({
  color: '#fff',
  backgroundColor: '#D9728E',
  marginBottom: '7px',
  padding: '10px 20px',
})

export default class extends Component {
  render() {
    return (
      <main style={{ margin: 50 }}>
        <button css={buttonCss}>Test</button>
      </main>
    );
  }
}
```

これで完成です。次の画像のようにボタンが表示されていれば成功です。

![emotion-styled-button.png](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson05%2Femotion-styled-button.png?alt=media&token=ac4c2c61-ddc2-4b68-b0a0-86da059d4de9)


### 変数を利用してスタイルを変更する。

スタイリングの中では変数を利用することもできます。

```js
const color = red

const buttonCss = css({
  color,
  backgroundColor: '#D9728E',
  marginBottom: '7px',
  padding: '10px 20px',
})
```

### Styled Componentを利用する場合

この場合は、styledファンクションをインポートします。

```js
import styled from '@emotion/styled'
```

次にButtonコンポーネントを定義します。

```js
const Button = styled.button({
  color: '#fff',
  backgroundColor: '#D9728E',
  marginBottom: '7px',
  padding: '10px 20px',
})
```

これでコンポーネントを作成できたので、最後にrenderします。

```js
import styled from '@emotion/styled'

const Button = styled.button({
  color: '#fff',
  backgroundColor: '#D9728E',
  marginBottom: '7px',
  padding: '10px 20px',
})

class App extends Component {
  render() {
    return (
      <main style={{ margin: 50 }}>
        <Button>Test</Button>
      </main>
    );
  }
}

export default App;
```

## Propsを利用してダイナミックにスタイルを変更する

Styled Componentを利用する場合には、Propsを渡すことでダイナミックにスタイルを変更することができます。最初に挙げた例を見てみましょう。

```js
// src/Button.js
import styled from '@emotion/styled'

const Button = styled.button(
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

この場合だと、typeというPropsを利用して、ボタンのタイプごとに背景を変えています。この際に、例のようにstylesという配列を最初に作成し、そのオブジェクトに順番にスタイルを追加していくと便利です。

例えば、type以外にsizeというプロパティを追加してみましょう。

```js
// src/Button.js
import styled from '@emotion/styled'

const Button = styled.button(
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
    switch (props.size) {
      case 'large':
        styles.push({ 
          padding: '20px 40px',
          fontSize: '18px',
        })
        break;
      case 'small':
        styles.push({ 
          padding: '7px 14px',
          fontSize: '13px',
        })
        break;
      default:
        break;
    }
    return styles;
  }
)
```

上記の場合だと、typeとsizeの2つのpropsを指定して、スタイルを変更出来るようになります。例えば次の例では大きいサイズのdangerボタンを利用しています。

```js
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

結果はこのようになります。

![emotion-button-sample-large.png](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson05%2Femotion-button-sample-large.png?alt=media&token=4d8af40a-4797-47f5-b5e3-8775a1a7e5dd)

## 複数のクラスを利用する

cssプロップを利用する場合は、配列を利用して、複数のスタイルを簡単に組み合わせることができます。

```js
/** @jsx jsx */
import { css, jsx } from '@emotion/core'

css_one = css({
  padding: '10px, 20px'
})

css_two = css({
  margin: '20px'
})

const compositionTest = () => (
  <div css={[css_one, css_two]}></div>
)
```

## Babel Macroまたはbabel-plugin-emotionを利用する

emotionは通常のようにインポートするだけでも利用できるのですが、以下に挙げるBabel Macro、babel-plugin-emotionのいずれかを利用することが推奨されています。

理由は以下の通りです。

- ラベルを自動でつけてくれる

ES5にBabelがトランスパイルする際にラベルを自動でつけてくれる

- ファイルを最小化してくれる

スタイリングの定義からスペースを無くして、最小のファイルサイズにしてくれる

- ソースマップを出してくれる

ソースマップとは、トランスパイルされ、最小化されたファイルを人間の目で見ても分かりやすいようにマッピングしてくれる機能です。ソースマップを利用すると`.map`というファイルが生成されます。バグが発生した際にGoogle Consoleでバグの場所を見るのに便利です。

- 他のエモーションで作られたコンポーネントにアクセス出来るようになる
詳しくは説明しませんが、気になる方は公式ドキュメントを参照ください。

[emotion公式 - Targeting another emotion component](https://emotion.sh/docs/styled#targeting-another-emotion-component)


### Babel Macroを利用する

Babel Macroを利用すると上記で説明したメリットをBabelのコンフィグファイルの設定なしで行えます。利用するのは簡単で、インポートするファイル名の最後に`/macro`をつけるだけです。

```js
import styled from '@emotion/styled/macro'
import css from '@emotion/css/macro'
```

注意点として、`@emotion/core`ではBabel Macroを利用することが出来ません。

### babel-plugin-emotion

babel-plugin-emotionを利用する場合は、まずパッケージをインストールした後にコンフィグファイルを設定する必要があります。

1. パッケージのインストール

babel用のプラグインはトランスパイル時にしか使われないので`--dev`というオプションをつけてインストールします。

```bash
$ yarn add --dev babel-plugin-emotion
```

2. コンフィグファイルの編集

.babelrcを以下のように編集します。

```
{
  "plugins": ["emotion"]
}
```

以上で設定は終わりです。Babel Macroとbabel-plugin-emotionはどちらを利用しても大丈夫ですので、好きな方を利用しましょう。


## ラベルをつける

さて、先程の例でcss propsを利用した場合のボタンをGoogle Consoleで見てみましょう。すると例えば以下のようになっていると思います。

```html
<div class="css-8z6z14"></div>
```

しかしこれでは、HTMLを見ながら修正をしたい時にぱっと見てなんの定義をしているのかが分かりません。そこでラベルを利用することで判別を行いやすくできます。(利用にはbabel-plugin-emotionを追加している必要があります。)

```js
const buttonCss = css({
  label: 'Button',
  ...,
})
```

こうすることで、以下のようにHTML上で表示されるようになります。

```html
<div class="css-8z6z14-Button"></div>
```

Styled Componentを利用している場合には、コンポーネント名が自動的にラベルとして追加されるので、css propsとは違ってlabelを追加する必要はありません。

emotionでは他にもアニメーションなど、他にも様々な機能が用意されています。こうした機能については一度に理解する必要はありませんので都度、emotionのドキュメントを確認しながら実装していきましょう。

## UIコンポーネントを利用する

さて、ここまでの説明でCSS in JSを利用してStyled Componentを作成する方法を学びました。しかし、ボタンやメニュー、グリッドなどのよく使われる機能を全て自分で実装する必要があるでしょうか？ プログラマの世界では車輪の再発明は避けるべきだと考えられています。もちろん、独自のカスタマイズの必要なケースもあるのですが、ほとんどの場合は一般的なものがあれば間に合うはずです。

そこで、よく利用されるようなパーツをまとめてコンポーネントとして利用出来るようにしたライブラリが多く出てきており、こうしたライブラリをUIコンポーネント(英語ではUI Componentsで複数形です。)と呼びます。

## 有名なUIコンポーネント

UIコンポーネントはたくさん出ているのですが、最も有名なものとしては以下が挙げられます。

- [React Bootstrap](https://react-bootstrap.github.io/)
Twitter Bootstrap4をReactコンポーネントにしたもの。

- [Ant Design](https://ant.design/)
中国アリババ社の子会社であるアントファイナンシャルによって作られたUIコンポーネント。非常に質が高く人気があるが中国製のためGitHubのイシューなどが中国語で書かれている場合もある。

- [Material UI](https://material-ui.com/)
Googleの提唱するUIデザインパターンであるマテリアルデザインに準拠したUIコンポーネント。NASAやユニクロなど大企業での採用も多い。

- [Semantic UI React](https://react.semantic-ui.com/)
Bootstrapと並んで人気のあった後発のCSSフレームワークSemantic UIをReactコンポーネントにしたもの。Netflix、Amazonなど名だたる大企業で利用されている。

今回は、既にHTML/CSSコースから利用しており親しみのあるBootstrapを紹介していきます。

## React Bootstrapをインストールする

React Bootstrapを利用するには、以下の2つのことが必要です。

1. React Bootstrapをインストールする
2. BootstrapのCSSをインポートする。

まずはReact Bootstrapをインストールしましょう。

```bash
$ yarn add react-bootstrap
```

次に、CSSをダウンロードし、`bootstrap.min.css`を`src/styles/`フォルダ下に加えましょう。

[Bootstrap - Download](https://getbootstrap.com/docs/4.3/getting-started/download/)

ファイルを加えたら、index.js内にこのCSSをインポートします。

```js
// src/index.js
import './styles/bootstrap.min.css'
```

これで準備は完了です。

## React Bootstrapを利用する

Bootstrapの用意しているコンポーネントは多くあるので、今回のレッスンではグリッド、ボタンに絞って紹介していきます。Bootstrapではご存知の通りこれ以外にも様々なコンポーネントが用意されています。必要な場合はドキュメントを参照しながら実装を行いましょう。

### グリッドを利用する

通常のHTMLではBootstrapのグリッドは以下のように書きました。

```html
<div class="container">
  <div class="row">
    <div class="col-lg-4 col-md-8 col-sm-12">
    </div>
    <div class="col-lg-4 col-md-4 col-sm-12">
    </div>
    <div class="col-lg-4 col-md-12 col-sm-12">
    </div>
  </div>
</div>
```

これと全く同じものをReact Bootstrapで書くと以下のようになります。

```js
import { Container, Row, Col } from 'react-bootstrap'

<Container>
  <Row>
    <Col lg={4} md={8} sm={12} />
    <Col lg={4} md={4} sm={12} />
    <Col lg={4} md={12} sm={12} />
  </Row>
</Container>
```

すると以下の画像のようにグリッドが出来ます。(分かりやすいように文字を入れています。)

![react-bootstrap-grid-sample.gif](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson05%2Freact-bootstrap-grid-sample.gif?alt=media&token=44627f40-dc0c-4c92-934c-a654c104e217)

ほぼ同じ感覚で書けることが分かるでしょうか？ また、offsetを利用することも出来ます。この場合は以下のようにして書きます。

```js
<Col md={{ span: 4, offset: 4 }}/>
```

### ボタンを利用する

ボタンは以下のようにして利用します。variantプロップを変更することで様々な色を利用できます。

```js
import { Button } from 'react-bootstrap'

// Primaryボタン
<Button variant="primary">Primary</Button>
// Dangerボタン
<Button variant="danger">Danger</Button>
// infoボタン
<Button variant="info">Info</Button>
```

また`outline-`をつけたすことで、バックグラウンドカラーなしのボタンも作成出来ます。

```js
import { Button } from 'react-bootstrap'

// Primaryボタン
<Button variant="outline-primary">Primary</Button>
// Dangerボタン
<Button variant="outline-danger">Danger</Button>
// infoボタン
<Button variant="outline-info">Info</Button>
```

## 更に学ぼう

- [emotion公式(英語)](https://emotion.sh/docs/introduction)
- [ReactBootstrap公式(英語)](https://react-bootstrap.github.io/)