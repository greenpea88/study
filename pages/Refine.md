- **Headless Admin**
	- 구성 요소를 제공해줌
	- library만을 제공하기 때문에 눈에 보이는 부분은 직접 만들어내야 함(ex. tailwind 이용 등등,,)
- [Refine Tutorial](https://refine.dev/docs/tutorials/headless-tutorial/)
	- [[Tutorial]]
- [Refine Git](https://github.com/refinedev/refine)
- `npx create-react-app app명`을 이용해서 시작
	- `tsconfig`를 이용해야 import 설정을 바꿀 수 있음
- `yarn add @pankod/refine-core @pankod/refine-react-router-v6`
	- refine용으로 custom해서 제공해주는 packages
- data 사용을 위한 [Fake Rest api](https://api.fake-rest.refine.dev/)을 제공함
	- users, post, categories를 이용해서 tutorial 진행
- ### App.tsx
	- ```tsx
	  export const App: React.FC = () => {
	  	return (
	        <Refine
	        	routerProvider={routerProvider},
	          // data 받아오는 곳
	        	dataProvider={dataProvider("https://api.fake-rest.refine.dev")},
	          // 요청해서 받아오는 특정 data들
	          // name(required) : endpoint가 name의 값으로 만들어져야 함
	          resources={[{name: "posts", icon: PostIcon}]}
	          Layout
	          LoginPage
	          />
	      )
	  }
	  ```
- ## Provider
	- 각 provider를 custom해서 원하는 정보를 가져오도록 변경할 수 있음
	- [[Data Provider]]
		- [**Data provider**](https://refine.dev/docs/tutorials/headless-tutorial/#using-a-dataprovider)를 이용해서 data를 가져올 수 있음
			- 주소 체계가 동일하면 하나로 사용해서 할 수 있음
			- 요구사항이 단순한 경우에 유리함
			- 내부적으로 `React query`를 감싸서 사용할 수 있도록 되어있음
			- `yarn add @pankod/refine-simple-rest` -> 이용해서 사용할 수 있음
			- 주소 체계에 맞게 **Data provider**를 만들면 사용하고 싶은 data를 쉽게 사용 가능
	- [[Auth Provider]]
		- [**Auth provider**](https://refine.dev/docs/api-reference/core/providers/auth-provider/)
		- `login`을 쉽게 붙일 수 있음
- `cell` 을 사용해서 table을 그릴 수 있음