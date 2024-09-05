# 在GitHub暴露了敏感信息的文件怎么办？

要在 GitHub 上删除已经暴露了敏感信息的文件怎么办？，并确保将来不再推送该文件，可以按照以下步骤

>以application.yaml为例子

## 1.从 Git 仓库中删除敏感文件

```bash
git rm --cached D:\Emergency_Material_Manage\SpringBoot\src\main\resources\application.yaml
```

## 2.在 .gitignore 文件中忽略文件

```bash
application.yaml
```

## 3.提交更改并上传

就成功了