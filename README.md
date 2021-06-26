#### *Hà nội ngày 25/06/2021*
### Installation
1. Install React, Webpack, and Babel
```
npm install react react-dom
npm install --save-dev webpack webpack-cli webpack-dev-server html-webpack-plugin @babel/core babel-loader @babel/preset-env @babel/preset-react
```
#### Here's what each package does:
* **react**: UI library for creating modular components
* **react-dom**: enables us to render the React within the browser DOM
* **webpack**:
  ```
  Module bundler - là công cụ để build các module trong javascript
  **Quan trọng** Công việc cốt lõi là merge tất cả các file js thành 1 file
  **Module** là một khái niệm trong NodeJS (không có trong các browser thông thường) 
  **Create module** sử dụng module.export = object
  **Load module** sử dụng required để load file
  ```
  
* **webpack-cli**: allows webpack to work with CLI commands
* **webpack-dev-server**: transforms our source code and serves it on a web server as we develop it continuously. This helps use see the output of your code in the browser.
* **html-webpack-plugin**: an extension to webpack that adds the basic index html file
* **@babel/core**: a JavaScript transpiler to converts modern JavaScript into a production-ready version that's compatible with all browsers.
  ```
  **Quan trong** Babel là Javasccript Transpiler dùng để chuyển đổi code JS (không bao gồm scss, css ...) phiên bản mới thành JS phiên bản cũ
  
  ```
* **babel-loader**: connects Babel to webpack's build process
  ```
  **Quan trọng** Loader là cầu nối để nối babel, css, scss, asset vào với Webpack. Méo hiểu sao phức tạp vc
  ```
* **@babel/preset-env**: group of commonly used Babel plugins bundled into a preset that converts modern JavaScript features into widely compatible syntax
* **@babel/preset-react**: React-specific Babel plugins that convert JSX syntax into plain old JavaScript that browsers can understand

Quick note: --save-dev flag is for partitioning the dependencies into development-specific dependencies, so that they are not included in the production JavaScript bundle

2. Create the files
   
*Copy and paste the following code into each file:*
```
touch webpack.config.js
mkdir src
cd src
touch index.js
touch index.html
```

*webpack.config.js*
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  // the output bundle won't be optimized for production but suitable for development
  mode: 'development',
  // the app entry point is /src/index.js
  entry: path.resolve(__dirname, 'src', 'index.js'),
  output: {
  	// the output of the webpack build will be in /dist directory
    path: path.resolve(__dirname, 'dist'),
    // the filename of the JS bundle will be bundle.js
    filename: 'bundle.js'
  },
  module: {
    rules: [
      {
      	// for any file with a suffix of js or jsx
        test: /\.jsx?$/,
        // ignore transpiling JavaScript from node_modules as it should be that state
        exclude: /node_modules/,
        // use the babel-loader for transpiling JavaScript to a suitable format
        loader: 'babel-loader',
        options: {
          // attach the presets to the loader (most projects use .babelrc file instead)
          presets: ["@babel/preset-env", "@babel/preset-react"]
        }
      }
    ]
  },
  // add a custom index.html as the template
  plugins: [new HtmlWebpackPlugin({ template: path.resolve(__dirname, 'src', 'index.html') })]
};
```
*src/index.js*
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(<App/>, document.getElementById('root'));
```
*src/index.html - bundle.js được config ở bên trong webpack.config.js*
```html
<html>
  <head>
    <title>Hello world App</title>
  </head>
  <body>
  <div id="root"></div>
  <script src="bundle.js"></script>
  </body>
</html>
```
3. Config trong package.json
```json
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --open",
    "build": "webpack",
    "dev": "webpack serve --config webpack.config.js --progress"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "react": "^17.0.2",
    "react-dom": "^17.0.2"
  },
  "devDependencies": {
    "@babel/core": "^7.14.6",
    "@babel/preset-env": "^7.14.7",
    "@babel/preset-react": "^7.14.5",
    "babel-loader": "^8.2.2",
    "html-webpack-plugin": "^5.3.2",
    "webpack": "^5.40.0",
    "webpack-cli": "^4.7.2",
    "webpack-dev-server": "^3.11.2"
  }
}
```
**Đến thời điểm này đã có thể run được project *so happy :)***

4. Config scss - Chắc không thanh niên nào muốn code css 1 dự án lớn đâu

***Đù mé vẫn chưa xong à, nhiều vc lần 2***

> Vậy là đã hiểu Webpack để bundle các module thành 1 file để có để deploy,
> babel transpile code ES mới thành js version cũ
> loader là cầu nối giữa babel, scss, css, asset (thông qua import nhá) với webpack rồi nhá !!! Cố hiểu đi ae
=> Tức là cứ có chữ import là webpack vào việc chẳng cần biết import cái gì (scss, css, image, js ....)
```
**Tư duy** scss chắc chắn thuộc nhóm loader rồi search tiếp nào - ơn giời xong sass loader có config webpack luôn

npm install sass-loader sass --save-dev
```
#### *Hà nội 28/06/2021*
> Nhận ra rằng làm việc khó trong 1 lúc một chốc 1 lát là rất không hiệu quả
> công việc thường xuyên xảy ra lỗi, nôn nóng => Không tìm được phương án tốt
> nhất mà chỉ muốn làm sao để cho xong

Tiếp nhé ae !!! - Sau khi cài đặt sass và sass-loader cần phải apply vào trong config của webpack

<span style="color:red">*Quan trọng*:</span> Các loader đều được khai báo trong module (Mỗi rules là một loader).

Lúc này file webpack.config.js => module sẽ như thế này
```javascript
module: {
    rules: [
      {
        // for any file with a suffix of js or jsx
        test: /\.jsx?$/,
        // ignore transpiling JavaScript from node_modules as it should be that state
        exclude: /node_modules/,
        // use the babel-loader for transpiling JavaScript to a suitable format
        loader: 'babel-loader',
        options: {
          // attach the presets to the loader (most projects use .babelrc file instead)
          presets: ["@babel/preset-env", "@babel/preset-react"]
        }
      },
      {
        test: /\.s[ac]ss$/i,
        use: [
          // Creates `style` nodes from JS strings
          "style-loader",
          // Translates CSS into CommonJS
          "css-loader",
          // Compiles Sass to CSS
          "sass-loader",
        ]
      }
    ]
  }
```

