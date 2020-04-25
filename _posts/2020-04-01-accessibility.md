---
layout: post
title: web Accessibility
date: 2020-04-01 00:00:00 +0200
published: 2020-04-01 00:00:00 +0200
categories: development
tags: ['Accessibility']
comments: true,
github: "https://github.com/gmm117/gmm117.github.io"
---

<pre>
    웹접근성
    웹접근성 스펙
    웹접근성 검사
    웹접근성 도구
</pre>
<!--more-->


---

# 웹접근성이란?
 - 웹 접근성(Web Accessibility) 이란 장애인, 고령자 등이 웹 사이트에서 제공하는 정보에 비장애인과 동등하게 접근하고 이해할 수 있도록 보장하는 것입니다.
   그림이나 사진들을 제공할 때 눈으로 볼 수 없는 경우를 대비하여 그림이나 사진을 대신 할 수 있는 설명을 텍스트로 제공해야 하며, 
   동영상이나 오디오의 경우 청각장애인을 위한 음성정보를 문자로 제공해야 합니다. 
   또한, 마우스를 사용할 수 없는 사용자를 위하여 키보드만으로도 모든 콘텐츠에 접근하여 이용할 수 있도록 해야 하며, 움직임이 느린 사용자를 위해 시간조절기능을 제공해야 합니다.

- 소개 동영상 (유튜브)
    - <a href="https://youtu.be/20SHvU2PKsM" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://youtu.be/20SHvU2PKsM</a>
    - <a href="https://www.youtube.com/watch?v=OwxZbJwtYDM&list=PL_6yF2upGJYs6IzIl9UiCt12eIM17k2Xe" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">https://www.youtube.com/watch?v=OwxZbJwtYDM&list=PL_6yF2upGJYs6IzIl9UiCt12eIM17k2Xe(1~23 웹접근성 소개영상, N WAX 사용방법)</a>
- 참고사이트
    - <a href="https://www.w3.org/WAI/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">W3C 웹접근성 가이드</a>
    - <a href="http://www.wah.or.kr/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">웹접근성 연구소</a>
    - <a href="http://accessibility.naver.com/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">네이버 접근성 가이드</a>

# 웹접근성 스펙(한글웹접근성평가센터기준)
- 참고사이트
    - <a href="http://www.kwacc.or.kr/WebAccessibility/ComplianceWay" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">한글웹접근성평가센터기준</a>
    - <a href="http://nuli.navercorp.com/sharing/a11y/checklist" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">NHN Web Content 접근성 평가기준</a>

- 인식의 용의성(Perceivable) : 글로 표현할 수 없는 콘텐츠를 제외하고 장애 유형에 관계없이 모든 사용자가 콘텐츠를 인지할 수 있도록 제공해야한다.

## 적절한 대체 텍스트 제공(img, map, input)
- (Alt 미제공)텍스트 이미지에 대체 텍스트가 제공되지 않은 경우
- (Alt 미제공)이미지 버튼에 대체 텍스트를 제공하지 않는경우
- (Alt 미제공)블릿 이미지에 대한 대체 텍스트를 제공하지 않은경우(화면낭독기:그래픽링크 읽게됨)
- (Alt 미제공)대체텍스트를 Title 속성만 제공한 경우
- (Alt 미제공)이미지맵을 사용한 "<img>" 요소에 alt 속성으로 제공하지 않는경우
- 의미없는정보는 대체텍스트 미제공
- 충분한 정보가 필요한 경우

## 인식의 명료성
- 색에 무관한 콘텐츠 인식(색상이 아닌 패턴이나 명암으로 정보를 인식)
- 시각/청각에 대한 지시사항 제공 : 이미지파일

## 링크텍스트
- 링크 텍스트가 단독으로 사용될때도 정확한 의미 파악을 제공

## 포커스 인식
- 포커스를 시각적으로 구별가능여부
- 멀티미디어 대체수단(자막제공)

# 운용의 용이성(Operable) : 웹 콘텐츠에 포함된 모든 기능은 누구나 쉽게 사용할 수 있어야 한다.

## 입력장치 접근성
- 키보드로 접근 및 운용이 불가능한 경우(마우스로만 가능) : 이미지파일1/이미지파일2
- mouse(over,out,click) => onfocus, onblur등/img 태그=>button 태그
- 키보드 동작가능 태그
    - < a > : 링크로 이동할 수 있도록 연결
    - < input > : 서식에 정보를 입력, 
    - < select > : 몇 가지의 내용을 나열하여 선택
    - < textarea > :  글쓰기 등에 사용하는 편집창
    - < button > : 액션을 실행
