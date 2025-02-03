# WeChat MiniProgram OSS Licenses Semi-Auto Gen

> [!warning]
> 文档没写完

在托管至 GitHub 的原生微信小程序工程上实现对 npm 依赖库的开放源代码许可证信息半自动化生成

<details>

<summary>效果示例</summary>

![](/readme-assets/demo-screenshot.jpg)

</details>

## 前言

此仓库所有文档采用[木兰开放作品许可协议 署名-相同方式共享，第1版](LICENSE-docs)许可，代码采用[木兰宽松许可证, 第2版](LICENSE)许可。

如果您的微信小程序工程非原生微信小程序工程（如 Uni-App、Taro 等跨平台工程，或 Vite、Webpack、Rollup 等构建工具工程），请优先考虑使用诸如 [rollup-license-plugin](https://github.com/codepunkt/rollup-license-plugin)、[webpack-license-plugin](https://github.com/codepunkt/webpack-license-plugin) 等插件。此仓库所列举的方案只能实现半自动化生成开放源代码许可证信息，而使用 Vite、Webpack、Rollup 等构建工具及其插件可实现全自动编译产物构建。

## 依赖需求

在原生微信小程序工程根目录（即微信小程序 `project.config.json` 所在目录）中打开终端，安装 `license-checker-rseidelsohn` 依赖

```shell
# npm
npm install license-checker-rseidelsohn --save-dev
```

```shell
# yarn
yarn add license-checker-rseidelsohn --dev
```

```shell
# pnpm
pnpm add license-checker-rseidelsohn --dev
```



