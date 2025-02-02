# VanManga - 漫画抓取应用 
<div align="center">
  
![VanLogo](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/e4f30d77-a6fe-421a-b411-af73134ffdfa)  

![Brands](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/654e0b06-45e4-4754-8841-51abb64d019e)  

![Preview](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/40b1bfc5-0e74-41e3-9fe0-07ba6882ca12)

VanManga是一款轻量化的，功能丰富的漫画抓取应用。该应用可以抓取、管理漫画资源，以此为漫画阅读、资源备份等提供服务。  

[Readme - English Version](https://github.com/Alen-QK/python-vanmanga-crawler/blob/aijiangsb/dev_config/README-engVer.md)
</div>  
  
  
## VanManga有什么功能？
1. 🔍 **漫画搜索** - 通过在搜索框中输入漫画名称，你可以获取前十个最相关的搜索结果，并选择你喜欢的漫画进行下载。
2. ⚠️ **漫画管理** - 为保证本地资源精简不重复，应用会在提交下载任务时自动检查已下载漫画中是否有相似资源，并返回结果以供用户参考。
3. 📋 **下载队列** - 漫画会根据下载任务提交顺序进入队列等待，并依序下载。
4. 🔄 **每日自动更新** - 应用会每日定时扫描所有已下载漫画，检查并自动下载最新的更新内容。
5. 🖥️ **下载管理面板** - 在管理面板中，你可以检视所有的已下载资源。并且可以通过WebSocket返回的信息实时跟踪当前下载任务的进度和漫画下载状态。
6. 🗃️ **重新打包漫画文件** - 有外部资源？抑或是本地资源文件结构有问题？没关系，可以使用本地重新打包功能对资源文件夹下的所有漫画文件进行重新打包。
7. 🔧 **重新下载指定内容** - 想要对下载可能失败的单章漫画进行修复？没问题，使用单章重新下载的功能你可以自由的选择任意漫画的任何章节进行重下。
8. **也许在未来我们会添加更多新功能！**
  
  

## 如何安装？
目前，本应用可以通过Docker Hub上的打包镜像来进行本地部署。同时，你也可以自定义本地VanManga的HTTP服务端口。
1. 请确保你的系统中已经安装了[Docker](https://www.docker.com/)。在Windows和MacOS系统下，我们推荐安装[Docker Desktop](https://www.docker.com/products/docker-desktop/)的GUI界面来更轻松的进行部署。如果你正在使用Linux系统，请使用命令行来完成部署。
2. 安装Docker后，请使用如下命令来从Docker Hub获取最新版本的VanManga镜像：
    ```
   docker pull wzl778633/vanislord_manga:latest
   ```
3. 为了确保你的Docker容器可以正常运行，请按照如下介绍设置 环境变量 \  文件路径映射 \ 端口:
    - 环境变量
        >**PYTHONUNBUFFERED**: 用于使Docker container可以输出应用的日志记录，默认值是1  
        **MANGA_BASE_URL**: 你用于连接后端的URL，默认值是 http://localhost:5000. 如果你正在你的Docker中使用bridge，你需要把'localhost'改成你的bridge的内部IP。  
        **MANGA_BASE_WEBSOCKET_URL**: 你对外暴露的服务器URL, 主要用于WebSocket服务, 默认值是 http://localhost:5000.
    - 文件路径映射
        > { 你的主机上想要设置为漫画下载库的物理地址 } <==> /downloaded    
        { 你的主机上想要设置为漫画config文件夹的物理地址 } <==> /vanmanga/eng_config
    - 端口
        > 5000:5000 /* 你也可以映射任何你想映射的端口 */
    
   你可以直接复制并使用如下命令，或者在Docker Desktop的GUI中直接设置相关参数:  
    ```
   docker run --name /*你的容器名称*/ \
    -e PYTHONUNBUFFERED=1 \
    -e MANGA_BASE_URL=http://localhost:5000  \
    -e MANGA_BASE_WEBSOCKET_URL=http://localhost:5000  \
    --mount type=bind,source=/*你的主机上想要设置为漫画下载库的物理地址*/,target=/downloaded \
    --mount type=bind,source=/*你的主机上想要设置为漫画config文件夹的物理地址*/,target=/vanmanga/eng_config \
    -p 5000:5000
    wzl778633/vanislord_manga:latest
   ```
4. 当容器开始运行后，你需要访问你刚刚设定的对外暴露的服务器地址(如： http://localhost:5000) 来启动初始化。目前，网站只有在初始化完成后才会允许访问，但是你可以在Docker container的日志记录中查看服务器初始化的状态。
    - 初始化的目的在于，扫描config文件夹下的`manga_library.json`，将所有的已经存在的漫画记录都添加到服务器中，同时检查每部漫画是否有更新。
  
  
## 如何使用？
1. **搜索漫画**  
    目前我们使用[DogeManga](https://dogemanga.com/)来作为漫画源。  
    在搜索框中输入想搜索的漫画名称，它会返回前十个最相关的结果。**请注意：** 我们会在用户提交下载任务时自动检查已存在资源中是否有可能的类似漫画，如果存在，我们会返回相关信息方便用户确认。  
    ![s1](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/b0daddd5-faa3-41e6-aaba-2b18c8ea43a7)     
    ![s2](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/1b7ab286-64c6-4069-83dc-bae342fdc49a)
2. **下载漫画**  
    从搜索结果中选择希望下载的对象，并提交下载任务。目前，应用会下载目标漫画下可下载的所有内容(包括但不限于：单行本、连载章节、番外、特殊内容等)。 
    ![s3](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/b27b1631-0faa-46a2-96f9-645b3929907e)  
    ![s4](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/2b0d9c7a-e343-4ad6-8a0a-57493a87460e)
3. **漫画库管理界面**  
    你下载的所有漫画都可以在此仪表板中找到。你可以查看每部漫画的当前状态（例如：下载中/排队中/已完成...）。目前，仪表板仅显示了你已下载的每部漫画的最新章节名称，以便你跟踪每部漫画的状态。
    ![s4 5](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/9246f8ff-01a6-44cd-a360-10d401eafb25) 
    ![s5](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/3a1d2837-fad0-46bc-a953-d3d1c080eec3)
4. **重新下载**  
    对于每一部漫画，如果在阅读过程中发现任何（包括但不限于：单行本、连载章节、番外）内容出现错误，您可以简单地请求重新下载该特定（包括但不限于：单行本、连载章节、番外）的内容。
    ![s6](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/4b0cbb19-fb58-40ab-9e68-3e73175efa78) 
    ![s7](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/0b2aca2f-1a40-4d66-9671-0992f1b9ac61)
    ![s8](https://github.com/Alen-QK/python-vanmanga-crawler/assets/37805183/d19194c8-e273-4dbe-9439-53e54b0a4a3d)
5. **阅读漫画**  
    目前，本应用的文件结构主要支持[Kavita](https://github.com/Kareadita/Kavita)。我们也强烈建议你使用Kavita来作为漫画库服务器。您可以通过其Web界面轻松管理您的漫画文件，并使用其阅读器进行阅读。它同样支持在IOS设备上使用[Paperback](https://paperback.moe/)以及在Android设备上使用[Tachiyomi](https://tachiyomi.org/)进行阅读。下面是一些有关服务器部署及设置的相关链接：  
    - Kavita: https://wiki.kavitareader.com/en
    - Paperback: https://wiki.kavitareader.com/en/guides/misc/paperback
    - Tachiyomi: https://wiki.kavitareader.com/en/guides/misc/tachiyomi
   
    当然，如果你喜欢使用除Kavita外的其他的漫画服务器，请随意使用～
  
  
## Demo

当前上线的版本为私人社群服务版本，不公开展示，非常抱歉。
  
  
## 贡献
### 贡献者

目前，有两位主要贡献者在为这个项目开发。
<a href="https://github.com/Alen-QK/python-vanmanga-crawler/graphs/contributors">
Tom "Van" Wang & Kun "Alen" Qi
</a>

我们非常欢迎你对我们的项目做出任何贡献～
  
  
## License

[MIT](LICENSE)
