# HTML의 기본 개념과 문서 구조

## HTML이란?

HTML(HyperText Markup Language)은 웹 페이지를 만들기 위한 기본 언어입니다. 웹 브라우저가 이해할 수 있는 마크업 언어로, 텍스트, 이미지, 링크 등의 콘텐츠를 구조화하여 표현합니다.

### HTML의 특징
- **마크업 언어**: 태그(Tag)를 사용하여 콘텐츠를 표시
- **구조화된 문서**: 계층적 구조로 정보를 조직화
- **플랫폼 독립적**: 모든 운영체제와 브라우저에서 동작
- **확장 가능**: CSS, JavaScript와 함께 사용하여 기능 확장

## 기본 HTML 문서 구조

### 완전한 HTML 문서 예제
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>페이지 제목</title>
</head>
<body>
    <h1>안녕하세요!</h1>
    <p>첫 번째 HTML 페이지입니다.</p>
</body>
</html>
```

### 각 요소별 설명

#### 1. DOCTYPE 선언
```html
<!DOCTYPE html>
```
- HTML5 문서임을 브라우저에 알림
- 반드시 문서의 맨 첫 줄에 위치
- 대소문자 구분하지 않음

#### 2. html 요소
```html
<html lang="ko">
```
- HTML 문서의 루트(최상위) 요소
- `lang` 속성으로 문서의 주 언어 지정
- 모든 다른 요소들의 부모 요소

#### 3. head 요소
```html
<head>
    <!-- 메타데이터가 들어가는 영역 -->
</head>
```
- 문서의 메타데이터(정보)를 포함
- 브라우저에는 표시되지 않음
- 검색엔진, 브라우저가 참조하는 정보

**head 내부에 포함되는 주요 요소들:**
```html
<head>
    <!-- 문자 인코딩 -->
    <meta charset="UTF-8">
    
    <!-- 반응형 웹을 위한 뷰포트 설정 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- 페이지 제목 (브라우저 탭에 표시) -->
    <title>페이지 제목</title>
    
    <!-- 페이지 설명 -->
    <meta name="description" content="페이지 설명">
    
    <!-- CSS 파일 연결 -->
    <link rel="stylesheet" href="style.css">
    
    <!-- JavaScript 파일 연결 -->
    <script src="script.js"></script>
</head>
```

#### 4. body 요소
```html
<body>
    <!-- 실제 콘텐츠가 표시되는 영역 -->
</body>
```
- 사용자에게 실제로 보여지는 콘텐츠
- 텍스트, 이미지, 링크, 버튼 등 모든 가시적 요소
- 한 문서에 하나만 존재

## HTML 태그의 기본 구조

### 태그의 구성요소
```html
<tagname attribute="value">내용</tagname>
```

1. **여는 태그**: `<tagname>`
2. **속성**: `attribute="value"`
3. **내용**: 태그 사이의 텍스트나 다른 요소
4. **닫는 태그**: `</tagname>`

### 태그 예제
```html
<!-- 기본 태그 -->
<h1>제목</h1>
<p>단락</p>

<!-- 속성이 있는 태그 -->
<a href="https://www.google.com">구글</a>
<img src="image.jpg" alt="이미지 설명">

<!-- 자체 닫힌 태그 (빈 요소) -->
<br>
<hr>
<img src="image.jpg" alt="설명">
<input type="text" name="username">
```

### 중첩 태그
```html
<!-- 올바른 중첩 -->
<p><strong>굵은 글씨</strong>와 <em>기울인 글씨</em></p>

<!-- 잘못된 중첩 (피해야 함) -->
<p><strong>굵은 글씨</p></strong>
```

## HTML 요소의 분류

### 1. 블록 레벨 요소 (Block-level Elements)
- 전체 너비를 차지
- 새 줄에서 시작
- 높이와 너비를 설정 가능

```html
<div>블록 요소</div>
<p>단락</p>
<h1>제목</h1>
<ul>
    <li>목록 항목</li>
