# 优雅调用Gemini的API

# 使用 Vercel 反代 Gemini API，解决国内访问难题

## 为什么要使用 Gemini API

1. **Gemini API 是免费的** (有使用限制)：[Google AI 官方定价](https://ai.google.dev/pricing "limits applied!")

## 国内访问 Gemini API 的挑战

直接使用 Gemini API 存在一个主要问题：

`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=GEMINI_API_KEY`​

由于网络限制，国内环境无法直接访问该 API。即使使用科学上网，也可能因为网络不稳定或经济成本较高而难以稳定使用。

## 解决方案：使用 Vercel 反向代理

### 原理

Vercel 本身域名在国内被墙，但其自定义域名功能不受影响。通过在 Vercel 上部署反向代理服务，并将自定义域名绑定到该服务，即可实现国内直连 Gemini API。

### 优势

* **绕过网络限制：**  利用自定义域名，实现国内稳定访问。
* **理论上可代理其他被墙站点：**  此方法不仅限于 Gemini API，也可用于其他被墙站点的反向代理。

## 如何开始

### 前期准备

1. **Google API 密钥：**  前往 [Google API 密钥页面](https://makersuite.google.com/app/apikey) 获取。
2. **一个域名：**  无需备案，可从阿里云或腾讯云等服务商购买，价格通常较低。

### Vercel 部署步骤

1. **使用 Vercel 部署项目：**  点击 [Vercel 部署链接](https://vercel.com/new/clone?repository-url=https://github.com/PublicAffairs/openai-gemini&repository-name=my-openai-gemini "点击进行部署")，一键部署。

    * **项目来源：**  [PublicAffairs/openai-gemini: Gemini ➜ OpenAI API proxy. Serverless!](https://github.com/PublicAffairs/openai-gemini)
2. **设置自定义域名：**

    * 进入 Vercel 项目，依次点击 `Settings`​ -> `Domains`​。
    * 添加域名，建议使用二级域名 (例如 `abc.yourdomain.com`)，因为一级域名通常用于网站展示。![image-20250105165312506](https://gitee.com/Ashesttt/picture/raw/master/202501051653609.png)
    
    * **添加二级域名示例：**
      * **主机记录：**  例如 `abc`​ (可自定义)
      * **域名类型：**  选择二级域名
      * **DNS 解析：**  Vercel 会提示添加 DNS 解析记录。![image-20250105165339072](https://gitee.com/Ashesttt/picture/raw/master/202501051653150.png)
3. **配置 DNS 解析：**

    * 前往你的域名服务商 (如阿里云/腾讯云) 的域名管理页面。
    * 找到并点击 **解析**，然后点击 **添加记录**。
    * 将 Vercel 提供的 DNS 解析记录复制粘贴到域名服务商的解析设置中。
    * **示例：**

      * **记录类型：**  `CNAME`​
      * **主机记录：**  `api`​ (与 Vercel 设置一致)
      * **记录值：**  Vercel 提供的 CNAME 值
    * 保存 DNS 解析设置。![image-20250105165353293](https://gitee.com/Ashesttt/picture/raw/master/202501051653339.png)
4. **验证域名配置：**

    * 返回 Vercel，点击 `Refresh`​ 按钮。
    * 稍等片刻，如果域名状态显示 `Valid`，则表示配置成功。![image-20250105165401364](https://gitee.com/Ashesttt/picture/raw/master/202501051654435.png)

## 如何使用反代后的 Gemini API

现在，你可以使用你配置的自定义域名来替换 `generativelanguage.googleapis.com`​ 进行 API 调用。

### API 请求示例

```bash
curl https://api.yourdomain.com/v1/chat/completions \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer YOUR_GEMINI_API_KEY" \
 -d '{
     "model": "gemini-1.5-flash",
     "messages": [{"role": "user", "content": "介绍一下你自己吧!"}],
     "temperature": 0.7
 }'
```

**注意：**

* 将 `https://api.yourdomain.com`​ 替换为你实际的自定义域名。
* 将 `YOUR_GEMINI_API_KEY`​ 替换为你实际的 Gemini API 密钥。

### 重要提示

* 直接在浏览器中访问反代后的域名会返回 `404 Not Found`​，这是正常的。
* API 并非设计为直接浏览器访问，应在软件或代码中调用。

### 更多信息

如需了解支持的模型、API 终端节点和参数，请参考项目文档：

[Dasuay/GeminiApi：Gemini ➜ OpenAI API 代理。无服务器！](https://github.com/Dasuay/GeminiAPI?tab=readme-ov-file#models)
