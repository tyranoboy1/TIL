# JavaScript Study

## 이벤트

### 이벤트 드리븐 프로그래밍

이벤트가 발생했을 때 호출될 함수를 *이벤트 핸들러*라고 하고, 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 *이벤트 핸들러 등록*이라고 한다.<br/>

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me</button>
    <script>
      const $button = document.querySelector("button");
      // 사용자가 버튼을 클릭하면 함수를 호출
      $button.onClick = () => {
        alter("button click");
      };
    </script>
  </body>
</html>
```

프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 *이벤트 드리븐 프로그래밍*이라고 한다.

### 이벤트 타입

1. 마우스 이벤트<br/>
   click => 마우스 버튼을 클릭했을때<br/>
   dblclick => 마우스 버튼을 더블 클릭했을때<br/>
   mousedown => 마우스 버튼을 눌렀을때<br/>
   mouseup => 누르고 있던 마우스 버튼을 놓았을때<br/>
   mousemove => 마우스 커서를 움직였을 때<br/>
   mouseenter => 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링 되지 않는다.)<br/>
   mouseover => 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링 된다.)<br/>
   mouseleave => 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링 되지 않는다.)<br/>
   mouseout => 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링 된다.)<br/>

2. 키보드 이벤트<br/>
   keydown => 모든 키를 눌렀을 때 발생한다.<br/>

   - control,option,shift,tab,delete,enter, 방향 키와 문자, 숫자, 특수 문자 키를 눌렀을 때 발생한다.<br/>
   - 단, 문자,숫자,특수 문자, enter 키를 눌렀을 때는 연속적으로 발생하지만 그외의 키를 눌럿을 때는 한 번만 발생한다.<br/>

   keypresss => 문자 키를 눌렀을 때 연속적으로 발생한다.<br/>

   - control,option,shift,tab,delete,방향 키등을 눌렀을 때는 발생하지 않고 문자, 숫자, 특수 문자, enter키를 눌렀을 때만 발생한다.<br/>
   - 폐지되었으므로 사용하지 않을 것을 권장한다.<br/>

   keyup => 누르고 있던 키를 놓았을 때 한 번만 발생한다<br/>

   - keydown 이벤트와 마찬가지로 control,option,shift,tab,delete,enter,방향 키와 문자, 숫자,특수 문자 키를 놓았을때 발생한다.<br/>

3. 포커스 이벤트<br/>
   focus => HTML 요소가 포커스를 받았을 때(버블링 되지 않는다.)<br/>
   blur => HTML 요소가 포커스를 잃었을 때(버블링 되지 않는다.)<br/>
   focusin => HTML 요소가 포커스를 받았을 때(버블링 된다.)<br/>
   focusout => HTML 요소가 포커스를 잃었을 때(버블링 된다.)<br/>

4. 폼 이벤트<br/>
   submit<br/>

   - form 요소 내의 input(text,checkbox,radio), select 입력 필드(textarea 제외)에서 엔터키를 눌렀을때<br/>
   - form 요소 내의 submit 버튼(<button>, <input type="submit">)을 클릭했을 때<br/>

   * submit 이벤트는 form 요소에서 발생한다.<br/>

   - reset form 요소 내의 reset 버튼을 클릭했을 때(최근에는 사용안함)<br/>

5. 값 변경 이벤트<br/>
   input<br/>

   - input(text,checkbox,radio),select,textarea 요소의 값이 입력되었을 때<br/>

   change<br/>

   - input (text, checkbox,radio),select,textarea 요소의 값이 변경되었을 때<br/>

   * change 이벤트는 input 이벤트와는 달리 HTML 요소가 포커스를 잃었을 때 사용자 입력이 종료되었다고 인식하여 발생한다.<br/>
     즉, 사용자가 입력을 하고 있을때는 input 이벤트가 발생하고 사용자가 입력이 종료되어 값이 변경되면 change 이벤트가 발생한다.<br/>

   readystatechange<br/>

   - HTML 문서의 로드와 파싱 상태를 나타내는 document.readyState 프로퍼티 값('loading','interactive','complete')이 변경될 때<br/>

6. DOM 뮤테이션 이벤트<br/>

   DOMContentLoaded => HTML 문서의 로드와 파싱이 완료되어 DOM 생성이 완료되었을때<br/>

7. 뷰 이벤트<br/>

   resize => 브라우저 윈도우(window)의 크기를 리사이즈 할 때 연속적으로 발생한다.<br/>

   - 오직 window 객체에서만 발생<br/>

   scroll => 웹페이지 또는 HTML 요소를 스크롤 할때 연속적으로 발생한다.<br/>

8. 리소스 이벤트<br/>

   load => DOMContentLoaded 이벤트가 발생한 이후, 모든 리소스(이미지, 폰트 등)의 로딩이 완료되었을때(주로 window 객체에서 발생)<br/>
   unload => 리소스가 언로드 될 때(주로 새로운 웹페이지를 요청한 경우)<br/>
   abort => 리소스 로딩이 중단되었을 때<br/>
   error => 리소스 로딩이 실패했을 때<br/>

### 이벤트 핸들러 어트리뷰트 방식

_이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미한다._ <br/>

```javascript
function onclick(event) {
  sayHi("Lee");
}
```

### 이벤트 핸들러 프로퍼티 방식

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me</button>
    <script>
      const $button = document.querySelector("button");

      // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
      $button.onclick = function () {
        console.log("button click");
      };
    </script>
  </body>
</html>
```

