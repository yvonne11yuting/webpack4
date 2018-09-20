# React + Webpack 4 + Babel 7
這邊紀錄簡單的react、webpack 4和babel 7的安裝設定。
多虧webpack 4亮麗登場，預設了entry point和output point，可以無腦的少寫幾行設定?<br>
這篇主要參考 [Tutorial: How to set up React, webpack 4, and Babel 7 (2018)](https://www.valentinog.com/blog/react-webpack-babel/) <br>
真的是一篇輕鬆好讀的入門文，大推！

## Step1: 設定專案
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

## Step2: 設定webpack
安裝 webpack and webpack-cli
```
yarn add webpack webpack-cli -D
```
* --save-dev(-D): 開發階段會用到的package / save the package for development purpose
* --save is: 專案運行時會用到的package / save the package required for the application to run.

為 **webpack** 增加npm scripts在 **package.json** 裡
```json
"scripts": {
  "build": "webpack --mode production"
}
```
完成後可以在 src 資料夾增加 index.js 檔案，然後在Terminal上執行yarn run build / npm run build，webpack即產生一個打包後的main.js檔案

※ webpack的預設入口(entry point)為index.js

## Step3: 設定Babel
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

## Step4: 撰寫React components
參考的教學遵循了Container / Presentational原則做了小範例，包含
[container components](https://medium.com/@learnreact/container-components-c0e67432e005)和[smart and dumb components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)。

* container component: 帶有所有邏輯的component，用於處理state更改，內部component state等function
* presentational component: 只用於顯示markup，通常只是普通的arrow function，透過props來接收資料。

先來安裝react吧
```
yarn add react react-dom -D
```
然後建立資料夾。-p這個參數可以把中間沒建立的資料夾一併一起做掉，等於我現在src這層底下沒子層資料夾，
透過-p可以幫我把js > components > container 和 presentational 等資料夾一起建立。真的好棒棒！

```
mkdir -p src/js/components/{container,presentational}
```

然後在container下建立一個名為FormContainer.js的檔案
```
touch src/js/components/container/FormContainer.js
```
內部程式碼如下
```javascript
import React, { Component } from "react";
import ReactDOM from "react-dom";
class FormContainer extends Component {
  constructor() {
    super();
    this.state = {
      title: ""
    };
  }
  render() {
    return (
      <form id="article-form"></form>
    );
  }
}
export default FormContainer;
```
它目前只是個骨架，之後會再改到它。

然後在presentational下建立Input.js檔案
```
touch src/js/components/presentational/Input.js
```
內部程式碼如下
```javascript
import React from "react";
const Input = ({ label, text, type, id, value, handleChange }) => (
  <div className="form-group">
    <label htmlFor={label}>{text}</label>
    <input
      type={type}
      className="form-control"
      id={id}
      value={value}
      onChange={handleChange}
      required
    />
  </div>
);
export default Input;
```

※ 原文還有使用[Prop Types](https://reactjs.org/docs/typechecking-with-proptypes.html)來判斷送來的props是否符合型別與是否為必須值，但因為這次只是想紀錄react和webpack+babel安裝設定，所以先略過了...哈哈，有興趣可參考[原文](https://www.valentinog.com/blog/react-webpack-babel/)

好囉好囉，Input也完成了，可以更新FormContainer.js的內容哩，更新後程式碼如下~<br>
主要更新的部分為將Input component放進form內，然後將增加Input props所需的handleChange function建立，並且在constructor綁定this，避免function被呼叫時，this指向的不是這個class。

```javascript
import React, { Component } from "react";
import ReactDOM from "react-dom";
import Input from "../presentational/Input";
class FormContainer extends Component {
  constructor() {
    super();
    this.state = {
      seo_title: ""
    };
    this.handleChange = this.handleChange.bind(this);
  }
  handleChange(event) {
    this.setState({ [event.target.id]: event.target.value });
  }
  render() {
    const { seo_title } = this.state;
    return (
      <form id="article-form">
        <Input
          text="SEO title"
          label="seo_title"
          type="text"
          id="seo_title"
          value={seo_title}
          handleChange={this.handleChange}
        />
      </form>
    );
  }
}
export default FormContainer;
```

因為webpack預設的entry point(入口)為./src/index.js (沒有就建立一個新的吧)
```
touch src/index.js
```
所以我們要在index.js匯入我們寫好的container component，index.js內容如下：
```javascript
import FormContainer from "./js/components/container/FormContainer";
```
然後就可以執行試試啦
```
yarn run build
```
執行完後可以在./dist/main.js看到我們編譯後的javascript

## Step5: the HTML webpack plugin
接下來要做的事就是透過webpack幫我們生成一個html檔把我們的react component放在<script></script>裡面。

首先安裝相關的package
```
yarn add html-webpack-plugin html-loader -D
```
接著更新webpack的設定檔(webpack.config.js)<br>
這次更新的部分為增加html-loader，並加入html-webpack-plugin設定
```javascript
const HtmlWebPackPlugin = require("html-webpack-plugin");
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader"
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
};
```
再來在./src下新增index.html<br>
這邊使用了Bootstrap作為CSS library，反正這部分不太重要大家隨意就好
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-beta.2/css/bootstrap.min.css" >
    <title>How to set up React, Webpack, and Babel</title>
</head>
<body>
    <div class="container">
        <div class="row mt-5">
            <div class="col-md-4 offset-md-1">
                <p>Create a new article</p>
                <div id="create-article-form">
                    <!-- form -->
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```
裡面的重點是這個
```html
<div id="create-article-form"></div>
```
等等要render reactDOM就是抓這個element Id

最後一步就是在./src/js/components/container/FormContainer.js的程式碼最底部加上以下程式碼綁定element
```javascript
const wrapper = document.getElementById("create-article-form");
wrapper ? ReactDOM.render(<FormContainer />, wrapper) : false;
```

此時再執行
```
yarn run build
```
就能看到我們在./dist資料夾看到webpack就會自動幫我們綁定javascript

## Step6: webpack dev server
好溜，我們不會每次調整一個小東西就執行一次build<br>
這裡要安裝webpack-dev-server讓我們開發更順暢<br>
之後每當檔案有修改儲存後，webpack dev server就會幫我們自動刷新頁面囉
```
yarn add webpack-dev-server -D
```
安裝完後我們進到package.json改一下npm scripts，更新後的scripts如下
```json
  "scripts": {
    "start": "webpack-dev-server --open --mode development",
    "build": "webpack --mode production"
  }
```
然後在Terminal執行
```
yarn start
```
大功告成！


# reference
[Tutorial: How to set up React, webpack 4, and Babel 7 (2018)](https://www.valentinog.com/blog/react-webpack-babel/)