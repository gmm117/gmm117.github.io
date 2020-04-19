---
layout: post
title: Front-End Performance
date: 2020-04-19 00:00:00 +0200
published: 2020-04-19 00:00:00 +0200
categories: development
tags: ['front-end performance', 'blog']
comments: true
github: "https://github.com/gmm117/gmm117.github.io"
---

<pre>
    DOM(Document Object Model), CSSOM(CSS Object Model) 생성
        Render Tree 생성
        렌더링 과정
        Layout, Paint
    프론트엔드 성능 최적화
        렌더링 최적화 : Reflow, Repaint 줄이기
        Reflow (Layout), Repaint (Paint)
</pre>
<!--more-->

---

<h1 style="font-weight:bold">렌더링 과정</h1>

<h2 style="color:#ff6b6b">DOM(Document Object Model), CSSOM(CSS Object Model) 생성</h2>
가장 첫번째 단계는 서버로부터 받은 HTML, CSS를 다운로드 받습니다. 그리고 HTML, CSS파일은 단순한 텍스트이므로 연산과 관리가 유리하도록 Object Model로 만들게 됩니다. HTML CSS 파일은 각각 DOM Tree와 CSSOM으로 만들어집니다.

![렌더링 과정](/assets/images/{{page.id}}/rendering_dom.png)
- <a href="http://bit.ly/3137pmh" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">DOM(좌) CSSOM(우)을 시각화 한 그림(출처 : http://bit.ly/3137pmh)</a>

각 문서(HTML, CSS)가 어떻게 파싱되고 어떻게 DOM Tree가 되는지 자세한 과정은 Google 개발자 문서를 통해 확인할 수 있습니다.

여기서 좀더 TMI를 추가하자면 렌더링 엔진은 더 나은 사용자경험을 위해 가능한 빠르게 내용을 표시하게 만들어졌습니다. 따라서 모든 HTML 파싱이 끝나기도 전에 이후의 과정을 수행하여 미리 사용자에게 보여줄 수 있는 일부 내용들을 출력하게 됩니다. 

<h2 style="color:#ff6b6b">Render Tree 생성</h2>
DOM Tree와 CSSOM Tree가 만들어졌으면 그 다음으로는 이 둘을 이용하여 Render Tree를 생성합니다. 순수한 요소들의 구조와 텍스트만 존재하는 DOM Tree와는 달리 Render Tree에는 스타일 정보가 설정되어 있으며 실제 화면에 표현되는 노드들로만 구성됩니다.

![Render Tree 구조도](/assets/images/{{page.id}}/rendering_rendertree.png)
- <a href="http://bit.ly/2Okn0fG" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">Render Tree 구조도(출처 : http://bit.ly/2Okn0fG)</a>

그러면 여기서 각 요소에 스타일 정보들이 설정되어 있는건 이해할 수 있겠는데 실제 화면에 표현되는 노드들로만 구성된다는 이야기에 "모든 요소는 다 화면에 표현되는거 아닌가?" 라는 의문을 가지실 것 같습니다.

결론을 말하면 네, 아닙니다. 간단한 예로 display: none 속성이 설정된 노드는 화면에 어떠한 공간도 차지하지 않기 때문에 Render Tree를 만드는 과정에서 제외됩니다. 여기서 조금만 더 팁을 드리자면 visibility: invisible 은 display: none과 비슷하게 동작하지만, 공간은 차지하고 요소가 보이지 않게만 하기 때문에 Render Tree에 포함됩니다.

<h2 style="color:#ff6b6b">Layout</h2>
Layout 단계는 브라우저의 뷰포트(Viewport) 내에서 각 노드들의 정확한 위치와 크기를 계산합니다. 풀어서 얘기하자면 생성된 Render Tree 노드들이 가지고 있는 스타일과 속성에 따라서 브라우저 화면의 어느위치에 어느크기로 출력될지 계산하는 단계라고 할 수 있습니다. Layout 단계를 통해 %, vh, vw와 같이 상대적인 위치, 크기 속성은 실제 화면에 그려지는 pixel단위로 변환됩니다.

![Render Tree 구조도](/assets/images/{{page.id}}/rendering_layout.png)
- <a href="http://bit.ly/3137pmh" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">Viewport 에 상대적인 요소 연산(출처 : http://bit.ly/3137pmh)</a>

