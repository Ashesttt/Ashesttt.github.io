# Alpha_优雅调用Gemini的API

## 为什么要使用Gemini的API

1. Gemini [API 是免费的](https://ai.google.dev/pricing "limits applied!")

‍

## 困难

​`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=GEMINI_API_KEY"`​

这是Gemini的API 国内环境根本用不了 科学上网能否用全靠运气（或者经济实力）（反正我是用不了）

那怎么办呢

‍

## 使用vercel反代

国内只是墙了 vercel 本身的域名，但是自定义域名没有墙，因此做了代理并绑定域名之后就可以用自己的域名在国内直连 openai api 了。

理论上也可以代理其他被墙的站点。

‍

## 如何开始

* Google [API 密钥](https://makersuite.google.com/app/apikey)
* 一个域名（无需备案），没有的话可以在阿里云（或者腾讯云）上买一个几块钱一年的

‍

### 使用 Vercel 进行部署

#### [vercel部署](https://vercel.com/new/clone?repository-url=https://github.com/PublicAffairs/openai-gemini&amp;repository-name=my-openai-gemini "点击进行部署")

项目来源：[PublicAffairs/openai-gemini: Gemini ➜ OpenAI API proxy. Serverless!](https://github.com/PublicAffairs/openai-gemini)

‍

#### 部署完毕，开始设置你的域名

进入到项目里之后，依次点击 Settings -> Domains，然后添加你的域名。添加的域名类型有两种，一种是一级域名(xxxx.com)和二级域名(openai.xxxx.com)，我个人推荐使用二级域名，因为一级域名一般用来做网站展示用，只能有一个，而二级域名可以有无限个（只要你有一个域名就可以自己创建无限个二级域名）

我这里就展示二级域名了

1. （以 abc主机记录为例，可以改成自己喜欢的）![image](assets/image-20250105161210-uutgltc.png)​

2. 添加域名有三种方式，这里我们选第三种，因为简单
3. 添加二级域名后 Vercel 会提示让你添加 DNS 解析记录![609410f6c437502f22026890a165afe](assets/609410f6c437502f22026890a165afe-20250105161413-op0bez3.png)​

4. 前往你的阿里云或者腾讯云的**我的域名**找到并点击**解析**，然后点击**添加记录**，然后复制粘贴，确认，完事。

​![3a16ad626c051ea25150dea2b47a5bd](assets/3a16ad626c051ea25150dea2b47a5bd-20250105161751-nvowl6u.png)​

5. 回 Vercel 点击 Refresh 按钮，第一次会等待的有点久，出现下图所示的情况就表明配置完成了。![cf64056d4c4b2ececb535179ad50425](assets/cf64056d4c4b2ececb535179ad50425-20250105162010-qx15psr.png)​

至此，已设置完vercel。

## 如何使用调用Gemini的API

现在已经可以通过你在vercel设置的域名来替换掉`generativelanguage.googleapis.com`​来使用了

API 请求示例：

```bash
curl https://yourdomain/v1/chat/completions \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer `GEMINI_API_KEY`" \
 -d '{
     "model": "gemini-1.5-flash",
     "messages": [{"role": "user", "content": "介绍一下你自己吧!"}],
     "temperature": 0.7
 }'
```

如果您在浏览器中打开新部署的站点，则只会看到一条消息`404 Not Found`​。这是意料之中的，因为 API 不是为直接浏览器访问而设计的。 要使用它，您应该在软件设置的相应字段中输入您的 API 地址和 Gemini API 密钥。

‍

如果您想调用其他模型或者媒体或者想知道支持的 API 终端节点和适用参数，请移步到项目查看：

[Dasuay/GeminiApi：Gemini ➜ OpenAI API 代理。无服务器！](https://github.com/Dasuay/GeminiAPI?tab=readme-ov-file#models)

‍