이벤트 핸들러를 등록하기 위해서는 이벤트를 발생시킬 객체인 *이벤트 타깃*과 이벤트의 종류를 나타내는 문자열인 _이벤트 타입_ 그리고 *이벤트 핸들러*를 지정할 필요가 있다.<br/>

### addEventListener 메서드 방식

EventTarget.addEventListener('eventType', functionName, [, useCapture])<br/>

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me</button>
    <script>
      const $button = document.querySelector("button");

      // addEventListener 메서드는 동일한 요소에서 발생한 동일한 이벤트에 대해 하나 이상의 이벤트 핸들러를 등록할 수 있다.
      $button.addEventListener("click", function () {
        console.log("[1]button click");
      });
      $button.addEventListener("click", function () {
        console.log("[2]button click");
      });
    </script>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me</button>
    <script>
      const $button = document.querySelector("button");

      const handleClick = () => console.log("button click");

      // 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 핸들러만 등록된다.
      $button.addEventListener("click", handleClick);
      $button.addEventListener("click", handleClick);
    </script>
  </body>
</html>
```

### 이벤트 핸들러 제거

addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.<br/>

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me</button>
    <script>
      const $button = document.querySelector("button");
      const handleClick = () => console.log("button click");
      // 이벤트 핸들러 등록
      $button.addEventListener("click", handleClick);
      // 이벤트 핸들러 제거
      // addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.

      $button.removeEventListener("click", handleClick, true); // 실패
      $button.removeEventListener("click", handleClick); // 성공
    </script>
  </body>
</html>
```

이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 removeEventListener 메서드로 제거할 수 없다.<br/>
이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 이벤트 핸들러 프로퍼티에 null을 할당한다.<br/>

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me</button>
    <script>
      const $button = document.querySelector("button");
      const handleClick = () => console.log("button click");

      // 이벤트 핸들러 프로퍼티 방식으로 이벤트 핸들러 등록
      $button.onclick = handleClick;

      // removeEventListener 메서드로 이벤트 핸들러를 제거할 수 없다.
      $button.removeEventListener("click", handleClick);

      // 이벤트 핸들러 프로퍼티에 null을 할당하여 이벤트 핸들러를 제거한다.
      $button.onclick = null;
    </script>
  </body>
</html>
```

### 이벤트 전파

_생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파된다._<br/>

이벤트 전파는 이벤트 객체가 전파되는 방향에 따라 3단계로 분류<br/>

1. 캡처링 단계 => 이벤트가 상위 요소에서 하위 요소 방향으로 전파<br/>
2. 타깃 단계 => 이벤트가 이벤트 타깃에 도달<br/>
3. 버블링 단계 => 이벤트가 하위 요소에서 상위 요소 방향으로 전파<br/>

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById("fruits");

      // $fruits 요소의 하위 요소인 li 요소를 클릭한 경우
      $fruits.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLIElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

이벤트 타깃(event.target)은 li 요소이고 커런트 타깃(currentTarget)은 ul 요소다.<br/>

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById("fruits");
      const $banana = document.getElementById("banana");

      // $fruits 요소의 하위 요소인 li 요소를 클릭한 경우 캡처링 단계의 이벤트를 캐치한다.
      $fruits.addEventListener(
        "click",
        (e) => {
          console.log(`이벤트 단계: ${e.eventPhase}`); // 1: 캡처링 단계
          console.log(`이벤트 타깃: ${e.target}`); // [object HTMLIElement]
          console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
        },
        true
      );
      // 타깃 단계의 이벤트를 캐치한다.
      $fruits.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 2: 타깃 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLIElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
      // $fruits 요소의 하위 요소인 li 요소를 클릭한 경우 캡처링 단계의 이벤트를 캐치한다.
      $fruits.addEventListener("click", (e) => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLIElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

_이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치할 수 있다._<br/>
