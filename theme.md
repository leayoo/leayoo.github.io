# 其他主题对应的配置文件
pure

config.yml

baseURL: https://leayoo.github.io/
# languageCode = "en-us"
# title = "Hello World"
# theme = "hugo-theme-pure"

theme: pure
title: Pure theme for Hugo
copyright: CC BY 4.0 CN
defaultContentLanguage: en  # en/zh/...
footnoteReturnLinkContents: ↩
hasCJKLanguage: true
paginate: 7
enableEmoji: true
PygmentsCodeFences: false
googleAnalytics: ""      # UA-XXXXXXXX-X 
permalinks:
  posts: /:year/:month/:filename/

taxonomies:
    category : categories
    tag : tags
    series : series
outputFormats:          # use for search. recommend not to modify 
  SearchIndex:
    mediaType: "application/json"
    baseName: "searchindex"
    isPlainText: true
    notAlternative: true

outputs:
    home: ["HTML","RSS","SearchIndex"]  # recommend not to modify
# sitemap
sitemap:
  changefreq: monthly
  filename: sitemap.xml
  priority: 0.5

menu:
  main:
    - identifier: home
      name: Home
      title: Home
      url: /
      weight: 1

    - identifier: archives
      name: Archives
      title: Archives
      url: /posts/
      weight: 2

    - identifier: categories
      name: Categories
      title: Categories
      url: /categories/
      weight: 3

    - identifier: tags
      name: Tags
      title: Tags
      url: /tags/
      weight: 4

    - identifier: about
      name: About
      title: About
      url: /about/
      weight: 5


params:
  since: 2017
  dateFormatToUse: "2006-01-02"
  enablePostCopyright: true
  copyright_link: http://creativecommons.org/licenses/by/4.0/deed.zh
  # the directory under content folder that you want to render
  mainSections: ["posts"]
  # Enable/Disable menu icons
  # Icon Reference: http://blog.cofess.com/hexo-theme-pure/iconfont/demo_fontclass.html
  enableMathJax: true #Enable mathjax support, to use mathematical notations
  highlightjs:
    langs: ["python", "javascript"] # refer to http://staticfile.org/, search highlight.js, already have highlight.min.js

  tag_cloud:
    min: 8
    max: 20
  # Allows you to specify an override stylesheet
  # put custom.css in $hugo_root_dir/static/
  # customCSS: css/custom.css

  menuIcons:
    enable: true  # 是否启用导航菜单图标
    home: icon-home-fill
    archives: icon-archives-fill
    categories: icon-folder
    tags: icon-tags
    repository: icon-project
    books: icon-book-fill
    links: icon-friendship
    about: icon-cup-fill

  # profile
  profile:
    enabled: true # Whether to show profile bar
    avatar: avatar.png
    gravatar: # Gravatar email address, if you enable Gravatar, your avatar config will be overriden
    author: xiaoheiAh
    author_title: author title
    author_description: Good Good Study, Day Day Up~
    location: Shanghai, China
    follow: https://github.com/xiaoheiAh
    # Social Links
    social:
      links:
        github: https://github.com/xiaoheiAh
        # weibo: http://weibo.com/{yourid}
        # twitter: https://twitter.com/
        # facebook: /
        rss: /index.xml
      link_tooltip: false # enable the social link tooltip, options: true, false
  # Site
  site:
    logo:
      enabled: true
      width: 40
      height: 40
      url: favicon.ico
    title: Hugo # 页面title
    favicon: favicon.ico
    board: <p>enjoy~</p> # 公告牌

  # Share
  # weibo,qq,qzone,wechat,tencent,douban,diandian,facebook,twitter,google,linkedin
  share:
    enable: true # 是否启用分享
    sites: weibo,qq,wechat,facebook,twitter # PC端显示的分享图标
    mobile_sites: weibo,qq,qzone # 移动端显示的分享图标

  # Comment
  comment:
    type:  # type disqus/gitalk/valine 启用哪种评论系统
    disqus: your_disqus_name # enter disqus shortname here
    gitalk: # gitalk. https://gitalk.github.io/
      owner: #必须. GitHub repository 所有者，可以是个人或者组织。
      admin: #必须. GitHub repository 的所有者和合作者 (对这个 repository 有写权限的用户)。
      repo:  #必须. GitHub repository.
      ClientID: #必须. GitHub Application Client ID.
      ClientSecret: #必须. GitHub Application Client Secret.
    valine: # Valine. https://valine.js.org
      appid: # your leancloud application appid
      appkey: # your leancloud application appkey
      notify: # mail notifier , https://github.com/xCss/Valine/wiki
      verify: # Verification code
      placeholder: enjoy~ # comment box placeholder
      avatar: mm # gravatar style
      meta: nick,mail # custom comment header
      pageSize: 10 # pagination size
      visitor: false # Article reading statistic https://valine.js.org/visitor.html

  # Donate
  donate:
    enable: false
    # 微信打赏
    wechatpay:
      qrcode: donate/wechatpayimg.png
      title: 微信支付
    # 支付宝打赏
    alipay:
      qrcode: donate/alipayimg.png
      title: 支付宝

  # PV
  pv:
    busuanzi:
      enable: false # 不蒜子统计
    leancloud:
      enable: false # leancloud统计
      app_id: # leancloud <AppID>
      app_key: # leancloud <AppKey>

  # wordcount
  postCount:
    enable: true
    wordcount: true # 文章字数统计
    min2read: true # read time 阅读时长预计

  # config
  config:
    skin: theme-black # theme color default is white. other type [theme-black,theme-blue,theme-green,theme-purple]
    layout: main-center # main-left main-center main-right
    excerpt_link: Read More
    toc: true

  # Sidebar
  sidebar: right

  # Search
  search:
    enable: true # enable search. thanks for https://raw.githubusercontent.com/ppoffice/hexo-theme-icarus/master/source/js/insight.js

  # Sidebar only the following widgets. you can remove any you don't like it.
  widgets:
    - board
    - tag_cloud
    - category
    - tag
    - recent_posts



