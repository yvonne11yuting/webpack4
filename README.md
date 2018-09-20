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
> --save-dev(-D): 開發階段會用到的package / save the package for development purpose
> --save is: 專案運行時會用到的package / save the package required for the application to run.

為 **webpack** 增加npm scripts在 **package.json** 裡
```
"scripts": {
  "build": "webpack --mode production"
}
```
完成後可以在 src 資料夾增加 js 檔案，然後在Terminal上執行yarn build / npm build，webpack即產生一個打包後的main.js檔案




# reference
[Tutorial: How to set up React, webpack 4, and Babel 7 (2018)](https://www.valentinog.com/blog/react-webpack-babel/)