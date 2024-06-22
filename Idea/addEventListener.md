```js
document.querySelector("#app").innerHTML = `
	<button type="button">This is Button</button>
	
	<input class="name" type="text" placaholder="Type youre name: " />
	
    <div class="parent-of-button">
      <button class="helloworld-button" type="button">
        <span>Hello</span>
        <span>World</span>
      </button>
    </div>
`

document.querySelector("button").addEventListener("click", (evnet) => {
	console.log(event);
})

document.querySelector(".name").addEventListener("change", (evnet) => {
	console.log(event);
})

document.querySelector(".name").addEventListener("input", (evnet) => {
	console.log(event);
})

  document
    .querySelector(".helloworld-button")
    .addEventListener("click", (event) => {
      event.stopPropagation();
      console.log("event from button", event);
    });

  document
    .querySelector(".parent-of-button")
    .addEventListener("click", (event) => {
      console.log("event from div", event);
    });

  document.querySelector(".name").addEventListener("keyup", (event) => {
    console.log("input keyup", event);
  });

  document.body.addEventListener("keyup", (event) => {
    console.log(event.key);
  });
```

input의 change 이벤트는 모든 입력이 끝난 후에 callback을 호출한다.

input의 input 이벤트는 입력 마다 callback을 호출한다.

**Event Bubbling**이 소개되었다.

> 모든 컴포넌트의 이벤트는 기본적으로 **Event Bubbling**되어 부모 태그로 전파된다.



