# JavaScript Study

## 프로미스

### 비동기 처리를 위한 콜백 패턴의 단점

1. 콜백헬<br/>

```javascript
const get = (url) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();
  // onload 이벤트 핸들러가 비동기로 동작
  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콘솔에 출력한다.
      console.log(JSON.parse(xhr.response));
    } else {
      console.log(`${xhr.status} ${xhr.statusText}`);
    }
  };
};
// id가 1인 post를 취득
get("https://jsonplaceholder.typicode.com/posts/1");
```

_비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료된다._<br/>
_즉 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다._<br/>
_비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다._<br/>

```javascript
// GET 요청을 위한 비동기 함수
const get = (url) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 1 서버의 응답을 반환한다.
      return JSON.parse(xhr.response);
    }
    console.error(`${xhr.status} ${xhr.statusText}`);
  };
};

// 2 id가 1인 post를 취득
const response = get('https://jsonplaceholder.typicode.com/posts/1`)
console.log(response) // undefined
```

```html
<!DOCTYPE html>
<body>
    <input type="text">
    <script>
        document.querySelector('input').oninput = function (){
            console.log(this.value)
            // 이벤트 핸들러에서의 반환은 의미가 없다.
            return this.value
        }
        </script>
    </body>
</html>
```

```javascript
let todos
// GET 요청을 위한 비동기 함수
const get = (url) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 1 서버의 응답을 상위 스코프의 변수에 할당한다.
      todos = JSON.parse(xhr.response)
    }
    else{
        console.error(`${xhr.status} ${xhr.statusText}`);
    }
  };
};

// 2 id가 1인 post를 취득
const response = get('https://jsonplaceholder.typicode.com/posts/1`)
console.log(todos) // undefined
```

_xhr.onload 핸들러 프로퍼티에 바인딩한 이벤트 핸들러가 즉시 실행되는 것이 아니다._<br/>
_xhr.onload 이벤트 핸들러는 load 이벤트가 발생하면 일단 태스크 큐에 저장되어 대기하다가. 콜 스택이 비면 이벤트 루프에 의해 콜스택으로 푸시되어 실행된다._<br/>

_비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다. 따라서 비동기 함수의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야한다._<br/>
_비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다._ => _필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 실패하면 호출될 콜백 함수를 전달할 수 있다._

```javascript
// GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리를 한다.
      successCallback(JSON.parse(xhr.response))
    }
    else{
        // 에러 정보를 콜백 함수에 인수로 전달하면서 호출하여 에러 처리를 한다.
        failureCallback(xhr.status)
    }
  };
};
// id가 1인 post를 취득
// 서버의 응답에 대한 후속 처리를 위한 콜백 함수를 비동기 함수인 get에 전달해야 된다.
get('https://jsonplaceholder.typicode.com/posts/1`)
```

콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또 다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상 => 콜백헬

```javascript
// GET 요청을 위한 비동기 함수
const get = (url, callback) => {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", url);
  xhr.send();

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리를 한다.
      callback(JSON.parse(xhr.response));
    } else {
      console.error(`${xhr.status} ${xhr.statusText}`);
    }
  };
};
const url = "https://jsonplaceholder.typicode.com";
get(`${url}/post/1`, ({ userId }) => {
  console.log(userId); // 1
  // post의 userId를 사용하여 user 정보를 취득
  get(`${url}/users/${userId}`, (userInfo) => {
    console.log(userInfo);
  });
});
```

```javascript
get("/step1", (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        console.log(d);
      });
    });
  });
});
```

```javascript
// 비동기 콜백 패턴의 가장 큰 문제점 => 에러 처리 곤란
try {
  setTimeout(() => {
    throw new Error("Error!");
  }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다.
  console.error("캐치한 에러", e);
}
```

_에러는 호출자 방향으로 전파된다._<br/>
즉 콜 스택의 아래 방향(실행 중인 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.
