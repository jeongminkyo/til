### SASS(Syntactically Awesome Style Sheets)란?

- CSS를 효율적으로 작성할 수 있도록 도와주는 프로그램이다.
- 기존의 CSS의 유지보수의 불편함 등을 SASS를 사용하면 해결 할 수 있다. 
- 위에서 언급한 CSS의 단점을 보완하기 위한 기술로, SASS 자체를 그대로 사용할수는 없고, SASS의 문법에 맞게 SASS파일을 만들면 컨버터를 이용해서 CSS를 생성한다. 
- 즉, SASS문법에 맞게 CSS를 작성하고, SASS 컴파일러를 사용하여 HTML이 이해 할 수 있는 문법으로 변환한다.
- 반복되는 코드를 한 번의 선언으로 여러 곳에서 재사용할 수 있도록 도와주는 일종의 CSS 전처리기(pre-processor)이다.



### 전처리기란?

전처리기는 임의의 소스 파일을 가져와서 브라우저가 이해할 수 있는 것으로 변환시켜주는 도구이다. 전처리기는 소스맵이라고 불리는 기능을 지원하는 데, 소스맵은 일종의 JSON 기반 매핑 형식으로, 축소된 파일과 그 소스 사이의 관계를 만들어주는 역할을 한다. 프로덕션용으로 빌드하면 자바스크립트 파일을 축소하고 조합하는 작업 외에도 원본 파일에 대한 정보를 담은 소스 맵을 만들게 된다.

CSS 전처리기는 CSS 파일을 만들 때마다 컴파일된 CSS에 더하여 소스 맵 파일(.map)도 생성한다. 이 소스 맵 파일은 JSON 파일로 각각의 생성된 CSS 선언과 소스 파일에서 해당 줄 사이의 매핑을 정의한다.각 CSS 파일에는 자신의 소스 맵 파일을 지정하는 주석이 포함되어 있고, 이는 파일의 마지막 줄에 달린 특별 주석(special comment)에 포함된다.

Ex)

~~~scss
%$textSize: 26px;
$fontColor: red;
$bgColor: whitesmoke;
h2 {
    font-size: $textSize;
    color: $fontColor;
    background: $bgColor;
}
~~~

Sass가 **styles.css**라는 CSS 파일을 만들고, 여기에 sourceMappingURL 주석이 지정된다.

~~~css
h2 {
  font-size: 26px;
  color: red;
  background-color: whitesmoke;
}
/*# sourceMappingURL=styles.css.map */
~~~

아래는 소스 맵 파일의 예시이다.

~~~json
{
  "version": "3",
  "mappings":"AAKA,EAAG;EACC,SAAS,EANF,IAAI;EAOX,KAAK"
  "sources": ["sass/styles.scss"],
  "file": "styles.css"
}
~~~



##### 자바스크립트

