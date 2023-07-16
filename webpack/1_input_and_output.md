# Webpack 學習筆記 之一：輸入與輸出

// webpack.config.js

```js
const path = require('path');

module.exports = {
  context: path.resolve(__dirname, 'src'),
  mode: 'development',
  devtool: 'inline-source-map',
  entry: './index.js',
  output: {
    iife: true,
    clean: true,
    filename: '[name].[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

- path: node.js 的內建模組，負責處理檔案路徑。
- dirname: node.js 的內建變數，代表目前檔案所在的目錄。
- context:
  預設是 config 檔案所在的目錄，但是可以透過`context: path.resolve(__dirname, 'src')`這個設定改變預設的目錄，代表專案的根目錄是`src`，這樣在設定其他路徑時，就可以省略`src`這個目錄。

- mode: 設定 webpack 的模式，有`development`、`production`、`none`三種模式，預設是`production`，會對打包後的檔案進行壓縮，但是在開發時，我們不需要壓縮檔案，所以設定為`development`，`none`則是不進行任何處理。
- devtool: 設定 sourcemap 的模式，預設是`false`，不產生 sourcemap，但是在開發時，我們需要 sourcemap 來方便除錯，所以設定為`inline-source-map`，這樣打包後的檔案中，會包含 sourcemap 的內容。
- entry: 設定 webpack 的進入點，預設是`./src/index.js`，但是我們可以透過這個設定改變進入點，這樣 webpack 就會從這個檔案開始打包，可以設定的格式有以下幾種：
  - 字串：`'./src/index.js'`
  - 陣列：`['./src/index.js', './src/other.js']`
  - 物件：`{ main: './src/index.js', other: './src/other.js' }`
    字串的話，代表只有一個進入點，陣列的話，代表有多個進入點，物件的話，代表有多個進入點，並且可以設定 chunk 的名稱，chunk 會在之後的 code splitting 詳細說明。
- output: 設定 webpack 的輸出，預設是`./dist/main.js`，但是我們可以透過這個設定改變輸出的檔案名稱與路徑，可以設定的格式有以下幾種：
  - 字串：`'bundle.js'`
  - 物件：`{ filename: 'bundle.js', path: path.resolve(__dirname, 'dist') }`
    字串的話，代表只有一個輸出點，物件的話，
    - iife: 設定輸出的檔案是否使用 IIFE 包裝，預設是`false`，但是我們可以透過這個設定改變輸出的檔案是否使用 IIFE 包裝，如果設定為`true`，那麼輸出的檔案就會使用 IIFE 包裝，這樣就可以避免污染全域變數。
    - clean: 設定輸出的檔案是否要清除，預設是`false`，但是我們可以透過這個設定改變輸出的檔案是否要清除，如果設定為`true`，那麼輸出的檔案就會在每次打包前先清除，這樣就可以避免輸出的檔案越來越多。
    - filename: 設定輸出的檔案名稱，預設是`main.js`，但是我們可以透過這個設定改變輸出的檔案名稱，可以使用以下的 placeholder：
      - `[name]`：進入點的名稱，如果是物件的話，就是物件的 key。
      - `[contenthash]`：檔案內容的 hash 值，如果檔案內容有變更，hash 值就會變更，這樣可以避免瀏覽器快取舊的檔案。
    - path: 設定輸出的路徑，預設是`./dist`，但是我們可以透過這個設定改變輸出的路徑，可以使用 node.js 的內建模組`path`來處理路徑。

這裡指大概說明一下 webpack 的常用設定，有需要之後會再更深入說明。

## 參考資料

- [Configuration - webpack](https://webpack.js.org/configuration/)