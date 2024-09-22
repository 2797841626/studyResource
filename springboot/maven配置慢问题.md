显示一直在resolving dependencies，速度很慢
原因：maven会使用远程仓库来加载依赖，是一个国外的网站，所以会很慢。应该使用阿里云的镜像，这样速度会提升很多。

步骤：
1.右击pom.xml，选择"maven"->“create settings.xml”，创建了之后该图标会显示成"open settings.xml"，点击它。

2.接着在setting.xml中添加镜像。
镜像源码：
`<mirrors>`
        `<mirror>`
            `<id>alimaven</id>`
            `<name>aliyun maven</name>`
            `<url>http://maven.aliyun.com/nexus/content/groups/public/</url>`
            `<mirrorOf>central</mirrorOf>`
        `</mirror>`
    `</mirrors>`
最后，重启idea，右击pom.xml选择reImport即可。