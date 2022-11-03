- `src/App.tsx`부터 시작
- ### tailwind 사용
	- tailwind = postcss의 `plugin`
	- 작업 환경이 다른 곳에서 작업한 것들을 일관적인 작업 환경을 맞춰주는 기능
	- class 이름을 읽어서 프로젝트에서 사용하고 있는 것들만을 css 파일을 만들어줌
	- ```shell
	  yarn add -D tailwindcss postcss autoprefixer
	  npx tailwindcss init #를 해줘야 tailwind를 적용할 수 있음
	  ```
	- `tailwind.config.js`
	  ```css
	  module.exports = {
	      content: ["./src/**/*.{js,jsx,ts,tsx}"],
	      theme: {
	          extend: {},
	      },
	      plugins: [],
	  };
	  ```
	- `index.css`
	  ```css
	  // tailwind + plugins base style
	  @tailwind base;
	  // tailwind + plugins componet classses
	  @tailwind components; // 내부에서 동작하는 것들
	  @tailwind utilities; 
	  ```
	- tag에 style을 입히고 싶으면 `base`와 `components` 사이에 적용시킬 것을 적어주면 됨
		- ```css
		  @tailwind base;
		  button {
		  	@apply bg-blue-800 text-white p-2 x-4 rounded-md;
		  }
		  @tailwind components; 
		  @tailwind utilities; 
		  ```
- components
	- 공통으로 사용하는 것들
	- 독립적인 기능을 수행하는 단위 모듈
- page : 한 페이지로 돌아가는 것들
- `yarn add @pankod/refine-react-table`
-
- ## 상태 관리
	- 변수의 값을 감지하는 것 -> 값의 변화가 일어나면 그걸 감지해서 관리하는 것을 상태 관리라고 함
	- `[a, setA] = useState()`
	- a의 값을 변경하기 위해서는 `setA(1)`로 실행
	- a는 변수라기 보다는 **상태**
	- ### hook
		- **component 안에서만** 돌아가는 함수
		- `use`로 시작하는 함수들
		- `useState()` : component 상태 관리 시 (뜸/안뜸)
		- `useEffet()` : component의 lifecycle 관리 시
			- `useEffect(() => {초기화 과정}, [의존값])`
			- = side effect 관리
			- component 생성 / 사라짐일 때 어떤 일이 일어나는지
		- 상태 관리를 쉽게하기 위해 **react hook form**을 사용
			- [react hook form](https://codesandbox.io/s/9ltw0)
			- `{...register("category.id", { required: true })}` : register 함수에서 받은 값을 value로 넣어줌
			-