# About_Web
<h1>브라우저 <-> front(client) <-> server 와의 통신 흐름과 관계를 알아보도록 하겠습니다</h1>
<strong>axios, node-express, mysql-sequelize 를 이용하여 서버,DB 와 의 통신을 실행하겠습니다.</strong>
<p>아래의 목차에 대해 진행됩니다.</p>
<ul>
  <li>브라우저 - 렌더링</li>
  <li>front - 통신</li>
  <li>server - 통신</li>
</ul>

<h2>1. 브라우저 - 렌더링</h2>
<p>Client는 서버와 통신할수 있는 모든 소프트웨어 입니다. 대표적인 예로 브라우저가 있습니다.</p>
<p>웹에 대해 다루고 있기 때문에 브라우저의 목적, 특징을 알아보겠습니다.</p>
<p>브라우저는 렌더링된 결과물을 시각적으로 보여주는걸 목표로 가지고 있습니다. <br>
    렌더링 이란 서버로 부터 HTML 파일을 받아 브라우저에 뿌려주는 과정을 의미합니다. <br>
    보통 웹은 html이 뼈대, css가 꾸며주기, javascript가 동작을 나타내기 때문에 이 3가지를 받아옵니다. <br>
</p>
<p>렌더링을 하는 주체는 브라우저 입니다.(Chrome, IE, Whale, ...) 브라우저가 렌더링을 위해 서버에게 HTML파일(리소스)을 요청(request)하고
    서버는 해당 파일(리소스)을 보내줍니다.(응답-response).
</p>
<ul>
    <li>렌더링을 하는 주체는 브라우저 입니다.(Chrome, IE, Whale, ...) 브라우저가 렌더링을 위해 서버에게 HTML파일(리소스)을 요청(request)하고
        서버는 해당 파일(리소스)을 보내줍니다.(응답-response).<br><br></li>
  <li>브라우저 렌더링 엔진은 해당 <strong>HTML파일을 위에서 아래로, 동기적으로 파싱</strong>을 하며 DOM을 생성합니다. 그러다가 css-link, img-tag, js-script 같은걸 만나면
        HTML파싱을 중지하고 서버로 해당 파일을 request보내고 response 받습니다.<br>
        CSS도 동일한 파싱 과정을 거쳐 CSSOM을 생성하고, 그 후 다시 HTML파싱과 DOM생성을 이어서 진행합니다.
        이렇게 html,css의 파싱 결과물인 DOM,CSSOM을 결합하여 Render Tree를 생성합니다.<br><br>
    </li>
    <li>html을 계속 파싱하는 중에 css처럼 script-js를 만나면 다시 html파싱을 멈추고 js파일을 서버로부터 받아 js를 파싱합니다.
        js파일은 브라우저 엔진이 아니라 자바스크립트 엔진이 진행을 합니다. 위와 비슷하게 파싱을 하여 AST를 생성하며 이는 인터프리터가 실행할 수
         있는 중간 코드인 바이트코드로 변환되고 인터프리터에 의해 실행됩니다.<br><br>
    </li>
    <li>만약 js파일에 DOM이나 CSSOM을 변경하는 DOM API가 사용 되었다면 <strong>DOM, CSSOM이 변경되고 변경된걸로 다시 Render Tree로 결합이 됩니다</strong>.
        렌더트리로 레이아웃을 만들고 브라우저 화면에 픽셀을 렌더링하는 페인팅 과정을 거칩니다.<br>
      즉 js파일에서 DOM, CSSOM을 변경하였다면 렌더트리가 바뀌기 때문에 <strong>리플로우</strong>(레이아웃 계산을 다시 하는것)와 <strong>리페인팅</strong>(재결합된 Render Tree로 다시 페인팅 하는것)을 실행합니다.
    </li>
</ul>

<h2>2. Client</h2><br>
<p>Client는 서버와 통신할수 있는 모든 소프트웨어 입니다. 대표적인 예로 브라우저가 있습니다.</p>
<p>비슷한 단어로 front,즉 프런트엔드, 프런트엔드 개발자가 있습니다. front-end는 client와 차이점으로 주체가 다릅니다.<br>
  front는 <strong>사용자</strong>가 주체입니다. 예를들어 브라우저는 client입니다. 브라우저가 서버에게 리소스를 요청하죠(html, css, js, img 등등).<br>
  front는 사용자의 인터페이스, 경험 같은것들 입니다. 즉 <strong>UX/UI</strong>입니다. 즉 브라우저가 보여준게 front입니다.<br><br>
    브라우저는 우리에게 렌더링 결과물을 보여주죠. 그리고 이것들을 우리가 시각적으로 볼수있고, 마우스로 클릭할수 있고, 터치할수 있습니다.
    즉 우리가 동작시킬수 있고 경험할수 있는겁니다. 그래서 front-end developer는 우리, 즉 사용자들의 경험을 만족시키기 위하여 리소스를 개발합니다. <br>
    사용자들이 접근할수 있는것은 눈으로 확인할수 있는 layout이기 때문에 즉, html, css, js 같은걸 개발하게 되죠.
</p>
<h2>3. Server</h2><br>
<p>쉽게 말하면 서버는 인터넷에 연결된 컴퓨터입니다. 인터넷에 수많은 서버를 모아놓고 브라우저가 인터넷을 이용하여 서버에 있는 리소스를 화면에 보여주는겁니다. <br>
    모든 것들이 서버에 저장되어 있습니다. 물론 DB를 포함한 이야기입니다. 앞서 얘기했듯 front-end가 앞단 UX/UI를 다루듯 back-end는 뒷단 
    제공할 리소스, 환경을 다룹니다. 즉 서버,DB 등등을 다룹니다.<br>
  결론은 <strong>client는 요청자</strong>, <strong>server는 제공자</strong> 라고 할수 있습니다.
    <br>
    그럼 서버는 한개일까요? 오직 back-end만 서버를 가질까요? 아닙니다. front도 서버를 가집니다.
</p><br>
<p>
    front-end, back-end 처럼 직군이 분리된것 처럼 개발환경도 분리됩니다. back-end는 본래 서버를 다루듯 서버가 있고, 
    front-end는 브라우저에 출력해줄 리소스를 제공할 서버가 있는겁니다.<br><br> 즉 앞단에서는 html,css,js를 보여줄 서버가 있으며 UI를 통해 서버와 
    통신을 합니다. 그러면 back-end가 작업을 합니다. 이렇듯 front,back 둘다 서버를 가지고 있습니다.
  
![K-004](https://user-images.githubusercontent.com/36911316/116374718-8311e880-a849-11eb-89b0-70bbf237c9fa.png)


front서버에서 html,css,js를 제공하고 <br>

![K-005](https://user-images.githubusercontent.com/36911316/116374727-873e0600-a849-11eb-829a-7b9aa1847d6b.png)


back서버에서 비동기로 요청한 리소스를 제공해줍니다 <br>

![K-003](https://user-images.githubusercontent.com/36911316/116374321-244c6f00-a849-11eb-87b0-d8b1f6e57b4d.png)
![K-006](https://user-images.githubusercontent.com/36911316/116374755-8f964100-a849-11eb-8a8a-014dafa7b229.png) <br>
빨강이 front서버에서 받은 원래 html내용 <br>
파랑이 back서버에서 받은 리소스로 DOM을 조작하여 리페인트한 내용
    
</p>
