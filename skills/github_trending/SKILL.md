---
name: github trending
description: Get the trending repositories on GitHub.
license: MIT
metadata: 
  authors: 
    - stevessr
  tags:
    - github
    - trending
    - rss
---

# GitHub 热门项目

在本节中，您将学习如何获取 GitHub 上的热门仓库。这有助于您发现新项目，并随时了解软件开发社区的最新趋势。

## 步骤 1：使用 仓库 mshibanami/GitHubTrendingRSS 获取 GitHub 上的热门仓库

GitHubTrendingRSS 提供了一个 RSS API，允许您访问各种数据，包括热门仓库。
请参考以下示例请求来获取 GitHub 上的热门仓库：

`GET https://mshibanami.github.io/GitHubTrendingRSS/${time}/${language}.xml`
time 可以是 daily、weekly 或 monthly，表示您想要获取的热门仓库的时间范围。
language 是必选的，表示您想要获取的热门仓库的编程语言，
特殊的，all 表示获取所有，unknown 表示未知的。

## 步骤 2：获取 RSS API 响应

API 的响应将以 XSS 格式返回，返回值的示例为 (请注意，实际返回的时候可能会有多个 item，这里只展示了一个项目的示例)：

```sh
curl https://mshibanami.github.io/GitHubTrendingRSS/daily/zig.xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/">
  <channel>
    <title>GitHub Zig Daily Trending</title>
    <description>Daily Trending of Zig in GitHub</description>
    <pubDate>Wed, 25 Feb 2026 03:23:58 GMT</pubDate>
    <link>http://mshibanami.github.io/GitHubTrendingRSS</link>
    <item>
      <title>ziglang/zig</title>
      <link>https://github.com/ziglang/zig</link>
      <description>&lt;p&gt;Moved to Codeberg&lt;/p&gt;&lt;hr&gt;</description>
      <media:content url="https://opengraph.githubassets.com/f23abb6db4466ca89cde3530dd97c822d52c561a224e0431101e9be798f79fa5/ziglang/zig" medium="image" />
    </item>
  </channel>
</rss>
```

推荐实践为存入某个 xml 文件中，使用 xml 解析器进行解析，提取出您需要的信息，例如仓库名称、描述和 URL。

## 步骤 3: 处理响应

存入文件后，你可以使用 xmlstarlet 进行解析，提取出你需要的信息，例如仓库名称、描述和 URL。

```sh
xmlstarlet sel -t -m "//item" -v "title" -o " | " -v "description" -o " | " -v "link" -n trending.xml
```

## 步骤 4: 输出结果
处理完响应后，您可以将结果输出到控制台或存储在数据库中，以便以后使用。