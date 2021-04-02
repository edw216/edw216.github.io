---
title:  "[Blog]깃 블로그 시작하기[2] - 메인 페이지 기본 설정"

categories:
  - Blog
tags:
  - [Blog, Jekyll, Github, Git, GitBlog]

toc: true
toc_sticky: true
 
date: 2021-04-02
last_modified_at: 2021-04-02
---


이전 포스팅에서는 블로그를 생성해봤으니 이번 포스팅에는 블로그의 메인페이지 설정을 해보려고 한다.<br>
![blog2-1](https://github.com/edw216/edw216.github.io/blob/master/assets/images/blog2/blog2-1.png?raw=true)
처음 블로그에 접속하면 Site Title, Your Name, Quick-Start Guide 등 기본 설정으로 되어있는 것이 보이는데 이 부분들을 나만의 블로그로 꾸며보려한다.<br>

우선 설정을 하기전에 minimal mistakes 블로그사이트의 구조에 대해서 알고 싶다면 [지킬 홈페이지](https://mmistakes.github.io/minimal-mistakes/docs/structure/)를 참고하면 된다.<br>


        minimal-mistakes
    ├── _data                      # data files for customizing the theme
    |  ├── navigation.yml          # main navigation links
    |  └── ui-text.yml             # text used throughout the theme's UI
    └── _config.yml                # site configuration


`_data`의 `navigation.yml`파일과 `_config.yml`파일의 기본 설정을 수정한다.

## navigation.yml 변경하기
---
navigation.yml파일을 변경하면 최상단 우측이 변경된다.<br>
처음 _data의 navigation.yml 파일을 열어보면 다음과 같은 기본형식이 보인다.<br>

    # main links
    main:
    - title: "Quick-Start Guide"
        url: https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/
    # - title: "About"
    #   url: https://mmistakes.github.io/minimal-mistakes/about/
    # - title: "Sample Posts"
    #   url: /year-archive/
    # - title: "Sample Collections"
    #   url: /collection-archive/
    # - title: "Sitemap"
    #   url: /sitemap/

기본 Quick start를 삭제해주고 나만의 navigation으로 만들어보자.<br>

    # main links
    main:
    - title: "About"
        url: "/about/"
    - title: "Category"
        url: "/categories/"
    # - title: "About"
    #   url: https://mmistakes.github.io/minimal-mistakes/about/
    # - title: "Sample Posts"
    #   url: /year-archive/
    # - title: "Sample Collections"
    #   url: /collection-archive/
    # - title: "Sitemap"
    #   url: /sitemap/

나의 대한 정보를 나타내는 About 과 포스터들의 카테고리별로 분류하여 보여주는 Category navigation을 생성해줬다.<br>
url은 quick-start와 같은 외부 링크가 아닌 내 블로그 페이지 링크를 걸어주었다.<br>

지킬에서 게시물은 post와 page로 분류 되는데 일반적인 블로그 포스트는 _posts에서 관리가 되고 사이트의 특정주소 포스트는 _pages에서 관리가 된다.<br>

navigation은 특정 주소 페이지로 이동하는것이기 때문에 _pages라는 폴더를 생성하여 about과 category에 대한 파일을 만들어준다.<br>


    ---
    permalink: /about/
    title: "About"
    excerpt: "Infomation About me "
    last_modified_at: 2021-04-02
    toc: true
    ---


생성한 `about.md` 파일의 최상단에 위와 같은 헤더를 추가해준다.<br>
permalink란 디렉토리의 고유주소이다. permalink에서 정의한 /about/이 고유주소가 되고 navigation에서 url:/about/ 을 통해 해당 파일의 경로로 이동을 시킨다.<br>
`_config.yml` 파일 맨 아래에 다음과 같은 _posts의 default설정이 보일 겁니다.<br>

    # Defaults
    defaults:
    # _posts
    - scope:
        path: ""
        type: posts
        values:
        layout: single
        author_profile: true
        read_time: true
        comments: # true
        share: true
        related: true
    
아래 부분에 _pages에 대한 default를 추가해줍니다.<br>

    # _pages 
    - scope:
        path: "_pages"
        type: pages
        values:
        layout: single
        author_profile: true

추가를 하면 navigation에 대한 설정이 끝납니다.<br>

자세한 파일 내용은 저의 [github](https://github.com/edw216/edw216.github.io/)를 참고해주세요.<br>


## _config.yml 변경하기
---

이제 최상단의 우측 설정을 완료했으니 최상단 좌측, 사이드바, 하단을 설정해준다.<br>

`_config.yml`파일은 디렉토리의 최상단파일로 메인페이지의 대부분의 설정을 다룬다.<br>


### Site Settings
---
    # Site Settings
    locale                   : "en-US"
    title                    : "LifeStudy's DevLog"
    title_separator          : "-"
    subtitle                 : # site tagline that appears below site title in masthead
    name                     : "LifeStudy"
    description              : "Let's make study a daily life"
    url                      : # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
    baseurl                  : # the subpath of your site, e.g. "/blog"
    repository               : # GitHub username/repo-name e.g. "mmistakes/minimal-mistakes"
    teaser                   : # path of fallback teaser image, e.g. "/assets/images/500x300.png"
    logo                     : # path of logo image to display in the masthead, e.g. "/assets/images/88x88.png"
    masthead_title           : # overrides the website title displayed in the masthead, use " " for no title

최상단에 `#Site Settings`주석부분의 title이 최상단 좌측에 사이트에 대한 제목으로 title만 변경해준다.<br>

### Site Author
---
    # Site Author
    author:
    name             : "LifeStudy"
    avatar           : "/assets/images/profile/myprofile.jpg"
    bio              : "Let's make study a daily life"
    location         : "South Korea"
    email            :
    links:
        - label: "Email"
        icon: "fas fa-fw fa-envelope-square"
        url: "mailto:ehddn216@gmail.com"
        - label: "Website"
        icon: "fas fa-fw fa-link"
        # url: "https://your-website.com"
        - label: "Twitter"
        icon: "fab fa-fw fa-twitter-square"
        # url: "https://twitter.com/"
        - label: "Facebook"
        icon: "fab fa-fw fa-facebook-square"
        # url: "https://facebook.com/"
        - label: "GitHub"
        icon: "fab fa-fw fa-github"
        url: "https://github.com/edw216"
        - label: "Instagram"
        icon: "fab fa-fw fa-instagram"
        # url: "https://instagram.com/"

`# Site Author` 주석은 저자에 대한 설정을 하는 부분이다.<br>
다음을 참고하여 설정하면 된다. links의 경우 사용하려는 부분의 url주석을 해제하고 형식에 맞게 링크url을 넣어주면 활성화가 된다.<br>

### Site Footer

    # Site Footer
    footer:
    links:
        - label: "Twitter"
        icon: "fab fa-fw fa-twitter-square"
        # url:
        - label: "Facebook"
        icon: "fab fa-fw fa-facebook-square"
        # url:
        - label: "GitHub"
        icon: "fab fa-fw fa-github"
        url: "https://github.com/edw216"
        - label: "GitLab"
        icon: "fab fa-fw fa-gitlab"
        # url:
        - label: "Bitbucket"
        icon: "fab fa-fw fa-bitbucket"
        # url:
        - label: "Instagram"
        icon: "fab fa-fw fa-instagram"
        # url:

`#Site Footer` 주석 부분은 맨 하단에 저자에 대한 sns정보가 보일 것이다.

default 로 FOLLOW : FEED가 보이는데 그곳에 나는 github를 추가해줬다. 


