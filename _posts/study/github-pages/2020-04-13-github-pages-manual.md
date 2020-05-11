---
title: "GitHub Pages 메뉴얼 (v0.1)"
excerpt: "GitHub Pages 사용 방법 정리"

categories:
    - study
    - github-pages
tags:
    - github-pages

header:
  teaser: /assets/images/github-pages/page-header-teaser.png
  og_image: /assets/images/github-pages/page-header-og-image.png

gallery:
  - url: /assets/images/github-pages/unsplash-gallery-image-1.jpg
    image_path: /assets/images/github-pages/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
    title: "Image 1 title caption"
  - url: /assets/images/github-pages/unsplash-gallery-image-2.jpg
    image_path: /assets/images/github-pages/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Image 2 title caption"
  - url: /assets/images/github-pages/unsplash-gallery-image-3.jpg
    image_path: /assets/images/github-pages/unsplash-gallery-image-3-th.jpg
    alt: "placeholder image 3"
    title: "Image 3 title caption"

last_modified_at: 2020-04-13T15:40:00+09:00
---


<div class="notice--info" markdown="1">
* 사용한 Theme는 `minimal-mistakes`입니다. 포스트 작성할 때 기본적인 문법은 아래 문서 참고 부탁드립니다.
  * [Quick Start Guide](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
  * [GitHub Pages Example](https://mogals.github.io/year-archive/)
* 해당 블로그는 아직 구글 검색 엔진에 등록되지 않은 상태입니다.
</div>

## 설치

##### Git

```bash
$ git clone https://github.com/rokebi/rokebi.github.io.git
$ cd rokebi.github.io
```

##### Ruby 설치

아래 커맨드로 먼저 Ruby가 설치되어 있는지 확인한다.

```bash
$ ruby -v
```

Ruby가 설치 안 됐으면 [Ruby 설치 가이드](https://www.ruby-lang.org/ko/documentation/installation/) 문서를 참조해서 Ruby를 설치한다.
Mac OS의 경우 `Homebrew`를 통해 간단하게 설치할 수 있다.

```bash
$ brew install ruby
```

##### Jekyll 과 bundler 설치

Ruby를 설치하면 `gem` 명령어를 통해 필요한 Ruby 프로그래밍 패키지를 설치할 수 있다.
현재 이 블로그는 Jekyll을 사용하므로 Jekyll과 Bundler를 설치한다. 
Budler를 설치하고 나면 `bundle` 명령어를 실행한다. 이 명령어는 Gemfile을 참조하여 필요한 목록을 설치한다.

```bash
$ gem install jekyll bundler
$ bundle
```

##### 페이지 실행

모든 설치가 완료되면 아래 명령어를 실행하고 `127.0.0.1:4000`으로 웹페이지 접속한다.
로깨비 기술블로그와 동일한 화면이 나오는지 확인한다. 이 명령어를 통해 Git에 올리기 전 웹페이지를 미리 확인할 수 있다.

```bash
$ bundle exec jekyll serve
```




## 메뉴 구성

* **About:** 블로그 소개. 향후 내용 추가 예정
* **개발 노트:** 로깨비 서비스 개발 관련 콘텐츠 추가 예정
* **스터디:** 기타 개발 관련 콘텐츠 추가 예정 

##### 파일 구성
메뉴는 `_data/navigation.yml` 에서 관리한다.
```yaml
main:
  - title: "About"
    url: /about
  - title: "개발 노트"
    url: /dev-notes
  - title: "스터디"
    url: /studies
```
이때 메뉴 별 url은 `_pages` 아래 md 파일과 매핑되는 구조이다.
```terminal
├── _pages
|  ├── about.md
|  ├── dev-notes.md
|  └── studies.md
```

##### 메뉴 포스트 관리
메뉴는 1차 카테고리와 동일하며 2차 카테고리가 실제 카테고리 역할을 하도록 설정되어 있다.
그렇기 때문에 포스트 작성 시 카테고리는 총 2개를 작성해야 하며, 1차 카테고리가 먼저 오도록 한다.

```
categories:
    - 1차 카테고리 (study | dev-note)
    - 2차 카테고리
```

*예시*
```markdown
---
title: "GitHub Pages 메뉴얼"
excerpt: "GitHub Pages 사용 방법 정리"

categories:
    - study
    - github-pages
tags:
    - github-pages
---
```

이렇게 카테고리를 작성하면 자동으로 메뉴에 등록된다.
현재 각 메뉴는 카테고리 별로 포스트 최대 3개까지 나열하도록 설정되어 있다.

##### 카테고리 관리

Jekyll v4.0은 아직 `slugified_categories` 기능을 지원하지 않는 관계로 (v4.1 부터 지원 예정)
카테고리는 설정 파일 `_data/categories.yml` 을 통해 이름과 값을 관리한다.

```yaml
- name: 개발 노트
  slug: dev-note
- name: 스터디
  slug: study
- name: GitHub Pages
  slug: github-pages
```

`name`은 포스트에 표시되는 카테고리 이름이고 `slug`는 카테고리의 실제 값이다. 포스트에는 `slug` 값을 입력한다.
현재 포스트에는 태그도 지원하고 있는데 태그는 `_data/tags.yml` 별도의 파일에 관리한다. 방법은 카테고리와 동일하다.

##### 포스트 작성

포스트는 편의 상 카테고리 별로 디렉토리를 생성하여 관리한다.

```terminal
├── _posts
|  ├── dev-note
|  └── study
|     └── github-pages
```

포스트 생성 시 파일 이름은 `YYYY-MM-DD-{file-title}.md` 이렇게 날짜 형식을 포함하여 작성한다.<br />
파일을 생성하고 파일 최상단에는 파일에 대한 정보를 적는다.

```markdown
---
title: "포스트 제목"
excerpt: "포스트에 대한 간략한 설명. 메뉴에서 포스트를 나열할 때 해당 설명이 제목 밑에 표시된다."

categories:
    - 1차 카테고리
    - 2차 카테고리
tags:
    - 태그

last_modified_at: 2020-04-03T15:15:00+09:00 #수정시각. 직접 수정해야 한다
---

파일 내용 <!-- 기본적으로 Mark down 형식을 따른다. -->

```

추가로, 포스트 URL 형식은 `_config.yml` 에서 관리하며 현재 `/:categories/:year/:month/:title` 형식을 따른다. 
* `:categories`: 각 카테고리들의 값들이 URL로 치환된다. 
* `:year`,`:month`,`:title`: 파일명에 있는 값들을 참조한다.

##### 이미지 첨부

이미지 파일은 `assets/images` 디렉토리에서 관리한다. 첨부하려는 이미지 파일을 해당 디렉토리에 저장한다.
포스트에 이미지 첨부 방식은 크게 2가지로 나뉜다. 포스트 최상단에서 설정으로 정의하여 묶음 형태로 보여주는 `gallery` 방식과
포스트 중간에 liquid 문법으로 추가하는 `caption` 방식으로 나뉜다.

* Gallery

포스트 최상단에 이렇게 이미지 묶음을 선언한다. 또한 url과 image_path에 외부 이미지 url 값을 넣어 외부 이미지를 참조하도록 할 수 있다. 혹은 url에 외부 링크를 넣어 클릭 시 페이지 이동하도록 설정 가능하다.

```yaml
gallery:
  - url: /assets/images/github-pages/unsplash-gallery-image-1.jpg
    image_path: /assets/images/github-pages/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
    title: "Image 1 title caption"
  - url: /assets/images/github-pages/unsplash-gallery-image-2.jpg
    image_path: /assets/images/github-pages/unsplash-gallery-image-2-th.jpg
    alt: "placeholder image 2"
    title: "Image 2 title caption"
  - url: /assets/images/github-pages/unsplash-gallery-image-3.jpg
    image_path: /assets/images/github-pages/unsplash-gallery-image-3-th.jpg
    alt: "placeholder image 3"
    title: "Image 3 title caption"
```

갤러리 선언 후에는 아래와 같은 liquid 문법으로 포스트 중간에 삽입할 수 있다. 이 때 `caption` 값을 통해 이미지 아래 글귀를 삽입할 수 있다.

```liquid
{% raw %}{% include gallery caption="This is a sample gallery with **Markdown support**." %}{% endraw %}
```

*결과*
{% include gallery caption="This is a sample gallery with **Markdown support**." %}

갤러리 이름은 똑같이 지을 필요는 없으나, 다른 경우엔 include 문 안에 `id` 변수를 통해서 값을 넣어줘야 한다.

```yaml
another_gallery:
  - url: /assets/images/github-pages/unsplash-gallery-image-1.jpg
    image_path: /assets/images/github-pages/unsplash-gallery-image-1-th.jpg
    alt: "placeholder image 1"
    title: "Image 1 title caption"
```

```liquid
{% raw %}{% include gallery id="another_gallery" caption="This is a sample gallery with **Markdown support**." %}{% endraw %}
```

`class="full"` 옵션을 통해 이미지를 콘테이너 꽉차게 표시할 수 있다.

```liquid
{% raw %}{% include gallery class="full" %}{% endraw %}
```

*결과*
{% include gallery class="full" %}

`layout` 옵션을 통해 이미지 컬럼 레이아웃을 변경할 수 있다.

```liquid
{% raw %}{% include gallery layout="half" %}{% endraw %}
```

*결과*
{% include gallery layout="half" %}

* Caption

포스트 최상단에 정의할 필요 없이 liquid 문법으로 이미지를 변수 형식으로 선언하여 사용 가능하다.

```liquid
{% raw %}{% capture 변수_이름 %}![name of the image]({{ '/assets/images/github-pages/unsplash-gallery-image-3.jpg' | relative_url }}){% endcapture %}

<figure>
  {{ 변수_이름 | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Photo from Unsplash.</figcaption>
</figure>{% endraw %}
```

*결과*
{% capture fig_img_sample_1 %}![name of the image]({{ '/assets/images/github-pages/unsplash-gallery-image-3.jpg' | relative_url }}){% endcapture %}

<figure>
  {{ fig_img_sample_1 | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Photo from Unsplash.</figcaption>
</figure>

Caption 방식으로 외부 링크를 걸을 때는 아래와 같이 선언하면 된다. Caption 글귀도 변수 형식으로 사용할 수 있다.

```liquid
{% raw %}{% capture fig_img %}
[![Foo](https://images.unsplash.com/photo-1541943869728-4bd4f450c8f5?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=800&fit=max&ixid=eyJhcHBfaWQiOjF9)](https://unsplash.com/)
{% endcapture %}

{% capture fig_caption %}
Stairs? Were we're going we don't need no stairs.
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>{{ fig_caption | markdownify | remove: "<p>" | remove: "</p>" }}</figcaption>
</figure>{% endraw %}
```

*결과*
{% capture fig_img %}
[![Foo](https://images.unsplash.com/photo-1541943869728-4bd4f450c8f5?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=800&fit=max&ixid=eyJhcHBfaWQiOjF9)](https://unsplash.com/)
{% endcapture %}

{% capture fig_caption %}
Stairs? Were we're going we don't need no stairs.
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>{{ fig_caption | markdownify | remove: "<p>" | remove: "</p>" }}</figcaption>
</figure>

##### 헤더 이미지

*추가 예정* <br />
*아직 블로그 레이아웃 구조 상 헤더 이미지가 표시되지 않음*

```yaml
header:
  teaser: /assets/images/github-pages/page-header-teaser.png
  og_image: /assets/images/github-pages/page-header-og-image.png
```

<br />
<br />