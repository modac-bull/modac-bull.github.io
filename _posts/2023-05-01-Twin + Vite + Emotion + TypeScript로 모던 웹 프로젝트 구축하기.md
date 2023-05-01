---
title: "Twin + Vite + Emotion + TypeScript로 모던 웹 프로젝트 구축하기"
exerpt: "이 글에서는 Twin, Vite, Emotion, 그리고 TypeScript를 함께 사용하여 모던 웹 프로젝트를 구성하는 방법, 이러한 구성의 특징, 그리고 이 구성을 선택한 이유에 대해 알아봅니다."
last_modified_at: 2023-05-01T21:15:06
header:
  teaser: "assets/images/teaser.png"
categories:
  - settings
tags:
  - vite
  - emotion
  - typescript
  - twin
toc: true
---

# 글의 목적

이 글의 목적은 '나'의 기억력을 믿지 못하기 때문이며 미래의 '나'에게 설명하는 마음으로 작성했습니다. 해당 내용이 부족하거나 잘못된 부분이 있으면 너그러운 마음으로 이해해주시기 바랍니다.

이 글은 Twin + Vite + Emotion + Typescript로 모던 웹을 구축했던 배경과 과정을 기록으로 남기기 위해 작성한 글입니다. 프로젝트 세팅 과정은 ben-rogerson/twin.examples[^1]를 참고했습니다. 이곳에 세팅 과정에 대한 설명이 상세하게 기술되어 있습니다. Vite 패키지 뿐만 아니라 twin.macro롤 다양한 패키지, next.js 등에 세팅하는 예시 데모도 있습니다. 


[^1]: <https://github.com/ben-rogerson/twin.examples/tree/master/vite-emotion-typescript>

# 프로젝트 구성 요소 소개
본 프로젝트에서 사용되는 주요 기술 요소들에 대해 간략하게 소개합니다.

- Vite: 빠른 개발 환경과 최적화된 빌드를 제공하는 프론트엔드 빌드 도구입니다. 개발 서버를 제공하며, Rollup 기반의 빌드로 최적화를 지원합니다.
- TypeScript: 정적 타입 검사와 최신 JavaScript 기능을 제공하는 슈퍼셋 언어입니다. 런타임 오류를 줄이고 유지 보수성을 높여줍니다.
- Emotion: 컴포넌트 기반의 CSS-in-JS 라이브러리로서, 스타일과 컴포넌트의 관심사를 분리하며 동적 스타일링에 유연하게 대처할 수 있습니다.
- Twin.macro: Emotion과 결합하여 Tailwind CSS를 사용할 수 있게 해주는 라이브러리입니다. 이를 통해 반응형 디자인을 빠르게 구현하고 일관된 디자인 시스템을 제공할 수 있습니다.

# 프로젝트 구성 선택 이유

Twin, Vite, Emotion, Typescript를 활용해 프로젝트를 구성하게 된 이유는 다음과 같습니다.
1. 기존에 Unity Class를 활용한 스타일 작업 방식에 익숙했습니다. 이번 기회에 tailwind의 Unity Class 방식을 경험해보고 싶었습니다.
2. CSS-in-JS 방식이 스타일과 컴포넌트를 분리하며 동적 스타일링에 이점이 있다고 했는데 이를 경험해보고 싶었습니다.
3. Vite 패키지를 선택한 이유는 주변에서 Vite 패키지의 속도가 빠르다는 이야기를 자주했었습니다. 개발 환경에서, 빌드할 때의 속도가 얼마나 빠른지 경험해보고 싶었습니다.
4. Typescript는 요즘 프론트엔드에서는 필수인만큼 기본 전제로 선택했습니다. 

# 프로젝트 설정하기
Twin, Vite, Emotion, Typescript를 함께 사용하는 프로젝트 설정하는 방법을 단계별로 설명합니다.
## 1. Vite를 이용한 리액트 프로젝트 생성
## 2. Emotion과 twin.macro 설치
## 3. Babel 설정
## 4. TypeScript 설정
## 5. Vite 설정
## 6. twin.macro 설정
## 7. ESLint 설정 (선택 사항)

# 구성 요소별 특징 분석


# 결론


### GFM Code Blocks

GitHub Flavored Markdown [fenced code blocks](https://help.github.com/articles/creating-and-highlighting-code-blocks/) are supported. To modify styling and highlight colors edit `/_sass/syntax.scss`.

```css
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
```

{% highlight scss %}
.highlight {
margin: 0;
padding: 1em;
font-family: $monospace;
font-size: $type-size-7;
line-height: 1.8;
}
{% endhighlight %}

```html
{% raw %}
<nav class="pagination" role="navigation">
  {% if page.previous %}
  <a
    href="{{ site.url }}{{ page.previous.url }}"
    class="btn"
    title="{{ page.previous.title }}"
    >Previous article</a
  >
  {% endif %} {% if page.next %}
  <a
    href="{{ site.url }}{{ page.next.url }}"
    class="btn"
    title="{{ page.next.title }}"
    >Next article</a
  >
  {% endif %}
</nav>
<!-- /.pagination -->{% endraw %}
```

```ruby
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
```

### Code Blocks in Lists

Indentation matters. Be sure the indent of the code block aligns with the first non-space character after the list item marker (e.g., `1.`). Usually this will mean indenting 3 spaces instead of 4.

1. Do step 1.
2. Now do this:

   ```ruby
   def print_hi(name)
     puts "Hi, #{name}"
   end
   print_hi('Tom')
   #=> prints 'Hi, Tom' to STDOUT.
   ```

3. Now you can do this.

### Jekyll Highlight Tag

An example of a code blocking using Jekyll's [`{% raw %}{% highlight %}{% endraw %}` tag](https://jekyllrb.com/docs/templates/#code-snippet-highlighting).

{% highlight javascript linenos %}
// 'gulp html' -- does nothing
// 'gulp html --prod' -- minifies and gzips HTML files for production
gulp.task('html', () => {
return gulp.src(paths.siteFolderName + paths.htmlPattern)
.pipe(when(argv.prod, htmlmin({
removeComments: true,
collapseWhitespace: true,
collapseBooleanAttributes: false,
removeAttributeQuotes: false,
removeRedundantAttributes: false,
minifyJS: true,
minifyCSS: true
})))
.pipe(when(argv.prod, size({title: 'optimized HTML'})))
.pipe(when(argv.prod, gulp.dest(paths.siteFolderName)))
.pipe(when(argv.prod, gzip({append: true})))
.pipe(when(argv.prod, size({
title: 'gzipped HTML',
gzip: true
})))
.pipe(when(argv.prod, gulp.dest(paths.siteFolderName)))
});
{% endhighlight %}

{% highlight wl linenos %}
Module[{},
Sqrt[2]
4
]
{% endhighlight %}
