SVG 시작하기#1
동기
그동안 SVG에 대해 막연하게 알고 있었다.
“백터 기반이라서 사이즈가 변경되어도 안 깨지고 XHTML 문법으로 이루어져서 CSS 및 Event Handle이 가능하다”
딱 이 정도만 알고 있었고 실무에서 사용한 경험이나 그 이상 공부해본 경험이 없다.
물론 Canvas는 스터디해봤지만 SVG는 딱 위에서 언급한 정도였는데 최근에 Data Visualization에 대해 관심이 생겨서 보니 D3.js 같은 Data Visualization Library가 SVG를 이용하고 있었다.
물론 D3나 D3를 Wrapping 한 C3를 이용하면 쉽게 데이터를 Visualization 할 수 있지만 SVG를 이해해서 내가 간단한 라이브러리를 만들어보면 어떨까 라는 생각이 들게 되어 스터디를 시작했다.
SVG란?
SVG는 정확히 무엇이고 용도가 어떤 것이며 어떠한 문제 영역을 해결하려고 나온 것일까?
SVG는 “Scalable Vector Graphic”의 약자이며 2차원의 백터 그래픽을 표현하기 위해서 나왔고 XML 기반이다.
1998년 W3C에서 개발하기 시작했으며 그 이전에서 Microsoft의 “VML”와 Apple의 “PGML”이라는 2차원의 백터 그래픽을 XML로 표현하는 언어가 있었지만 2개의 스펙 모두 W3C에서 채택하지 않았고 W3C는 SVG를 개발하여서 표준으로 채택한 것 같다.
Microsoft의 “VML”은 Root요소로 <xml> 태그를 지정해야 했는데 이는 XML 표준 위반사항이라서 채택되지 않았지만 아직 Office 제품에서 일부 사용 가능한 것으로 확인되며
Apple과 Sun Microsystems 등 여러 기업에서 추진한 “PGML”이 채택되지 않은 이유는 찾기가 어려웠다.
현재 SVG의 가장 최신 버전은 1.2 버전이지만 1.2는 “SVG2”라는 스펙으로 넘어갈 예정이며 현재는 1.1 버전을 쓰는 것이 가장 안정적이다.
정확하게 스펙을 알아보고 싶다면 https://github.com/w3c/svgwg를 참고하길 바란다.
SVG의 장단점
SVG의 장점은 백터 기반이기 때문에 기존의 비트맵 기반의 포맷과 달리 사이즈가 늘어나도 사진이 깨지지 않고 매끄럽게 표현된다.
Web에서 Device의 Ratio를 대응하기 위해 기존 사이즈보다 x2 된 사진으로 대응하는데 SVG를 사용하면 굳이 원본과 x2를 모두 준비할 필요 없이 자동으로 대응된다.
그리고 Web환경에서 사용할 때는 CSS를 이용하여 Styling이 가능하고 Javascript를 이용해서 Event Handling도 가능하다. 이를 이용하여 기존에 HTML / CSS로는 조금 어려웠던 Animation이나 효과들을 잘 구현할 수 있지 않을까 생각된다.
단점으로는 크기를 예로 들 수 있는데, 간단한 경우에는 JPG나 PNG보다 더 적은 용량일 수 있지만 Path가 많아지고 점점 더 무거워지면 기존 이미지 포맷들보다 더 커질 수 있다.
이러한 단점을 보완하기 위해 SVG Optimizing을 위한 툴도 있다. https://github.com/svg/svgo
SVG 시작해보기
<svg width="300" height="300" xmlns="http://w3.org/2000/svg" version="1.1" viewbox="0 0 300 300">
</svg>
HTML에서 SVG를 시작하려면 <svg> 태그부터 알아야 한다.
<svg> 태그는 svg의 Root태그이며 svg는 XHTML 스펙을 따르고 있기 때문에 반드시 "xmlns" Attribute를 이용하여 NameSpace을 지정해줘야 한다. version은 사용할 SVG 스펙의 버전을 말하는데 일반적으로는 1.1을 사용하면 된다.
width와 height는 svg Element의 크기를 지정해주며 Requirement Attribute이므로 무조건 지정해주어야 한다.
그리고 중요한 viewbox라는 Attribute가 있는데, viewbox는 실제 svg영역 중에 보여줄 기준점을 정하는 Attribute이다. viewbox="x y width height"로 이루어져 있는데 width나 height는 0 값 이하를 지정하면 SVG가 렌더링 되지 않는다.
해당 viewbox의 위치나 크기에 따라서 svg Element는 300x300이지만 100x100만 표시될 수도 있고 좌표값 0,0이 아닌 100,100을 기준으로 보여줄 수 있다.
<svg width="300" height="300" xmlns="http://w3.org/2000/svg" version="1.1" viewbox="0 0 300 300">
<rect x="0" y="0" width="100" height="100" />
</svg>
위 코드는 300x300의 SVG 위에 100x100의 사각형을 그리는 코드입니다.
rect의 좌표는 x: 0, y: 0 이기 때문에 SVG의 가장 왼쪽 위에 그려집니다.
만약 viewbox를 변경하면 어떻게 나올까요?
<svg width="300" height="300" xmlns="http://w3.org/2000/svg" version="1.1" viewbox="0 0 300 300">
<rect x="0" y="0" width="100" height="100" />
</svg>
위와 같이 viewbox의 기본 좌표를 50, 50으로 변경하면 0, 0을 기준으로 그려진 rect는 원래 보이던 크기의 반만큼만 나옵니다.
사실 viewbox가 변한다고 실제 rect의 크기가 작게 변한 것은 아니고 viewbox의 위치만 옮겨서 rect가 모두 보이지 않는 것입니다.
SVG에서 사용하는 Tag
rect
rect 태그는 SVG에서 사각형을 그릴 수 있는 태그이다.
<rect x="0" y="0" rx="10" ry="10" width="100" height="100" fill="red" stroke="blue" stroke-width="10"/>
Attribute
- x는 좌표계를 기준으로의 x 값을 말한다.
- y는 좌표계를 기준으로의 y 값을 말한다.
- rx, ry는 사각형 모서리의 radius 값을 말한다.
- width, height는 사각형의 크기를 말한다.
- fill은 사각형의 Background 색을 말한다.
- stroke는 사각형의 border색을 말한다.
- stroke-width는 사각형 border의 넓이를 말한다.
circle
<circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red"></circle>
Attribute
- cx는 원의 중심점중 x값을 말한다.
- cy는 원의 중심점중 y값을 말한다.
- cx와 cy의 교차점이 해당 circle의 중심점이 된다.
- r은 원의 반지름의 길이를 말한다. cx와 cy의 교차점부터 r의 값으로 circle의 rendering 된다.
Ellipse
<ellipse cx="50" cy="50" rx="40" ry="20" stroke="black" stroke-width="3" fill="red"></ellipse>
Attritube
- cx는 원의 중심점중 x값을 말한다.
- cy는 원의 중심점중 y값을 말한다.
- rx는 원의 가로축 반지름을 말한다.
- ry는 원의 세로축 반지름을 말한다.
Line
<line x1="0" y1="10" x2="30" y2="10" stroke="orange" stroke-width="3"></line>
Attribute
- x1, y1은 line의 시작이 되는 점입니다.
- x2, y2는 line의 시작 점부터 x2, y2점까지 그어지는 점입니다.
Polygon
<polygon points="x1,y1 x2,y2, x3,y3 ..." fill="black" stroke="" stroke-width=""></polygon>
Attribute
- points는 Polygon을 형성하는 꼭지점의 좌표값인데 위 처럼 총 3개의 점을 찍으면 자동으로 이어지고 끝점과 시작점은 지정하지 않아도 자동으로 이어준다.
polyline
<polyline points="100,100 130,100 130,130 160,130 160,160" fill="transparent" stroke="black" stroke-width="2"></polyline>
Attribute
- points는 Polygon의 points랑 비슷하다 다만 Polygon은 다각형 도형을 만들때 사용하고 polyline은 여러개의 점을 이을때 사용한다.
이번 글에서는 SVG에 대한 설명과 대략적인 사용법만을 알아봤다.
다음 포스트에서는 실제로 SVG 코드 및 예제를 만들어보면서 SVG를 깊숙히 살펴보도록 하겠다.