hugo-theme-stack

config.yaml

baseurl: https://leayoo.github.io/
languageCode: en-us
theme: hugo-theme-stack
paginate: 5
title: Example Site

# Change it to your Disqus shortname before using
disqusShortname: hugo-theme-stack

# GA Tracking ID
googleAnalytics:

# Theme i18n support
# Available values: en, fr, id, ja, ko, pt-br, zh-cn
DefaultContentLanguage: en

permalinks:
    post: /p/:slug/
    page: /:slug/

params:
    mainSections:
        - post
    featuredImageField: image
    rssFullContent: true
    favicon:

    footer:
        since: 2020
        customText:

    dateFormat:
        published: Jan 02, 2006
        lastUpdated: Jan 02, 2006 15:04 MST

    sidebar:
        emoji: 🍥
        subtitle: Lorem ipsum dolor sit amet, consectetur adipiscing elit.
        avatar:
            local: true
            src: img/avatar.png

    article:
        math: false
        license:
            enabled: true
            default: Licensed under CC BY-NC-SA 4.0

    comments:
        enabled: true
        provider: disqus

        utterances:
            repo:
            issueTerm: pathname
            label:

        remark42:
            host:
            site:
            locale:

    widgets:
        enabled:
            - search
            - archives
            - tag-cloud

        archives:
            limit: 5

        tagCloud:
            limit: 10

    opengraph:
        twitter:
            # Your Twitter username
            site:

            # Available values: summary, summary_large_image
            card: summary_large_image

    defaultImage:
        opengraph:
            enabled: false
            local: false
            src:

    colorScheme:
        # Display toggle
        toggle: true

        # Available values: auto, light, dark
        default: auto

    imageProcessing:
        cover:
            enabled: true
        content:
            enabled: true

### Custom menu
### See https://docs.stack.jimmycai.com/configuration/custom-menu
### To remove about, archive and search page menu item, remove `menu` field from their FrontMatter
menu:
    main:
        - identifier: home
          name: Home
          url: /
          weight: -100
          pre: home

related:
    includeNewer: true
    threshold: 60
    toLower: false
    indices:
        - name: tags
          weight: 100

        - name: categories
          weight: 200

markup:
    highlight:
        noClasses: false