여기서 뷰포트(Viewport)란 그래픽이 표시되는 브라우저의 영역, 크기를 말합니다. 뷰포트는 모바일의 경우 디스플레이의 크기, PC의 경우 브라우저 창의 크기에 따라 달라집니다. 그리고 화면에 그려지는 각 요소들의 크기와 위치는 %, vh, vw와 같이 상대적으로 계산하여 그려지는 경우가 많기 때문에 viewport 크기가 달라질 경우 매번 계산을 다시해야 합니다.

<h2 style="color:#ff6b6b">Paint</h2>
Layout 계산이 완료되면 이제 요소들을 실제 화면을 그리게 됩니다. 이전 단계에서 이미 요소들의 위치와 크기, 스타일 계산이 완료된 Render Tree 를 이용해 실제 픽셀 값을 채워넣게 됩니다. 이 때 텍스트, 색, 이미지, 그림자 효과등이 모두 처리되어 그려집니다.

이 때 처리해야 하는 스타일이 복잡할수록 Paint 단계에 소요되는 시간이 늘어나게 됩니다. 간단한 예시로 단순한 단색 background-color의 경우 paint 속도가 빠르지만 그라데이션이나 그림자 효과등은 painting 소요시간이 비교적 더 오래 소요됩니다.


<h1 style="font-weight:bold">프론트엔드 성능 최적화</h1>

<h2 style="color:#ff6b6b">렌더링 최적화 - Reflow, Repaint 줄이기</h2>
지금까지 웹 페이지가 렌더링되는 과정을 알아보았습니다. 그렇다면 웹 성능 최적화를 어떻게 할 수 있을까요? 이를 알려면 Reflow와 Repaint에 대해 먼저 짚고 넘어가야 합니다.

<h2 style="color:#ff6b6b">Reflow (Layout)</h2>
위에서 언급된 렌더링 과정을 거친 뒤에 최종적으로 페이지가 그려진다고 해서 렌더링 과정이 다 끝난것이 아닙니다. 어떠한 액션이나 이벤트에 따라 html 요소의 크기나 위치등 레이아웃 수치를 수정하면 그에 영향을 받는 자식 노드나 부모 노드들을 포함하여 Layout 과정을 다시 수행하게 됩니다. 이렇게 되면 Render Tree와 각 요소들의 크기와 위치를 다시 계산하게 됩니다. 이러한 과정을 Reflow라고 합니다.

```javascript
// reflow 발생 예제
function reflow() { 
	document.getElementById('content').style.width = '600px';
}
```
<h3 style="color:#ff6b6b">Reflow가 일어나는 대표적인 경우는 아래와 같습니다.</h3>
- 페이지 초기 렌더링 시(최초 Layout 과정)
- 윈도우 리사이징 시 (Viewport 크기 변경시)
- 노드 추가 또는 제거
- 요소의 위치, 크기 변경 (left, top, margin, padding, border, width, height, 등..)
- 폰트 변경 과(텍스트 내용) 이미지 크기 변경(크기가 다른 이미지로 변경 시)

<h2 style="color:#ff6b6b">Repaint (Paint)</h2>

Reflow만 수행되면 실제 화면에 반영되지 않습니다. 위에서 언급된 렌더링 과정과 같이 Render Tree를 다시 화면에 그려주는 과정이 필요합니다. 결국은 Paint 단계가 다시 수행되는 것이며 이를 Repaint 라고 합니다.

하지만 무조건 Reflow가 일어나야 Repaint가 일어나는것은 아닙니다. background-color, visibility와 같이 레이아웃에는 영향을 주지 않는 스타일 속성이 변경되었을 때는 Reflow를 수행할 필요가 없기 때문에 Repaint만 수행하게 됩니다.

<h2 style="color:#ff6b6b">Reflow, Repaint 줄이기</h2>

이번 포스팅에서 다룰 성능 최적화는 단순히 Reflow, Repaint 연산을 줄이는 방법에 대해 소개하고자 합니다. 아래 내용은 현재까지 조사된 부분만 소개하였습니다. 또한 실제 테스트해본 것이 아닌 이론적인 내용 및 발췌해 온 내용이므로 검증이 필요합니다. 하위 내용은 지속적으로 업데이트 하겠습니다.

