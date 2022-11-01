- `src/App.tsx`부터 시작
- tailwind <- postcss의 plugin
	- 작업 환경이 다른 곳에서 작업한 것들을 일관적인 작업 환경을 맞춰주는 기능
	- class 이름을 읽어서 프로젝트에서 사용하고 있는 것들만을 css 파일을 만들어줌
	- `index.css`
	  ```
	  @tailwind base; -> 완전 기본으로 사용하는 것
	  @tailwind components; -> 사용하고 있는 것들 엊저구
	  @tailwind utilities;
	  ```
	- tag에 style을 입히고 싶으면 base와 compoents 사이에 적용시킬 것을 적어주면 됨
- components: 공통으로 사용하는 것들
- pages: 한 페이지로 돌아가는 것들
- `yarn add @pankod/refine-react-table`
-
- cell? :