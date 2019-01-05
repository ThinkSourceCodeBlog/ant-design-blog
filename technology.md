这里的关键技术就是使用下面这个，可以将markdown的数据解析为page
https://github.com/raoshihong/bisheng.git

目标指定一个模板文件,这个模板文件在site/theme/static/template.html中,而这个模块的指定在site/them/bisheng.config.js中

module.exports = {
  port: 8001,
  source: {
    components: './components',
    docs: './docs',
    changelog: ['CHANGELOG.zh-CN.md', 'CHANGELOG.en-US.md'],
  },
  theme: './site/theme',
  htmlTemplate: './site/theme/static/template.html',
  
  
而在执行npm start 时,会执行package.json中的指令
"start": "rimraf _site && mkdir _site && node ./scripts/generateColorLess.js && cross-env NODE_ENV=development bisheng start -c ./site/bisheng.config.js",

所以在启动的时候,就已经开始解析模板文件了,template.html

整个项目中都使用了这一个模板来进行填充数据

所以模板中的这一句中的content表示填充的内容,每次在跳转时，都会被bisheng这个框架给拦截代理,并将内容填充到模板中
<div id="react-content">
  {{ content | safe }}
</div>

流程：
npm start -> index.js -> ./template/Layout/index.jsx -> Home/index.jsx

而在我们点击docs/react/ 或者docs/spec/feature-cn 这样的路径,则会被bisheng框架拦截转发到template/Content/index或者template/Redirect
所以当我们看到在Banner.jsx中<Link to={utils.getLocalizedPathname('/docs/react/introduce', isZhCN)}> 这句时,表示跳转到了template/Content/index.jsx
流程  docs/react/  转发  template/Content/index.jsx  --> MainContent.jsx  所以这样菜单栏和内容就出现了
