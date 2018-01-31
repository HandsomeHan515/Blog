+ 执行 yarn add babel-plugin-import --dev or npm install babel-plugin-import  --save-dev
+ 修改一下文件

webpack.config.dev.js(开发环境)中添加plugins

```webpack.config.dev.js
{
  test: /\.(js|jsx|mjs)$/,
  include: paths.appSrc,
  loader: require.resolve('babel-loader'),
  options: {
    plugins: [
			['import', { libraryName: 'antd', style: 'css' }],
		],
    // This is a feature of `babel-loader` for webpack (not Babel itself).
    // It enables caching results in ./node_modules/.cache/babel-loader/
    // directory for faster rebuilds.
    cacheDirectory: true,
  },
}
```

webpack.config.prod.js(线上环境)中添加plugins

```webpack.config.prod.js
// Process JS with Babel.
{
  test: /\.(js|jsx|mjs)$/,
  include: paths.appSrc,
  loader: require.resolve('babel-loader'),
  options: {
    plugins: [
      ['import', { libraryName: 'antd', style: 'css' }],
    ],

    compact: true,
  },
},
```
