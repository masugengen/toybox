# my coding Ruhru

# 設計ルール

## HTML設計について
本件ではclassの命名ルールに`MindBEMding`を採用します。

### class命名の基本型
classの命名の基本形は以下のとおりです。
```
.Block
.Block__Element
.Block--Modifier
```
命名は必ず`Block`から始まり、接頭辞にBlock名を持たずに`Element,Modifier`が命名されることはありません。

また、同一の要素に複数のBlock名があてられることはありません。

`Modifier`は追加でユニークなスタイルをあてる場合にのみ使用し、`Modifier`が単独で使用されることはありません。

Modifierに付与によるスタイルの差分が大きい場合は、モジュール単位の最上位にあたる`Block`に追加で`Modifier`を命名します。

Modifierに付与によるスタイルの差分が小さい場合は、スタイルの変更が行われる`Element`に追加で`Modifier`を命名します。

### 間違った命名パターン
```
・Blockを複数付与している
<div class="box box-2">

・Elementの命名にBlock__が存在していない
<div class="inner">

・Modifierを単独で使用している
<div class="box--shadow">
```

### 正しい命名パターン
```
・Blockが単一で使用されている
<div class="box">

・Elementの命名にBlock__が存在している
<div class="box__inner">

・ModifierとBlockを同時に使用している
<div class="box box--shadow">

```

### blockの命名について
Block名は予め決めた以下のような分類で命名します。  
また、`modifire`の命名に関して連番の場合は`01`を省き`02`から始めることとします。

| 分類 | 命名 | modifire系 |
| ------ | ------ | ------ |
| 見出し | hdg | hdg-02 |
| テキスト | text | text-02 |
| リンク | link | link-02 |
| リスト | list | list-02 |
| テーブル | tbl | tbl-02 |

### elementの命名について
Elementの命名は状態や細かい内容といったユニークな文字列は含めず、予め決めた以下の分類で命名します。  
ただし、Blockの中に内包するElementが多い、Elementの内容が決め打たれている、などの場合はユニークなElementの命名を可とします。

| Element | 使用用途 |
| ------ | ------ |
| inner, head, body,foot, col, content | Block内のラッパーElementとして使用します。 |
| hdg, lead, txt,sub, bold, sup | Block内の文章レベルElementとして使用します。 |
| link, img, btn,hook, icon, cap | Block内の特定の役割を果たすElementとして使用します。 |

### elementの命名の分類について

| Element | 使用例 |
| ------ | ------ |
| inner | Blockの内側を何らかの理由でラップする場合に使用します。<br>原則としてBlockの直下の子要素としてのみ使用します。 |
| content | ラッパーElementの中でElementをラップする場合に使用します。 |
| head, body, foot | Block内でラップしたElementに文脈の順序がある場合に使用します。<br>bodyが使用される場合には必ず同階層にheadが存在し、footが使用される場合には必ず同階層にbodyが存在します。 |
| col | Block内でラップしたElementに文脈の順序がない場合に使用します。 |
| hdg, lead, txt, sub | Element内のテキストのレベル感に応じて使用します。<br>hdgとleadの使い分けに厳密性はありません。 |
| bold, sup | テキストElementの中で特定の文字列の見た目を変更したい場合に使用します。 |

### Modifireの命名
Modifierはそれが何であるかを示し、原則としてBlockにのみ付与します。

#### 間違った使用例

```
・Modifierが何を指し示しているかが理解できない
<p class="txt txt--pattern-01">
```

#### 正しい使用例

```
・Modifierが何を指し示しているかが理解できる
<p class="txt txt--red">
```

### Modifireなのか、別のBlockなのか
Modifierは機能拡張の意図として使用することが原則であるため、Modifierのスタイルが一定以上（感覚値ですが）のBlockのスタイルを打消している場合は、Modifierではなく新規のモジュールの作成を検討ください。

## CSS設計について
本件におけるCSS設計は`scss`を採用します。

### リセット系CSS
本件では[ress](https://github.com/filipelinhares/ress)を採用します。

### ファイルの分類について

| フォルダ | 用途 | 作業ブランチ |
| ------ | ------ | ------ |
| core | サイト全体に影響する原則的なscssファイルを格納する | develop |
| lyt | `class="section"`系やランドマーク毎のscssファイルを格納する |  feature |
| mixin | mixinファイルを格納する<br>mixinファイルは適宜増やしても良い | develop |
| modules | モジュールごとのscssファイルを格納する | feature |
| utility | 調整用のscssファイルを格納する | feature |
| plugins | ライブラリやプラグイン系のscssファイルを格納する | feature |

### 詳細度について

本件ではcssの詳細度を最低限に抑えるため、下記の様な記述方法を採用します。

下記の記述方法で出力されたCSSは、ベースとなるBlock及びElementが最も弱い詳細度を持ち、後続で拡張されたElementはModifierクラスをひとつだけ親に持つ形のインデントが付与される為、最低限の詳細度でスタイルを上書きすることが出来ます。
```
// ベースとなるBlockのスタイルはscssの上部に記述する
.block {
    display: flex;
    flex-wrap: wrap;

    &__element-01 {
        background: #ccc;
    }

    &__element-02 {
        font-size: 1.6rem;
    }
}

// Elementの拡張を行う際は、ベースとなるBlockの下に記述する
// また、拡張するElementではなく親のBlockにModifierを付与する
.block--modifier-01 {
    .block__element-01 {
        background-color: #eee;
    }
}

.block--modifier-02 {
    .block__element-02 {
        font-size: 2rem;
    }
}
```

## ファイル管理について
階層などについては文書仕様策定書に準拠します。

本件においては下記となります。  
よって、scssの書き出し先は基本的に/common/css/style.cssとなります。

| 共通リソース格納先 | common |
| cssファイル | css |
| jsファイル | js |
| 画像ファイル | img |
| その他 | inc,pdf |

## 画像管理
### 命名ルール
```
[ページ名]-[分類]-[説明(必要に応じて付与)]-[連番].拡張子
```

### 分類
| 分類 | 命名 |
| --- | --- |
| 画像/図版 | img |
| ロゴ | logo |
| アイコン | icn |

実装時の解釈によるブレを避けるためにパターン数は少なくします。

----
