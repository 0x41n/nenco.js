# nenco.js
文字コードを色々するJavaScript

## これはなに？
JavaScriptで文字コードの変換とか判定をします。<br>
[encoding.js](https://github.com/polygonplanet/encoding.js)みたいなやつです。<br>
機能はとっても少ないです。

## 特徴
jsファイル一つで完結しているので使いまわしやすいと思われます。<br>
TextEncoderなどを使わず力技で変換しているのも特徴です。

## 使い方
`nenco.js`もしくは`nenco_min.js`をダウンロードして<br>
```html
<script src="nenco.js"></script>
```
みたいな感じで呼び出してください。<br>
`nenco`(`window.nenco`)というオブジェクトとして使えるようになります。<br>

## 対応している文字コード
|文字コード|nencoでの値|備考|
|---|---|---|
|UTF-8|`UTF-8`, `UTF8`, `Unicode 1-1 UTF-8`|`encode`時のデフォルト|
|UTF-16BE|`UTF-16BE`, `UTF-16`, `UTF16`|`BE`や`LE`を省略すると`BE`になります|
|UTF-16LE|`UTF-16LE`, `UTF16LE`||
|UTF-32BE|`UTF-32BE`, `UTF-32`, `UTF32`|`BE`や`LE`を省略すると`BE`になります|
|UTF-32LE|`UTF-32LE`, `UTF32LE`||
|Shift_JIS|`Shift_JIS`, `SJIS`, `MS_Kanji`, `x-sjis`||
|Shift_JIS-2004|`Shift_JIS-2004`, `SJIS2004`|`Shift_JIS`を指定したときもこのコードでencodeされます|
|EUC-JP|`EUC-JP`, `x-eucjp`||
|ISO-2022-JP|`ISO-2022-JP`, `JIS`|`JIS X 0213`部分まで対応(ISO-2022-JP-2004?)|

## できること
### `nenco.encode(text, encoding, bom?)`
文字列を指定した文字コードをもとに、数値の配列(`Uint8Array`)に変換します。
```js
const text = "こんにちは";
const array = nenco.encode(text, "SJIS");
console.log(array); // [130, 177, 130, 241, 130, 201, 130, 191, 130, 205]
```
引数のbomにtrueを指定すると先頭にBOMが付与された配列を返します。
```js
const text = "こんにちは";
const array = nenco.encode(text, "UTF16BE", true);
console.log(array); // [254, 255, 48, 83, 48, 147, 48, 107, 48, 97, 48, 111]
```
---
### `nenco.decode(array, encoding?)`
数値の配列を指定した文字コードをもとに、文字列に変換します。
```js
const array = [130, 177, 130, 241, 130, 201, 130, 191, 130, 205];
const text = nenco.decode(array, "SJIS");
console.log(array); // "こんにちは"
```
文字コードを省略すると配列から文字コードを推測して変換します。<br>
(推測が外れることもあります…)
```js
const array = [48, 83, 48, 147, 48, 107, 48, 97, 48, 111];
const text = nenco.decode(array);
console.log(array); // "こんにちは"
```
変換できなかった文字は`?`に置換されて返ってきます。<br>
不正な配列だった場合は`false`が返ってきます。

---

### `nenco.detext(array)`
数値の配列から、文字コードを推測します。
```js
const array = [48, 83, 48, 147, 48, 107, 48, 97, 48, 111];
const encoding = nenco.detext(array);
console.log(encoding); // "utf16"
```

## おまけ
[encoding.js](https://github.com/polygonplanet/encoding.js)よろしく、日本語のちょっとした変換が出来ます。<br>
文字列を入れて、文字列を受け取るような関数です。

- `toHan(text)`
  - 全角英数記号文字を半角英数記号文字に変換します
- `toZen(text)`
  - 半角英数記号文字を全角英数記号文字に変換します
- `toHira(text)`
  - 全半角カタカナをひらがなに変換します
- `toKata(text)`
  - ひらがなをカタカナに変換します

## 使用上の注意
すべての環境に対応するように作ったわけではないので、<br>
使いやすい形に加工してお使いください。