<h2 style="color:#ff6b6b">사용하지 않는 노드에는 visibilty: invisible 보다 display: none을 사용하기</h2>
visibility invisible은 레이아웃 공간을 차지하기 때문에 reflow의 대상이 됩니다. 하지만  display none은 Layout 공간을 차지하지 않아 Render Tree에서 아예 제외됩니다.

<h2 style="color:#ff6b6b">Reflow, Repaint 가 발생하는 속성 사용 피하기</h2>
아래는 각각 Reflow, Repaint가 일어나는 CSS 속성들 입니다. Reflow가 일어나면 Repaint는 필연적으로 일어나야 하기 때문에 가능하다면 Reflow가 발생하는 속성보다 Repaint 만 발생하는 속성을 사용하는것이 좋습니다.

<h3 style="color:#ff6b6b">Reflow가 일어나는 대표적인 속성</h3>
<table>
    <tbody>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">position</td>
            <td style="border: 1px solid #444444; padding: 2px;">width</td>
            <td style="border: 1px solid #444444; padding: 2px;">height</td>
            <td style="border: 1px solid #444444; padding: 2px;">left</td>
            <td style="border: 1px solid #444444; padding: 2px;">top</td>
        </tr>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">right</td>
            <td style="border: 1px solid #444444; padding: 2px;">bottom</td>
            <td style="border: 1px solid #444444; padding: 2px;">margin</td>
            <td style="border: 1px solid #444444; padding: 2px;">padding</td>
            <td style="border: 1px solid #444444; padding: 2px;">border</td>
        </tr>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">border-width</td>
            <td style="border: 1px solid #444444; padding: 2px;">clear</td>
            <td style="border: 1px solid #444444; padding: 2px;">display</td>
            <td style="border: 1px solid #444444; padding: 2px;">float</td>
            <td style="border: 1px solid #444444; padding: 2px;">font-family</td>
        </tr>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">font-size</td>
            <td style="border: 1px solid #444444; padding: 2px;">font-weight</td>
            <td style="border: 1px solid #444444; padding: 2px;">line-height</td>
            <td style="border: 1px solid #444444; padding: 2px;">min-height</td>
            <td style="border: 1px solid #444444; padding: 2px;">overflow</td>
        </tr>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">text-align</td>
            <td style="border: 1px solid #444444; padding: 2px;">vertical-align</td>
            <td style="border: 1px solid #444444; padding: 2px;">white-space</td>
            <td style="border: 1px solid #444444; padding: 2px;">...</td>
        </tr>
    </tbody>
</table> 

<h3 style="color:#ff6b6b">Repaint가 일어나는 대표적인 속성</h3>
<table>
    <tbody>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">background</td>
            <td style="border: 1px solid #444444; padding: 2px;">background-image</td>
            <td style="border: 1px solid #444444; padding: 2px;">background-position</td>
            <td style="border: 1px solid #444444; padding: 2px;">background-repeat</td>
            <td style="border: 1px solid #444444; padding: 2px;">background-size</td>
        </tr>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">border-radius</td>
            <td style="border: 1px solid #444444; padding: 2px;">border-style</td>
            <td style="border: 1px solid #444444; padding: 2px;">box-shadow</td>
            <td style="border: 1px solid #444444; padding: 2px;">color</td>
            <td style="border: 1px solid #444444; padding: 2px;">line-style</td>
        </tr>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">outline</td>
            <td style="border: 1px solid #444444; padding: 2px;">outline-color</td>
            <td style="border: 1px solid #444444; padding: 2px;">outline-style</td>
            <td style="border: 1px solid #444444; padding: 2px;">outline-width</td>
            <td style="border: 1px solid #444444; padding: 2px;">text-decoration</td>
        </tr>
        <tr>
            <td style="border: 1px solid #444444; padding: 2px;">visibility</td>
            <td style="border: 1px solid #444444; padding: 2px;">....</td>
        </tr>
    </tbody>
</table>

- <a href="https://csstriggers.com/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">CSS Trigger</a>
- <a href="https://docs.google.com/spreadsheets/u/0/d/1Hvi0nu2wG3oQ51XRHtMv-A_ZlidnwUYwgQsPQUg1R2s/pub?single=true&gid=0&output=html"  target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">Reflow & Repaint</a>

