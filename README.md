# WeChat MiniProgram OSS Licenses Semi-Auto Gen

在托管至 GitHub 的原生微信小程序工程上实现对 npm 依赖库的开放源代码许可证信息半自动化生成

> [!warning]
> 文档没写完

<details>

<summary>效果示例</summary>

![](/readme-assets/demo-screenshot.jpg)

</details>

## 前言

此仓库所有文档采用[木兰开放作品许可协议 署名-相同方式共享，第1版](LICENSE-docs)许可，代码采用[木兰宽松许可证, 第2版](LICENSE)许可。

如果您的微信小程序工程非原生微信小程序工程（如 Uni-App、Taro 等跨平台工程，或 Vite、Webpack、Rollup 等构建工具工程），请优先考虑使用诸如 [rollup-license-plugin](https://github.com/codepunkt/rollup-license-plugin)、[webpack-license-plugin](https://github.com/codepunkt/webpack-license-plugin) 等插件。此仓库所列举的方案只能实现半自动化生成开放源代码许可证信息，而使用 Vite、Webpack、Rollup 等构建工具及其插件可实现全自动编译产物构建。

## 依赖需求

在原生微信小程序工程根目录（即微信小程序 `project.config.json` 所在目录）中打开终端，安装 `license-checker-rseidelsohn`、`uglify-js` 和 `json5` 依赖

> npm

```shell
npm install license-checker-rseidelsohn uglify-js json5 --save-dev
```

> yarn

```shell
yarn add license-checker-rseidelsohn uglify-js json5 --dev
```

> pnpm

```shell
pnpm add license-checker-rseidelsohn uglify-js json5 --dev
```

## 快速开始

### 1. 配置许可证构建脚本

将 [OSSLicensesBuilder.js](source-code/OSSLicensesBuilder.js)、[OSSLicensesBuilderConfig.json5](source-code/OSSLicensesBuilderConfig.json5) 和 [OSSLicensesBuildFormat.json](source-code/OSSLicensesBuildFormat.json) 文件复制并粘贴至微信小程序工程根目录（即微信小程序 `project.config.json` 所在目录），并执行命令：

```shell
npm pkg set scripts.build-oss-licenses-dist="node OSSLicensesBuilder.js"
```

或直接在根目录 `package.json` 新增：

```json
{
  "scripts": {
    "build-oss-licenses-dist": "node OSSLicensesBuilder.js"
  }
}
```

<details>

<summary>package.json 完整示例</summary>

```json
{
  "dependencies": {},
  "devDependencies": {
    "license-checker-rseidelsohn": "^4.4.2",
    "json5": "^2.2.3",
    "uglify-js": "^3.19.3"
  }, 
  "scripts": {
    "build-oss-licenses-dist": "node OSSLicensesBuilder.js"
  }
}
```

</details>

此时执行 `npm run build-oss-licenses-dist` 命令，则会在微信小程序工程根目录生成 `OSSLicensesDist.js` 文件。开放源代码许可信息由 `module.exports` 语句导出为 JS 对象。

如果您希望将 `OSSLicensesDist.js` 文件放在具体 Page 页或其他自定义目录，请在 `OSSLicensesBuilderConfig.json5` 文件中修改 `customPath` 属性，如：

```json5
[
  {
    outputFile: "OSSLicensesDist",
    customFormat: "OSSLicensesBuildFormat.json",
    customPath: "/this-is-my-goal-page/my-assets/"
  }
]
```

如果您的微信小程序使用了分包，并由于分包而在多个子包存在 npm 依赖项，希望一并生成子包的开放源代码许可信息，则可在 `OSSLicensesBuilderConfig.json5` 文件中新增配置，如：

```json5
[
  {
    outputFile: "OSSLicensesDist",
    customFormat: "OSSLicensesBuildFormat.json",
    customPath: "/"
  },
  {
    outputFile: "OSSLicensesDistSub1",
    customFormat: "OSSLicensesBuildFormat.json",
    customPath: "/",
    startPath: "subpackage1/"
  },
  {
    outputFile: "OSSLicensesDistSub2",
    customFormat: "OSSLicensesBuildFormat.json",
    customPath: "/",
    startPath: "subpackage2/"
  }
]
```

此时执行 `npm run build-oss-licenses-dist` 命令，则会在微信小程序工程根目录生成 `OSSLicensesDist.js`、`OSSLicensesDistSub1.js` 和 `OSSLicensesDistSub2.js` 文件。可按需使用。

### 2. 配置 GitHub Actions 工作流
