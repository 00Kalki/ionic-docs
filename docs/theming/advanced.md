---
sidebar_label: 高度なカスタマイズ
initialTab: 'preview'
inlineHtmlPreviews: true
---

import CodeColor from '@components/page/theming/CodeColor';

# 高度なカスタマイズ

CSSベースのテーマ設定では、CSSファイルをロードするか、いくつかのCSSプロパティ値を変更することで、アプリの配色をすばやくカスタマイズできます。

## `theme-color` Meta

The `theme-color` value for a meta tag indicates a color that browsers can use to customize the display of a page or of the surrounding interface. This kind of meta tag can also accept media queries which allow developers to set the theme color for both light and dark modes.

The `content` value for the `theme-color` meta must contain a valid <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/color_value" target="_blank" rel="noopener noreferrer">CSS Color</a> and cannot contain CSS Variables.

:::note
The `theme-color` meta controls the interface theme when running in a web browser or as a PWA and has no effect when an app is deployed using Capacitor or Cordova. If you are looking to customize the area under the status bar, we recommend using the [Capacitor Status Bar Plugin](https://capacitorjs.com/docs/apis/status-bar).
:::

The example below demonstrates how to use `theme-color` to style the browser interface on iOS 15.

```html
<meta name="theme-color" media="(prefers-color-scheme: light)" content="#3880ff" />
<meta name="theme-color" media="(prefers-color-scheme: dark)" content="#eb445a" />
```

| Light Mode                                                                             | Dark Mode                                                                            |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| ![Application with theme-color meta in light mode](/img/theming/theme-color-light.png) | ![Application with theme-color meta in dark mode](/img/theming/theme-color-dark.png) |

The `theme-color` meta can also be used to customize the toolbar in Safari on macOS Monterey or newer.

Safari on iOS 15 and macOS will automatically determine an appropriate theme color to use, but adding this meta tag is useful if you need more control over the theme.

There is a small subset of colors that browsers will not use as they interfere with the browser interface. For example, setting `content="red"` will not work in Safari on macOS because that color interferes with the red close button in the toolbar. If you run into this situation, try altering your color selection slightly.

:::note
Browsers will prefer the `theme-color` meta over `theme` in `manifest.json` if both are present.
:::

For more information, see the <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name/theme-color" target="_blank" rel="noopener noreferrer">MDN theme-color documentation</a>.

## Global Variables

While the application and stepped variables in the themes section are useful for changing the colors of an application, often times there is a need for variables that are used in multiple components. The following variables are shared across components to change global padding settings and more.

### Application Variables

| Name                      | Description                                                                                |
| ------------------------- | ------------------------------------------------------------------------------------------ |
| `--ion-font-family`       | Font family of the app                                                                     |
| `--ion-statusbar-padding` | Statusbar padding top of the app                                                           |
| `--ion-safe-area-top`     | Adjust the safe area inset top of the app                                                  |
| `--ion-safe-area-right`   | Adjust the safe area inset right of the app                                                |
| `--ion-safe-area-bottom`  | Adjust the safe area inset bottom of the app                                               |
| `--ion-safe-area-left`    | Adjust the safe area inset left of the app                                                 |
| `--ion-margin`            | Adjust the margin of the [Margin attributes](../layout/css-utilities.md#element-margin)    |
| `--ion-padding`           | Adjust the padding of the [Padding attributes](../layout/css-utilities.md#element-padding) |

### Grid Variables

| Name                           | Description                                    |
| ------------------------------ | ---------------------------------------------- |
| `--ion-grid-columns`           | Number of columns in the grid                  |
| `--ion-grid-padding-xs`        | Padding of the grid for xs breakpoints         |
| `--ion-grid-padding-sm`        | Padding of the grid for sm breakpoints         |
| `--ion-grid-padding-md`        | Padding of the grid for md breakpoints         |
| `--ion-grid-padding-lg`        | Padding of the grid for lg breakpoints         |
| `--ion-grid-padding-xl`        | Padding of the grid for xl breakpoints         |
| `--ion-grid-column-padding-xs` | Padding of the grid columns for xs breakpoints |
| `--ion-grid-column-padding-sm` | Padding of the grid columns for sm breakpoints |
| `--ion-grid-column-padding-md` | Padding of the grid columns for md breakpoints |
| `--ion-grid-column-padding-lg` | Padding of the grid columns for lg breakpoints |
| `--ion-grid-column-padding-xl` | Padding of the grid columns for xl breakpoints |

## Known Limitations with Variables

### The Alpha Problem

16進数カラーのアルファ使用については、まだ完全な<a href="https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#Browser_compatibility" target="_blank">ブラウザサポート</a>はありません。<a href="https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#rgb()_and_rgba()" target="_blank">rgba()</a> 関数は、`R, G, B, A` (Red, Green, Blue, Alpha) のフォーマットのみ利用可能です。次のコードは、`rgba()`　に受け渡される正しい値と間違った値の例を示しています。

```css
/* These examples use the same color: blueviolet. */
.broken {
  --violet: #8a2be2;

  /* rgba(#8a2be2, .5) */
  color: rgba(var(--violet), 0.5); /* ERROR! Doesn't support hex. */
}

.working {
  --violet-rgb: 138, 43, 226;

  /* rgba(138, 43, 226, .5) */
  color: rgba(var(--violet-rgb), 0.5); /* WORKS! */
}
```

:::note
See the [CSS Variables](css-variables.md) section for more information on how to get and set CSS variables.
:::

Ionicはいくつかのコンポーネントで不透明度（アルファ）を​​持つ色を使用します。これが機能するためには、それらのプロパティはRGBフォーマットで提供されなければなりません。末尾にバリエーションがあるプロパティのいずれかを変更する場合、 `-rgb` で終わる括弧なしのカンマ区切り形式でも提供されることが重要です。以下は、テキストと背景色を変更するための例です。

```css
:root {
  /* These examples use the same color: sienna. */
  --ion-text-color: #a0522d;
  --ion-text-color-rgb: 160, 82, 45;

  /* These examples use the same color: lightsteelblue. */
  --ion-background-color: #b0c4de;
  --ion-background-color-rgb: 176, 196, 222;
}
```

RGB形式の色はhexプロパティとまったく同じ色ですが、現在は `rgba()` で使用できることに注意してください。例えば、`--ion-text-color-rgb` は以下のように利用できます。

```css
body {
  color: rgba(var(--ion-text-color-rgb), 0.25);
}
```

### Variables in Media Queries

[メディアクエリ](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries) のCSS変数は現在サポートされていませんが、この問題を解決する[custom media queries](https://drafts.csswg.org/mediaqueries-5/#custom-mq) と [custom environment variables](https://drafts.csswg.org/css-env-1/)を追加するためのオープンドラフトがあります。ただし、現在のサポート状態では、次の機能は動作しません。

```css
:root {
  --breakpoint: 600px;
}

@media (min-width: var(--breakpoint)) {
  /* Doesn't work :( */
}
```

### CSSカラー変数の変更

Sassの組み込み関数を使用して簡単に色を変更することは可能ですが、現在のところCSS変数で設定された色を変更するのはそれほど簡単ではありません。これは、CSSで [RGB](https://developer.mozilla.org/en-US/docs/Glossary/RGB) or [HSL](https://en.wikipedia.org/wiki/HSL_and_HSV) チャネルまたはHSLチャネルを分割してそれぞれの値を変更することで実現できますが、複雑で機能が不足しています。

正確に説明します。基本的に、SassなどのCSSプリプロセッサを使用すると、関数を使用して単一の色を操作できます。たとえば、Sassには次のカラーを作成できます:

```scss
// Background color, shade, and tint
$background: #3880ff;
$background-shade: mix(#000, $background, 12%);
$background-tint: mix(#fff, $background, 10%);

// Text color, darker and lighter
$text: #444;
$text-darker: darken($text, 15);
$text-lighter: lighten($text, 15);
```

After running through the Sass compiler, the colors will have the following values:

| Variable            | Value                                          |
| ------------------- | ---------------------------------------------- |
| `$background`       | <CodeColor color="#3880ff">#3880ff</CodeColor> |
| `$background-shade` | <CodeColor color="#3171e0">#3171e0</CodeColor> |
| `$background-tint`  | <CodeColor color="#4c8dff">#4c8dff</CodeColor> |
| `$text`             | <CodeColor color="#444444">#444444</CodeColor> |
| `$text-darker`      | <CodeColor color="#1e1e1e">#1e1e1e</CodeColor> |
| `$text-lighter`     | <CodeColor color="#6a6a6a">#6a6a6a</CodeColor> |

ただし、CSS変数は実行時に設定でき、より動的であるため、現時点では単純な関数を使用して操作することはできません。

これは通常は問題にはなりませんが、アプリケーションに動的なテーマカラーの設定が必要な場合は問題になります。Ionicでは、これが[各色にバリエーションがある](colors.md#layered-colors)理由であり、テーマ設定に[stepped colors](themes.md#stepped-colors)が必要な理由でもあります。

これを可能にする[color modification proposals](https://github.com/w3c/csswg-drafts/issues/3187)を議論している草案とIssuesはこちらからご覧いただけます。