또한 Reflow Repaint가 일어나지 않는 transform, opacitiy와 같은 속성도 있습니다. 따라서 left, right, width, height 보다 transform을, visibility/display 보다 opacitiy를 사용하는 것이 성능 개선에 도움이 됩니다.

<h2 style="color:#ff6b6b">영향을 주는 노드 줄이기</h2>
Javascript + Css를 조합하여 애니메이션이 많거나 레이아웃 변화가 많은 요소의 경우 position을 absolute 또는 fixed를 사용하여 영향을 받는 주변 노드들을 줄일 수 있습니다. fixed와 같이 영향을 받는 노드가 전혀 없는 경우 reflow과정이 전혀 필요가 없어지기 때문에 Repaint 연산비용만 들게 됩니다.

또다른 방법은 애니메이션 시작시 요소를 absolute, fixed로 변경 후 애니메이션이 종료되었을 때 원상복구 하는 방법도 Reflow, Repaint 연산을 줄이는대에 도움이 됩니다.

<h2 style="color:#ff6b6b">프레임 줄이기</h2>
단순히 생각하면 0.1초에 1px씩 이동하는 요소보다 3px씩 이동하는 요소가 Reflow, Repaint 연산비용이 3배가 줄어든다고 볼 수 있습니다. 따라서 부드러운 효과를 조금 줄여 성능을 개선할 수 있습니다.


<h1 style="font-weight:bold">프론트엔드 성능을 향상시키는 코딩방법</h1>

* 배열 대신 객체/맵을 사용
    - <a href="https://jsperf.com/finding-element-object-vs-map-vs-array/1" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">참고사이트</a>

![배열 대신 객체/맵을 사용](/assets/images/{{page.id}}/performance1.png)

* 예외를 먼저 처리하는 대신, IF문을 사용
    - <a href="https://jsperf.com/try-catch-vs-conditions/1" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">참고사이트</a>

![예외를 먼저 처리하는 대신, IF문을 사용](/assets/images/{{page.id}}/performance2.png)

* 가능한 한 반복문을 적게 사용
    - <a href="https://jsperf.com/array-function-chains-vs-single-loop-filter-map/1" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">참고사이트</a>

![가능한 한 반복문을 적게 사용](/assets/images/{{page.id}}/performance3.png)

* 기본 반복문을 사용
    - <a href="https://jsperf.com/for-loops-in-few-different-ways/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">참고사이트</a>

![기본 반복문을 사용](/assets/images/{{page.id}}/performance4.png)

* 내장 DOM 메소드를 사용
    - <a href="https://jsperf.com/native-dom-functions-vs-jquery/1" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">참고사이트</a>

![내장 DOM 메소드를 사용](/assets/images/{{page.id}}/performance5.png)

<h1 style="font-weight:bold">참고사이트</h1>

* <a href="https://gloriajun.github.io/frontend/2018/10/23/frontend-reflow-repaint.html" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://gloriajun.github.io/frontend/2018/10/23/frontend-reflow-repaint.html</a>
* <a href="https://junwoo45.github.io/2019-10-05-frontend-performance/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://junwoo45.github.io/2019-10-05-frontend-performance/</a>
* <a href="https://www.slideshare.net/NHNFORWARD/2018-130108045" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://www.slideshare.net/NHNFORWARD/2018-130108045</a>
* <a href="https://ideveloper2.dev/blog/2019-05-18--front-end-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94-%EA%B8%B0%EB%B3%B8/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://ideveloper2.dev/blog/2019-05-18--front-end-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94-%EA%B8%B0%EB%B3%B8/</a>
* <a href="https://ui.toast.com/fe-guide/ko_PERFORMANCE/#%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EA%B3%BC-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://ui.toast.com/fe-guide/ko_PERFORMANCE/#%EB%A0%88%EC%9D%B4%EC%95%84%EC%9B%83%EA%B3%BC-%EB%A6%AC%ED%8E%98%EC%9D%B8%ED%8A%B8</a>
* <a href="https://boxfoxs.tistory.com/408" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://boxfoxs.tistory.com/408</a>
* <a href="http://bit.ly/2SQXLzY" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">http://bit.ly/2SQXLzY</a>
* <a href="https://github.com/wonism/TIL/blob/master/front-end/browser/reflow-repaint.md" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://github.com/wonism/TIL/blob/master/front-end/browser/reflow-repaint.md</a>
* <a href="https://gist.github.com/faressoft/36cdd64faae21ed22948b458e6bf04d5" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://gist.github.com/faressoft/36cdd64faae21ed22948b458e6bf04d5</a>

