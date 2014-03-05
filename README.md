# Readme

たたき台

## コーディング規約
### js

- ディレクトリ
    - 機能（`view`,`net`,`model`,`controller`,`router`...)毎にそれぞれ適当なディレクトリ以下に配置していく
    - ファイル数が複数になりそうなモジュール（クラス）に関しては、そのモジュール（クラス）でさらにディレクトリを切る（ex. `/src/js/view/button/`)
- ファイル名
    - lowerCamelCaseとする
    - モジュール（クラス）でディレクトリをきる場合、ディレクトリ名と被る部分はファイル名に付けない(ex. `/src/js/view/button/buttonActionButton.js` -> `/src/js/view/button/actionButton.js`)
- ライブラリ・プラグイン
    - 必要に応じて
- Namespace
    - グローバル直下は`namespace`
    - `namespace.role.ClassName.method`（ex. `namespace.view.Modal.init`)
- 変数・クラス・メソッド命名規則
    - クラス名は`UpperCamelCase`
    - メソッド名は`lowerCamelCase`
    - 変数名は`lowerCamelCase`
    - 定数はすべて大文字で、単語の連結には`_`を用いる (ex. `CONST_NAME`)
    - クラスのプライベートメソッドには接頭辞`_`をつける
    - 役割がわかりにくくなる場合は単語を省略したりしない
- ブロック（Import, Constant, Class, Export…)ごとにコメントで区切る
- コンソールログ
    - 適切にコンソールログを出力するようにする
    - `log`, `error`, `info`
    - デフォルトの`console`は使わず、ラッパー関数の`hooop.util.Log`を利用する(prodで出力しないようにする予定）
- その他
    - main.jsには初期化処理のみ書く
    - ルーティング、ページごとの処理は`router`以下にまとめる
    - DOMアニメーションなどはそれ単体でメソッド化しておく（テストしにくいので）
    - ロジックはテスト設計しつつ組み立てること


### css

- ディレクトリ・ファイル名
    - `base`以下には原則としてファイルは追加せず、現在あるものを更新していく
        - `function` : `function`を定義する。面倒な計算など。
        - `mixin` : `include`して使う`mixin`を定義する。よく使うパターンはとりあえず`mixin`化。
        - `value` : グローバル変数を定義する。モジュールごとの変数はここに入れない。
        - `helper` : `placeholder`として使うスタイルをいれる。`%placeholder`
        - `reset` : 要素の基本スタイルを記述。原則として変更しない。
    - `parts`以下にはモジュールのスタイルをいれる
        - モジュールごとのディレクトリをきる
        - モジュールディレクトリ以下で、変数を使う場合は`_value.scss`を作っておく
        - ファイル名にはモジュール名は含めず、分割単位の役割をファイル名にする
    - `page`以下にはページごとのレイアウトや、ブロック単位のレイアウトをいれる
        - ページごと・ブロックごとにディレクトリを切る（ex /sass/page/pageName/blockName/_role.scss)
    - ファイル名は機能名のみにして、属しているブロックやモジュール名はいれない（ディレクトリ構造で表現する）
- 基本事項
    - idにはスタイルを当ててはいけない
    - マルチクラスを基本とする
    - ネストは3階層までとする。ただし、擬似セレクタなどでやむを得ず深く書かざるをえない場合はこの限りではない）
    - インデントは半角スペース4個
    - 値が0になるものは単位を付けない
    - 画像パスはcompassの `image-url`を使う
    - スプライトの生成はcompassを使う
    - IE8用に、CSS3PIEを利用する
    - `z-index`など、根拠がわかりにくくなりやすいものは基準になる値を変数化して意味を明示的にする
    - 記述の順序は `extend` 直書き `include`の順序
    - プロパティの順序は
        1. `position`, `float`
        2. `display`
        3. `width`, `height`
        4. `margin`
        5. `padding`
        6. ブロックスタイル（`border`,`background`など）
        7. インラインスタイル（`color`,`font`など）
- 命名規則
    - 基本的なルールは `モジュール名` + `-` + `機能名`
    - グローバルモジュールは `global`
    - 状態によって変化するものは、状態を記述するクラスを追記し、状態のクラスは`is`を接頭辞としてつける
    - ページ種類は接頭辞`page`をつける
    - 接頭辞を付ける場合など、それで一つの意味になる場合は `_` で連結する（ex. `is_current`、`module_name`、`page_type`）
    - `-`の数は多くとも2個までにする。それ以上になる場合はクラスを分けられるはず。
    - 変数 : `$module-valueName`
    - クラス名 : `.module_name-parts_name.is_condition`

        
### jade
- ディレクトリ
    - 基本的には実際の表示用のURLを同じディレクトリ構成
    - それとは別に、`include`や`mixin`などをまとめる
    - `include`: includeして使うファイル群をいれる。種類ごとのディレクトリをきる。引数のない固定のテンプレートのような感じ。
    - `mixin`: mixinとしてつかうファイル群をいれる。種類ごとにディレクトリをきる。引数を与えるテンプレートのような感じ。
    - `layout`: extendして使うファイルを入れる。
    - `sample`: jsやcssのサンプルをいれる。
- ファイル名
    - `_`がついた場合、`.html`ファイルが生成されない
    - それ以外はそのままのファイル名で`.html`が生成される
- 基本事項
    - `mixin`や`extend`を使う場合は、その`.jade`ファイルからの相対パスで`mixin`などが記述されたファイルを指定する
    - 基本的に`/jade/layout/_application`を`extend`する
    - `block`の中身をまるごと書き換える場合は `block blockName`
    - `block`の後ろに追記する場合は `append blockName`
    - `block`の前に追記する場合は `prepend blockName`
    - モジュールは`mixin`化する
    - 組み込み時、サーバサイド出力の場合はそのように何が入るかの情報をコメントで書く
- クラスルール
    - cssの項目を参照
    - jsからアクセスするためにつけるidやclassには、`js-`接頭辞をつける
