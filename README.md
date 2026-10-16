# Horror Countdown Timer

VHS風の背景映像と画像数字を使用した、ホラー演出付きのカウントダウンタイマーです。

HTML、CSS、JavaScriptのみで動作し、カウント開始・中止・終了時の効果音、1秒ごとのティック音、ミリ秒表示、グリッチ、走査線、ノイズ、画面反転などの演出を備えています。

## 現在作業中

Release

## 主な機能

- 画像による数字表示
- `分:秒.ミリ秒`形式のカウントダウン
- 小数第3位、1ミリ秒単位まで表示
- クリックまたはスペースキーによる開始・中止
- Tキーによる制限時間の設定
- 開始時、中止時、終了時の効果音
- 1秒ごとのティック音
- MP4によるVHS風の背景映像
- 走査線、ノイズ、トラッキングライン、周辺減光
- ランダムな数字グリッチ
- 残り30秒からの警告演出
- 終了時の暗めの色反転演出
- カウントダウン中は数字以外のUIを非表示
- FHD環境を想定した大型表示

## 操作方法

| 操作 | 動作 |
| --- | --- |
| 画面をクリック | 停止中は開始、カウント中は中止 |
| スペースキー | 停止中は開始、カウント中は中止 |
| Tキー | 制限時間を設定 |

### 時間の入力形式

Tキーを押すと、制限時間を入力できます。

```text
90
```

90秒として設定され、表示は`01:30.000`になります。

```text
1:30
```

1分30秒として設定されます。

```text
01:30.125
```

1分30.125秒として設定されます。

設定可能範囲は次のとおりです。

```text
00:01.000 ～ 99:59.999
```

## ファイル構成

```text
horror-countdown-timer/
├─ timer.html
├─ README.md
└─ assets/
   ├─ video/
   │  └─ vhs-background.mp4
   ├─ digits/
   │  ├─ 0.png
   │  ├─ 1.png
   │  ├─ 2.png
   │  ├─ 3.png
   │  ├─ 4.png
   │  ├─ 5.png
   │  ├─ 6.png
   │  ├─ 7.png
   │  ├─ 8.png
   │  └─ 9.png
   └─ audio/
      ├─ start.ogg
      ├─ abort.ogg
      ├─ finish.ogg
      └─ tick.ogg
```

## 素材仕様

### 背景動画

配置場所:

```text
assets/video/vhs-background.mp4
```

推奨仕様:

- MP4コンテナ
- H.264映像
- `yuv420p`
- 1920×1080または1280×720
- シームレスにループできる映像
- 音声トラックなし

AV1のMP4は、ブラウザーやOSによって再生できない場合があります。幅広い環境での再生を優先する場合は、H.264を使用してください。

FFmpegで変換する例:

```bash
ffmpeg -i input.mp4 -c:v libx264 -preset medium -crf 20 -pix_fmt yuv420p -movflags +faststart -an assets/video/vhs-background.mp4
```

### 数字画像

配置場所:

```text
assets/digits/0.png ～ assets/digits/9.png
```

推奨仕様:

- 透過PNG
- 600×1000px
- アスペクト比3:5
- すべて同じキャンバス寸法
- 数字の上下位置と中心位置を統一
- sRGB

通常の分・秒表示と、縮小されたミリ秒表示で同じ数字画像を使用します。

`1`などの細い数字も、画像自体の幅を狭くせず、600×1000pxの共通キャンバス中央に配置してください。画像サイズが数字ごとに異なると、切り替え時に表示位置が動きます。

### 効果音

| ファイル | 用途 |
| --- | --- |
| `start.ogg` | カウントダウン開始時 |
| `abort.ogg` | カウントダウン中止時 |
| `finish.ogg` | カウントダウン終了時 |
| `tick.ogg` | 1秒ごとのティック音 |

`tick.ogg`は、次の秒の音と重なりにくい短い音源を推奨します。

```text
推奨長: 約0.05～0.3秒
```

## 起動方法

### Live Serverを使用する場合

1. リポジトリを開く
2. `timer.html`をLive Serverで開く
3. ブラウザー上で操作する

### 簡易HTTPサーバーを使用する場合

Python 3がインストールされている環境では、リポジトリ直下で次を実行できます。

```bash
python -m http.server 8000
```

その後、ブラウザーで次を開きます。

```text
http://localhost:8000/timer.html
```

`file://`から直接開くこともできますが、動画・音声の調査やブラウザー間の挙動確認にはローカルHTTPサーバーの使用を推奨します。

## カスタマイズ

### 初期時間

JavaScript内の次の値を変更します。

```js
const DEFAULT_TIME_MS = 5 * 60 * 1000;
```

例として、初期時間を10分にする場合:

```js
const DEFAULT_TIME_MS = 10 * 60 * 1000;
```

### 警告演出を開始する時間

現在は残り30秒から警告演出を開始します。

```js
const DANGER_TIME_MS = 30 * 1000;
```

残り1分から開始する場合:

```js
const DANGER_TIME_MS = 60 * 1000;
```

### 背景の暗さ

CSS冒頭の変数で調整できます。

```css
:root {
  --screen-darkness: 0.7;
}
```

値を小さくすると背景が明るくなり、大きくすると暗くなります。

```css
--screen-darkness: 0.35;
```

### 終了時の反転演出

終了時の明るさは`.scene.finished`で調整します。

```css
.scene.finished {
  filter:
    invert(1)
    brightness(0.32)
    contrast(1.25)
    saturate(0.7);
}
```

`brightness()`を小さくすると、反転後の画面を暗くできます。

### コロンの位置

コロンはフォント文字ではなく、CSSの疑似要素で描画します。

```css
.separator::before {
  top: 34%;
}

.separator::after {
  top: 62%;
}
```

数字画像に合わせて、上側と下側の点の位置を調整してください。

## 動画が表示されない場合

次の項目を確認してください。

1. ファイルが`assets/video/vhs-background.mp4`に存在する
2. ファイル名の大文字・小文字がHTMLと一致している
3. 映像コーデックがH.264になっている
4. ピクセル形式が`yuv420p`になっている
5. ブラウザーの開発者ツールで404やデコードエラーが出ていない
6. 背景暗幕が強すぎない

FFprobeで確認する例:

```bash
ffprobe -v error -select_streams v:0 -show_entries stream=codec_name,pix_fmt,width,height -of default=noprint_wrappers=1 assets/video/vhs-background.mp4
```

期待される例:

```text
codec_name=h264
width=1920
height=1080
pix_fmt=yuv420p
```

## 注意事項

- ミリ秒値は内部的に計算されますが、画面の更新頻度はモニターのリフレッシュレートに依存します。
- 60Hzの画面では約16.7ミリ秒間隔で描画されるため、ミリ秒表示が1ずつ連続して変化するとは限りません。ディスプレイの更新頻度依存になります。
- ブラウザーの自動再生制限により、ユーザー操作前の音声再生は許可されない場合があります。
- 背景動画は`muted`で再生されます。動画を差し替えた際に音声が入っていても再生されません。
- 終了時には画面反転と点滅を使用します。必要に応じてCSSで演出を弱めてください。

## 使用技術

- HTML5
- CSS3
- JavaScript
- MP4 / H.264
- Ogg Vorbis
- PNG

## ライセンス

MIT Licence