<h1 style="font-weight:bold">Canvas 성능향상</h1>

<h2 style="color:#ff6b6b">오프스크린 캔버스로 미리 랜더링 해라</h2>
이건 뭐 이미지프로세싱 프로그래밍 해보신 분은 다 아시는 부분일텐데, canvas를 바로바로 렌더링 하지 말고 일단 이미지 버퍼나 더블버퍼링같은 원리처럼 렌더링은 RequestAnimationFrame를 사용해서 렌더링 하라는 뜻입니다.

<h2 style="color:#ff6b6b">일괄적인 드로잉 작업은 한번에 호출해라</h2>
예를 들어 캔버스에 사각형 같은 라인을 그릴때, 라인마다 beginPath()와 stroke()를 호출하지 말고, beginPath() 호출 후 moveTo와 lineTo를 이용하여 모든 드로잉 작업을 끝낸 후 stroke()를 호출하라는 뜻입니다. 이건 샘플코드를 보시면 단박에 이해가 가실겁니다.

<h2 style="color:#ff6b6b">불필요한 캔버스 상태 변경을 피해라</h2>
불필요한 연산을 피하라는... 위와 비슷한 얘기입니다.

<h2 style="color:#ff6b6b">변경된 부분의 캔버스 상태만 렌더링해라</h2>
이미지가 변경된 부분의 Bounding box를 구해서 그 부분만 렌더링 하라는 뜻입니다.

<h2 style="color:#ff6b6b">복잡한 장면엔 캔버스를 레이어로 구성해라</h2>
이건 맨처음 얘기한 오프스크린 캔버스 얘기와도 비슷한 얘기이기도 하고, 추가로 canvas를 겹쳐서 구성해서 렌더링시켜도 GPU에선 알파 합성을 통해 한번에 렌더링되므로 이득이라고 하네요.

<h2 style="color:#ff6b6b">쉐도우 블러(Blur) 이펙트를 피해라</h2>
당연한 얘기지만 blur나 shadow 효과를 끄라는 뜻입니다. 그나저나 canvas에서 이걸 기본 지원하는건 몰랐넹..

<h2 style="color:#ff6b6b">캔버스를 클리어하는 다양한 방법을 알아둬라</h2>
HTML5의 캔버스는 Immediate mode라고 해서 이미지 버퍼링 없이 바로바로 디스플레이에 출력됩니다. 애니메이션같은걸 만들땐 다음 프레임을 그리기 위해 이전 프레임을 지워줘야 하는데... 이때 캔버스 전체를 지우지 말고 위에서 말한것 처럼 이미지가 변경된 부분의 bounding box를 추적해서 clearRect 같은걸 해주라는 뜻입니다.

<h2 style="color:#ff6b6b">부동 소수점 좌표는 피해라</h2>
이미지를 배치시킬때 좌표가 부동소수점이면 자동으로 anti-aliasing이 먹어버립니다. 위에서 blur효과는 피하라고 했으니 좌표는 항상 정수로 찍히는게 좋습니다.

<h2 style="color:#ff6b6b">RequestAnimationFrame을 사용하여 최적화해라</h2>
윈도우 프로그래밍 해보신분이라면 눈치채셨을텐데 UIThread라고 보시면 됩니다. 다만 모든 브라우저에서 지원하는게 아니라서 안타깝네요..


<h2 style="color:#ff6b6b">DOM(Document Object Model), CSSOM(CSS Object Model) 생성</h2>
가장 첫번째 단계는 서버로부터 받은 HTML, CSS를 다운로드 받습니다. 그리고 HTML, CSS파일은 단순한 텍스트이므로 연산과 관리가 유리하도록 Object Model로 만들게 됩니다. HTML CSS 파일은 각각 DOM Tree와 CSSOM으로 만들어집니다.

* <a href="https://kuimoani.tistory.com/entry/HTML5-Canvas-%EC%84%B1%EB%8A%A5-%ED%96%A5%EC%83%81" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://kuimoani.tistory.com/entry/HTML5-Canvas-%EC%84%B1%EB%8A%A5-%ED%96%A5%EC%83%81</a>