</ul>
<section>섹션</section>
<article>아티클</article>
```

### 2. 인라인 요소 (Inline Elements)
- 내용만큼의 너비를 차지
- 같은 줄에 다른 요소와 함께 배치
- 높이와 너비 설정 불가

```html
<span>인라인 요소</span>
<a href="#">링크</a>
<strong>굵은 글씨</strong>
<em>기울임</em>
<img src="image.jpg" alt="이미지">
```

### 3. 인라인-블록 요소
- 인라인처럼 같은 줄에 배치
- 블록처럼 높이와 너비 설정 가능

```html
<img src="image.jpg" alt="이미지" width="200" height="100">
<input type="text" style="width: 200px;">
```

## 문서 구조의 모범 사례

### 1. 계층적 제목 구조
```html
<body>
    <h1>메인 제목</h1>
        <h2>섹션 제목</h2>
            <h3>서브섹션 제목</h3>
                <h4>세부 제목</h4>
        <h2>또 다른 섹션</h2>
            <h3>서브섹션</h3>
</body>
```

### 2. 논리적 문서 구조
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>구조화된 웹페이지</title>
</head>
<body>
    <!-- 헤더 영역 -->
    <header>
        <h1>사이트 제목</h1>
        <nav>네비게이션</nav>
    </header>
    
    <!-- 메인 콘텐츠 -->
    <main>
        <section>
            <h2>주요 내용</h2>
            <article>개별 글</article>
        </section>
        
        <aside>사이드바</aside>
    </main>
    
    <!-- 푸터 영역 -->
    <footer>
        <p>저작권 정보</p>
    </footer>
</body>
</html>
```

## HTML 주석

### 주석 작성법
```html
<!-- 이것은 주석입니다 -->
<p>이 텍스트는 표시됩니다</p>

<!-- 
여러 줄 주석도 가능합니다
이 부분은 브라우저에서 보이지 않습니다
-->

<div class="content">
    <!-- TODO: 나중에 스타일 추가 -->
    <p>콘텐츠</p>
</div>
```

### 주석 사용 팁
- 코드 설명을 위해 사용
- 임시로 코드를 비활성화할 때 활용
- 섹션 구분을 위한 표시
- 개발 중 메모나 할일 표시

## 문자 인코딩

### UTF-8 설정 (권장)
```html
<head>
    <meta charset="UTF-8">
</head>
```

### 다른 인코딩 예제
```html
<!-- EUC-KR (한국어 전용) -->
<meta charset="EUC-KR">

<!-- ISO-8859-1 (라틴 문자) -->
<meta charset="ISO-8859-1">
```

## 브라우저 호환성

### HTML5 지원 확인
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>HTML5 기능 확인</title>
</head>
<body>
    <!-- HTML5 새로운 입력 타입 -->
    <input type="email" placeholder="이메일을 입력하세요">
    <input type="date">
    <input type="color">
    
    <!-- HTML5 시맨틱 요소 -->
    <header>헤더</header>
    <nav>네비게이션</nav>
    <main>메인 콘텐츠</main>
    <aside>사이드바</aside>
    <footer>푸터</footer>
</body>
</html>
```

### 구버전 브라우저 대응
```html
<!--[if lt IE 9]>
<script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
<![endif]-->
```

## 실습 예제

### 기본 문서 구조 연습
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <!-- 메타데이터 영역 -->
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="HTML 기초 학습 페이지">
    <meta name="keywords" content="HTML, 웹개발, 마크업">
    <meta name="author" content="홍길동">
    
    <title>HTML 기초 - 문서 구조</title>
    
    <!-- 스타일 -->
    <style>
        body { font-family: Arial, sans-serif; }
        .container { max-width: 800px; margin: 0 auto; }
    </style>
</head>
<body>
    <div class="container">
        <!-- 페이지 헤더 -->
        <header>
            <h1>HTML 기초 학습</h1>
            <p>웹 개발의 첫걸음</p>
        </header>
        
        <!-- 메인 콘텐츠 -->
        <main>
            <section>
                <h2>HTML이란?</h2>
                <p>HyperText Markup Language의 줄임말로, 웹 페이지의 구조를 만드는 마크업 언어입니다.</p>
            </section>
            
            <section>
                <h2>HTML의 특징</h2>
                <ul>
                    <li>태그 기반 마크업</li>
                    <li>계층적 구조</li>
                    <li>의미론적 요소</li>
                </ul>
            </section>
        </main>
        
        <!-- 페이지 푸터 -->
        <footer>
            <p>&copy; 2023 HTML 학습 가이드</p>
        </footer>
    </div>
</body>
</html>
```

이 가이드를 통해 HTML의 기본 개념과 문서 구조를 이해하고, 올바른 HTML 문서를 작성하는 방법을 익혀보세요!
