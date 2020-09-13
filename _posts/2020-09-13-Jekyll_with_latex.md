---
title: "[Develop] Jekyll에서 Latex 사용하기"
date: 2020-09-13
categories: [Develop]
tag: [Blog, Develop, Latex]
toc: true
toc_sticky: true
use_math: true
---

# Jekyll에서 Latex 사용하기

Data Science관련 포스팅을 작성하기 위해서는 수식 작성이 필수이다.
평소에 생각없이 markdown과 latex를 함께 사용하고, 별 문제 없다고 생각했는데 이전 포스팅에서 수식이 표현되지 않았다는 것을 알게되어  방법을 찾아보았다.

## MathJax

Jekyll에서 latex를 사용하기 위해서는 [MathJax](http://docs.mathjax.org/en/latest/web/start.html)를 이용한다.

### 1. _config.yml 설정

Jekyll 설정 (_config.yml)에서 사용할 markdown 렌딩 엔진을 **kramdown**으로 변경한다. 사실 default이기 때문에 크게 신경쓸 필요는 없다.

또한 highlighter를 **rouge**로 세팅한다. (역시 default)

```yml
# Conversion
markdown: kramdown
highlighter: rouge
lsi: false
excerpt_separator: "\n\n"
incremental: false
```

그리고 kramdown 세팅 역시 수정한다.

```yml
# Markdown Processing
kramdown:
  input: GFM
  math_engine: mathjax
  ...
```

### 2. page에서 MathJax support파일 생성

아래 경로에 파일을 만들어 아래와 같이 입력한다. mathjax의 설정과 라이브러리 로드를 한다.
```_includes/mathjax_support.html```

```html
{% raw %}
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    TeX: {
        equationNumbers: {
        autoNumber: "AMS"
        }
    },
    tex2jax: {
    inlineMath: [ ['$', '$'], ["\\(","\\)"]  ],
    displayMath: [ ['$$', '$$'], ["\\[","\\]"]  ],
    processEscapes: true,
    }
});
MathJax.Hub.Register.MessageHook("Math Processing Error",function (message) {
        alert("Math Processing Error: "+message[1]);
    });
MathJax.Hub.Register.MessageHook("TeX Jax - parse error",function (message) {
        alert("Math Processing Error: "+message[1]);
    });
</script>

<script type="text/javascript" async
    src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
{% endraw %}
```

### 3. 실제 포스팅 페이지에서 로드하도록 수정

아래 경로의 header에 아래 내용을 복사하여 위에서 만든 support 파일을 include하도록 한다.
```_layouts/default.html```

```html
{% raw %}
<head>
    {% if page.use_math %}
        {% include mathjax_support.html %}
    {% endif %}
</head>
{% endraw %}
```

### 4. 실제 사용

포스팅 작성시 아래 구문을 통해 latex 수식을 작성할 수 있다.

* latex 문법

```
$Y = X + b$
```

* 실제 포스팅 표현

$Y = X + b$