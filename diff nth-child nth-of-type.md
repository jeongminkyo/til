##nth-child 와 nth-of-type 차이점 



|                nth-child(n)                |               nth-of-type(n)               |
| :----------------------------------------: | :----------------------------------------: |
| 부모 엘리먼트의 모든 자식 엘리먼트중 n번째 | 부모 엘리먼트의 특정 자식 엘리먼트중 n번째 |

예시를 보면 이해가 빠르다.

~~~Html
<div class="box">
	<p>1. p태그 1</p>
    <span>2. span태그 1</span>
	<p>1. p태그 2</p>
    <span>2. span태그 2</span>
    <p>1. p태그 3</p>
~~~

결과물

![img](http://cfile5.uf.tistory.com/image/999AEC335986B515302EF1)

nth-child를 이용하면,

~~~css
.box > p:nth-child(5){
    color: red;
}
~~~

 ![img](http://cfile5.uf.tistory.com/image/99B2B5335986B8C90E3661)



nth-child는 모든 요소를 포함해서 순서를 확인한다.

nth-of-type을 이용하면,

~~~css
.box > p:nth-of-type(3){
    color: blue;
}
~~~

![img](http://cfile10.uf.tistory.com/image/994A07335986B9AD2D9329)

nth-of-type은 특정 요소만을 순서를 확인한다.



다른 예시를 들어보면,

~~~Html
<div class="dot6">
    <div>
        <h3>1번 h3</h3>
        <h3>2번 h3</h3>
        <h3>3번 h3</h3>
    </div>
    
    <h3>4번 h3</h3>
    <h3>5번 h3</h3>
    <h3>6번 h3</h3>
    
    <div>
        <h3>7번 h3</h3>
        <h4>1번 h4</h4>
        <h3>8번 h3</h3>
    </div>
    
    <h3>9번 h3</h3>
    <h3>10번 h3</h3>
    <h3>11번 h3</h3>
</div>
~~~



~~~css
h3:nth-child(2){
    background : pink;
}
~~~

![이미지-007.jpg](http://blog.hivelab.co.kr/wp-content/uploads/2015/05/a5d8c61075c95d753216334a4ebd17b0.jpg)

- div 안에 2번째 요소가 h3여서 2번 h3는 pink

- dot6 div의 2번째 요소가 h3여서 4번 h3는 pink

- Div 안에 2번째 요소가 h3가 아니라서 1번 h4는 적용 x

- 9,10,11번은 dot6 div에서 2번째가 아니라서 적용 x

  

~~~css
h3:nth-of-type(2){
    background : pink;
}
~~~

![이미지-008.jpg](http://blog.hivelab.co.kr/wp-content/uploads/2015/05/df1947c265be60b7e527743fcd39198e.jpg)

- div 안에 h3 2번째 요소인 2번 h3 pink
- 전체에서 2번째 h3인 5번 h3는 pink
- div 안에 2번째 h3요소인 8번 h3는 pink



#####[참고]

http://firerope.tistory.com/5

http://blog.hivelab.co.kr/enth-childn%EA%B3%BC-enth-of-typen%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90/