| 컴파일러                                     | 명령어                                      | 안내                                       |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| [CoffeeScript](http://coffeescript.org/#source-maps) | `$ coffee -c square.coffee -m`           | 컴파일러에서 소스 맵을 출력하려면 m(--map) 플래그만 있으면 됩니다. 이렇게 하면 sourceMapURL comment pragma를 출력된 파일에 추가하는 작업도 처리해줍니다. |
| [TypeScript](http://www.typescriptlang.org/) | `$ tsc -sourcemap square.ts`             | -sourcemap 플래그는 맵을 생성하고 comment pragma를 추가합니다. |
| [Traceur](https://github.com/google/traceur-compiler/wiki/SourceMaps) | `$ traceur --source-maps=[file|inline]`  | `--source-maps=file`을 사용하면 `.js`로 끝나는 모든 출력 파일에 `.map`으로 끝나는 소스 맵 파일이 생성되고`source-maps='inline'`을 사용하면 `.js`로 끝나는 모든 출력 파일이 `data:` URL로 인코딩된 소스 맵을 포함하는 주석으로 끝나게 됩니다. |
| [Babel](https://babeljs.io/docs/usage/cli/#compile-with-source-maps) | `$ babel script.js --out-file script-compiled.js --source-maps` | --source-maps 또는 -s를 사용하여 소스 맵을 생성합니다. 인라인 소스 맵의 경우 `--source-maps inline`을 사용합니다. |
| [UglifyJS](https://github.com/mishoo/UglifyJS2) | `$ uglifyjs file.js -o file.min.js --source-map file.min.js.map` | 이는 'file.js'의 소스 맵을 만드는 데 필요한 매우 기본적인 명령입니다. 이 명령은 출력 파일에 comment pragma도 추가합니다. |

##### CSS

| 컴파일러                                     | 명령어                                      | 안내                                       |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| [Sass](http://sass-lang.com/)            | `$ scss --sourcemap styles.scss styles.css` | Sass에서 소스 맵은 Sass 3.3 이후부터 지원됩니다.        |
| [Less](http://lesscss.org/)              | `$ lessc styles.less > styles.css --source-map styles.css.map` | 1.5.0 버전에서 구현되었습니다. 자세한 내용과 사용 패턴은 [issue #1050](https://github.com/less/less.js/issues/1050#issuecomment-25566463)을 참조하세요. |
| [Stylus](https://learnboost.github.io/stylus/) | `$ stylus --sourcemaps styles.style styles.css` | 이 명령은 소스 맵을 출력 파일에 직접 base64로 인코딩된 문자열 형식으로 삽입합니다. |
| [Compass](http://compass-style.org/)     | `$ sass --compass --sourcemap --watch scss:css` | 또는 config.rb 파일에 `sourcemap: true`를 추가할 수도 있습니다. |
| [Autoprefixer](https://github.com/postcss/autoprefixer) | ``                                       | 링크를 따라가면 이를 사용하는 방법과 입력 소스 맵을 포함하는 방법을 확인할 수 있습니다. |



### SASS의 등장배경

CSS는 HTML을 디자인하는 언어다. CSS 단순성은 CSS의 미덕이지만, 규모가 조금만 커져도 CSS 유지보수하는 것은 어렵거나 불가능한 일이 된다. 자바스크립트와 같은 동적인 언어는 변수나 함수를 이용해서 코드의 재활용성을 높이고 코드의 양을 줄여주는데 반해서 CSS는 효율이 떨어진다.



### SASS이 장점

- 코드 중복을 줄일 수 있다.

>  CSS의 특성으로 인해서 셀렉터를 중복해서 사용해야 하는 경우가 많은데 Sass의 Nesting을 이용하면 코드의 양을 줄이고 연관된 코드끼리 그룹핑할 수 있다.

**SASS 사용 전 코드**

~~~css
#navbar {
  width: 80%;
  height: 23px; 
}
#navbar ul {
    list-style-type: none; 
}

~~~

**SCSS 사용 후 코드**

~~~scss
#navbar {
  width: 80%;
  height: 23px;
 
  ul { list-style-type: none; }
}

~~~

- 변수를 사용 할 수 있기 때문에 유지보수가 쉬워진다.

> CSS내에서 변수를 사용할 수 있다. 변수이름은 '$'로 시작해야 하고, 변수의 값으로 올 수 있는 것은 문자, 숫자(px, em포함), 칼러(#ce4dd6)가 있다. 변수를 이용하면 크기나 색상과 같은 값을 일괄적으로 변경할 수 있다.

~~~scss
$main-color: #ce4dd6;
$style: solid;
 
#navbar {
  border-bottom: {
    color: $main-color;
    style: $style;
  }
}
 
a {
  color: $main-color;
  &:hover { border-bottom: $style 1px; }
}

~~~

- 함수와 연산자를 사용 할 수 있다.



### SASS와 LESS의 차이

##### less

1. 자바스크립트 문법을 취하고 있으며, Node.js 엔진을 기반으로 구동된다.
2. Sass와 달리 브라우저단에서도 동작하는 특징을 갖고 있다.

##### 차이점

1.  LESS는 변수의 접두어로 @를 사용하고 SASS는 $를 사용한다는 점이 다르다.
2. LESS에서만 제공하는 기능으로는 유효 범위(scope)가  있다.

```css
@color : red;
#header {
    @color : blue;
    border: 1px solid @color;
}
#footer {
    border: 1px solid @color;
}

```

@를 $로 바꾸면 SASS용 코드도 된다. 하지만 LESS에서 컴파일한 CSS와 SASS에서 컴파일한 결과는 서로 다른데, LESS에서는 #header 블럭 안에서 정의한 변수가 전역 변수에 영향을 미치지 않기 때문이다. 반면, SASS에서는 전역/지역 변수의 구분이 없기 때문에 세 번째 줄의 코드를 실행하면 첫 번째 줄에서 정의한 @color의 값이 blue로 변경된다.

다음은 위의 코드를 각각 LESS와 SASS에서 컴파일한 결과이다.

```less
/* LESS */
#header { border: 1px solid blue; }
#footer { border: 1px solid red; }

/* SASS */
#header { border: 1px solid blue; }
#footer { border: 1px solid blue; }

```

3. SASS에서만 제공하는 기능으로서, 선택자 상속을 제공한다. 앞서 정의한 CSS 규칙에 선택자를 추가해주는 기능을 한다.

```css
.error {
    border : 1px solid red;
    background-color : #fdd;
}
.seriousError {
    @extend .error;
    border-width : 3px;
}

```

위 코드를 컴파일하면 다음과 같다.

```scss
.error, .seriousError {
    border : 1px solid red;
    background-color : #fdd;
}
. seriousError {
    border-width : 3px;
}

```



### SASS와 SCSS의 차이

Sass 가 처음 릴리즈 되었을 때, 주 문법은 CSS와 많이 달랐다. 괄호 `{ }` 대신 들여쓰기 (indentation) 을 사용하였으며 세미콜론 `;` 을 사용하지 않고 단축 연산자를 사용하였다.

```scss
=myclass // =means @minxin
  font-size: 12px
  
p
  +myclass // +means @include
```

그 시절, 일부 개발자는 이 새로운 문법에 익숙하지 않아서, Sass 버전 3 이상부터는 주 문법이 **.scss** 로 변경되었다.
SCSS 는 CSS 의 상위집합으로서, CSS와 동일한 문법으로 SASS 의 특별한 기능들이 추가되어있다.



### SASS 특징

**[변수 Variables]**

  작성방법 – $변수명 : 속성값;

**scss 코드**

~~~scss
$display-default:block;
$color-point:#080;
.lnk{
display:$display-default;
position:absolute;
left:5px;
color:$color-point;
}
~~~

**css 변환코드**

~~~Css
.lnk {
display: block;
position: absolute;
left: 5px;
color: #080;
}
~~~



##### **[중첩 Nesting]**

~~~Css
.info{background:#fff}
.info:before{display:block;content:''}
.info .tit{font-size:16px}
.info .lnk{display:block;margin-top:9px}
~~~

**&** : Referencing Parent Selectors(부모참조선택자) 
  &기호로 부모 선택자를 참조할 수 있습니다.

**scss 코드**

~~~Scss
.info{
background:#fff;
      &.tit{
font-size:16px;
}
.tit{
font-size:11px;
}
}
~~~

**css 변환코드**

~~~Css
.info {
background: #fff;
}
.info.tit {
font-size: 16px;
}
.info .tit {
font-size: 11px;
}
~~~

 &선택자를 사용한 .tit클래스는 .info의 복합선택자 속성값이 적용되지만, 그렇지 않은 .tit클래스는 .info 하위에 있는 자식선택자로 속성값이 적용된다. &선택자의 사용여부에 따라 부모 친구선택자(복합선택자)가 되느냐, 부모를 따르는 자식선택자가 되느냐로 구분할 수 있다.



##### [불러오기 Import]

Sass도 css처럼 import가 가능하다. 차이점은, css는 import된 각각의 .css파일의 로딩을 http에 요청해야한다면, sass는 여러개의 .scss파일을 import해도 최종적으로는 하나의 .css로 변환해주기 때문에 http에 요청을 여러번 보낼 필요가 없다.

\* 작성방법 – @import “파일명.scss”; or @import “파일명”;

**scss 코드**

~~~scss
@import ‘test2.scss’;	//test2.scss  $color-test:#000;
$color-main:#333;
.test{
font-color:$color-main;
}
.test2{
font-color:$color-test;
}
~~~

**css 변환코드**

~~~css
.test{
font-color:#333;
}
.test2{
font-color:#000;
}
~~~



##### [Mixin]

스타일을 클래스로 만들어놓고 필요한 위치에서 클래스를 추가로 넣어주거나, background-image가 들어가는 각각의 요소에 글자를 숨기기 위해 일부 동일한 속성을 반복해서 써야하는 번거로움. sass에서는 mixin기능을 이용하여 이런 번거로움을 줄일 수 있도록 해준다.

\* 작성방법
선언 – @mixin mixin명{}
호출 – @include mixin명{}

**scss 코드**

~~~scss
$overflow_hid:hidden;
@mixin ellipsis{
overflow:$overflow_hid;
white-space:nowrap;
text-overflow:ellipsis;
}
@mixin hide_txt{
display:inline-block;
overflow:$overflow_hid;
line-height:9999px;
vertical-align:top;
}
.title{
      @include ellipsis;
color:#1b1f2a;
}
.sub_tit{
      @include ellipsis;
}
.btn{
@include hide_txt;
width:12px;
height:8px;
}
.ico{
width:4px;
height:4px;
@include hide_txt;
}
~~~

**css 변환코드**

~~~css
.title {
overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
color: #1b1f2a;
}
.sub_tit {
overflow: hidden;
white-space: nowrap;
      text-overflow: ellipsis;
}
.btn {
      display: inline-block;
      overflow: hidden;
      line-height: 9999px;
      vertical-align: top;
width: 12px;
height: 8px;
}
.ico {
width: 4px;
height: 4px;
display: inline-block;
      overflow: hidden;
      line-height: 9999px;
      vertical-align: top;
}
~~~

또한 mixin은 특정 속성값을 인자로 지정하여 여러가지의 스타일을 만들 수도 있다. 또, 인자에 기본값을 지정할 수도 있다 . *기본값은 css처럼 (:)콜론으로 정의한다.

\* 작성방법
선언 – @mixin mixin명(변수명){}
호출 – @include mixin명(변수명){}

**scss 코드**

~~~scss
@mixin title_style($size, $color:#080){
overflow:hidden;
white-space:nowrap;
text-overflow:ellipsis;
font-size:$size;
color:$color;
}
h1{
@include title_style(16px)
}
h2{
@include title_style(14px, #333)
}
~~~

**css 변환코드**

~~~css
h1 {
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
font-size: 16px;
color: #080;
}
h2 {
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
font-size: 14px;
color: #333;
}
~~~



##### [확장 Extend]

\* 작성방법 – @extend .클래스명; or @extend %클래스명;

**%**  : Placeholder Selectors

extend를 사용할 때 ID나 class선택자처럼 %로 선택자를 만들 수 있다. 단, **%선택자는 css상에서는 출력되지 않는다.** 

**scss 코드**

~~~scss
.ico{
display:block;
}
%bg{
background:url(img/bg.gif) no-repeat;
}
.ico_new{
@extend .ico;
@extend %bg;
background:red;
}
.ico_qna{
@extend .ico;
      @extend %bg;
background:black;
}
~~~

**css 변환코드**

~~~css
.ico, .ico_new, .ico_qna {
display: block;
}
.ico_new, .ico_qna {
background: url(img/bg.gif) no-repeat;
}
.ico_new {
background: red;
}
.ico_qna {
background: black;
}
~~~

##### Mixin 과 extend의 차이점 

| Mixin                 | Extend                               |
| --------------------- | ------------------------------------ |
| 공통된 속성을 클래스마다 넣어줄 것인가 | (,)콤마 선택자로 클래스들을 하나로 묶어서 한번만 넣어줄 것인가 |













출처

https://velopert.com/1712

http://ithub.tistory.com/87

http://wit.nts-corp.com/2015/01/09/2936

https://taegon.kim/archives/3667

https://developers.google.com/web/tools/setup/setup-preprocessors?hl=ko