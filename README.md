# React + Webpack4
## Step1 setting up the project
為這個專案建個資料夾
```
mkdir webpack4
```
建立一個資料夾來放code
```
mkdir src
```
初始化專案
```
yarn init -y
```

## Step2 setting up webpack
安裝 webpack and webpack-cli
```
yarn add webpack webpack-cli -D
```
* --save-dev(-D): 開發階段會用到的package / save the package for development purpose
* --save is: 專案運行時會用到的package / save the package required for the application to run.

為 **webpack** 增加npm scripts在 **package.json** 裡
```
"scripts": {
  "build": "webpack --mode production"
}
```
完成後可以在 src 資料夾增加 js 檔案，然後在Terminal上執行yarn build / npm build，webpack即產生一個打包後的main.js檔案

## Step3 setting up Babel
React components 大部分都是使用Javascript ES6的語法寫的。由於瀏覽器無法理解React components，所以需要透過Babel轉換。

**Webpack loader** 能夠輸入某些內容並生成其他內容輸出
babel-loader是負責將ES6語法轉換成ES5語法的webpack loader
* babel preset env: 將ES6的code轉乘ES5的語法（注意：babel-preset-es2015已經棄用囉）
* babel preset react: 將jsx和其他東西轉成javascript

安裝和Babel相關的dependencies
```
yarn add @babel/core babel-loader @babel/preset-env @babel/preset-react -D
```

在專案資料夾下建立Babel設定檔(.babelrc)，配置如下
```
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

接下來來定義webpack configuration，在專案資料夾下建立webpack.config.js，然後把下面的設定塞進去。

下面的設定為把所有 **\.js** 的檔案都透過 **babel-loader** 從ES6轉成ES5。
```javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
};
```



# reference
[Tutorial: How to set up React, webpack 4, and Babel 7 (2018)](https://www.valentinog.com/blog/react-webpack-babel/)