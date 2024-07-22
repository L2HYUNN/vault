React를 사용해 보았다면 아래와 같이 컴포넌트에 인자를 넘길때 객체를 전달하는 방식에 익숙할 것이다.

```jsx
export default function Profile() {  
	return (  
		<Avatar  
			person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}  
			size={100}  
		/>  
	);  
}
```