- Tab 키와 Shift + Tab 키에 의한 초점의 이동 순서는 논리적이며 일관성이 있어야 하며, 동작을 해야한다. (좌측 상단 영역에서 우측 하단 영역으로 이동, 위에서 아래로)

## 쉬운 내비게이션
- 건너띄기 링크를 제공하지 않는경우
- 제목 제공
    - 페이지 제목의 타이블 속성을 제공
    - < frame >,< iframe >,< frameset > 요소의 title속성이 없거나 값을 비워둔 경우
- 목적이나 용도를 알기 어려운 링크 텍스트를 제공한 경우
- 광과민성 발작을 일으킬 수 있는 콘텐츠를 제공안됨(깜박임, 번쩍임)

## 이해의 용의성(Understandable) : 사용자들이 가능한 쉽게 이해할 수 있도록 콘텐츠나 제어 방식을 구성해야한다.
- 기본언어표시
    - 문서타입에 맞는 기본언어를 명시

- 사용자 요구에 따른 실행
    - 사용자가 실행하지 않는 상황에 액션실행되는 경우
    - 버튼 또는 링크 등을 실행할 때 사전에 알리지 않고 새 창이 발생하는 경우

- 콘텐츠의 논리성
    - 계층 구조가 명백하게 필요하나 콘텐츠를 중첩 마크업(ul,ol)을 이용하여 표현하지 않는경우

- 표의 구성
    - 표의 제목(caption)과 요약정보(summary)를 제공하지 않는경우
    - → HTML 5 이하(caption, summary 제공), HTML 5(caption만제공, summary 속성삭제)
- 데이터 테이블에 제목 셀과 내용 셀을 <th>와 <td> 요소로 구분하지 않은경우


# 웹접근성 검사
## 웹접근성 검사 사이트
- 한국웹접근성인증평가원 > 웹 접근성 자가진단 서비스
    - <a href=":https://www.beaccessible.net/dashboard/selfAnalyze" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">URL</a>

- 웹 접근성 연구소 > 개발자아카이브 > 웹 접근성 평가도구
    - <a href="http://www.wah.or.kr/Participation/online-wah.asp" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">URL</a>

## 도구 리스트
- Open-WAX
- WAVE
- AChecker
- Colour Contrast Analyser
- 네이버 웹접근성 평가도구 > N WAX
    - <a href="http://nuli.navercorp.com/sharing/a11y/tool#nwax" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">URL</a>
- W3C > Accessibility > Test & Evaluate > Tools
    - <a href="https://www.w3.org/WAI/ER/tools/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">URL</a>

## 웹접근성 도구 > 스크린리더
- 센스리더
    - <a href="http://www.xvtech.com" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">URL</a>
    - 국내 1위 스크린리더

- JAWS
    - <a href="https://www.freedomscientific.com/Products/Blindness/JAWS" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">URL</a>
    - 세계 1위 스크린리더

- NVDA
    - <a href="https://www.nvaccess.org/download/" target="_blank" style="font-size=30px; color: #4dabf7; text-decoration:underline;">URL</a>
    - OpenSource 기반 스크린리더

- VoiceOver
    - MacOS / iOS용 스크린리더 (기본탑재)

- TalkBack
    - Android용 스크린리더 (기본탑재)

## 스크린리더와 가상커서
- 가상커서
    - 웹페이지를 문서 편집기처럼 탐색할 수 있도록 재구성하여 상/하/좌/우 방향키 등으로 이동할 때 갖게 되는 포인터 위치

- 참고자료
    - 스크린 리더에서의 가상 커서와 객체 탐색 이해하기

## 스크린리더 기본동작
- 키보드의 키를 눌렀을 때
    - 입력된 키를 읽는다. (영문 a를 입력하면 a, 한글일 경우에는 조합된 글자를 소리냄)

- 웹브라우저 로딩시 (주소창을 입력하고 엔터키를 누르거나 F5를 눌러 리프레시할 때)
    - 브라우징된 내용을 순서대로 모두 읽는다. 읽는 도중 ESC를 누르면 읽기를 멈춤

- 탭키를 눌렀을 때
    - 포커스가 변경되며, 현재 포커싱된 Tag를 읽어준다.
    - 예로 "<a>한글과컴퓨터</a>" Tag일 경우 "링크 한글과컴퓨터"라고 읽는다.

- 화살표 방향키를 눌렀을 때
    - 가상커서가 활성화된다. 가상커서란 현재 읽고 있는 영역을 가리키는 커서이다. (포커스와는 다름)
    - 가상커서는 화면에 표시되지 않는다.
    - 좌우키를 누른 경우에는 한글자씩 가상커서가 이동되며 글자를 읽는다.
    - 상하키를 누른 경우에는 문단단위로 가상커서가 이동되며 문단 전체를 읽는다.

---