- `@pankod/refine-simple-reset`를 통해서 사용 가능
- Data Provider를 통해서 원하는 data를 받아올 수 있음
- ## Refine API consuming flow
- ![스크린샷 2022-11-03 오후 2.52.34.png](../assets/스크린샷_2022-11-03_오후_2.52.34_1667454757682_0.png)
- ## 사용하기
	- `App.tsx`에서 dataProvider를 등록해서 사용
	- ```tsx
	  const App: React.FC = () => {
	  	return (
	        <Refine
	        	dataProvider = {{
	        default: defaultDataProvider,
	        example: exampleDataProvider,
	        }}
	      />
	      );
	  };
	  ```
- **format에 맞게 custom을 통해 원하는 정보를 받아오게할 수 있음**
- ## Data Provider Custom
	- `getList`, `getOne`, `deleteOne`, `create`, `update` 는 필수로 구현해야함
	- ```tsx
	  import axios, { AxiosInstance } from "axios";
	  import { DataProvider } from "./interfaces/dataProvider.ts";
	  
	  const axiosInstance = axios.create();
	  
	  const SimpleRestDataProvider = (
	      apiUrl: string,
	      httpClient: AxiosInstance = axiosInstance,
	  ): DataProvider => ({
	    	// data 생성
	      create: ({ resource, variables, metaData }) => Promise,
	      createMany?: ({ resource, variables, metaData }) => Promise,
	    	// data 삭제
	      deleteOne: ({ resource, id, variables, metaData }) => Promise,
	      deleteMany?: ({ resource, ids, variables, metaData }) => Promise,
	      // 값 list로 불러오기
	      getList: ({ resource, pagination, sort, filters, metaData }) => Promise,
	      getMany?: ({ resource, ids, metaData }) => Promise,
	      // 값 하나만 불러오기
	      getOne: ({ resource, id, metaData }) => Promise,
	      // 값 수정하기
	      update: ({ resource, id, variables, metaData }) => Promise,
	      updateMany?: ({ resource, ids, variables, metaData }) => Promise,
	      custom: ({
	          url,
	          method,
	          sort,
	          filters,
	          payload,
	          query,
	          headers,
	          metaData,
	      }) => Promise,
	      getApiUrl: () => "",
	  });
	  ```
	- `getList`
		- return format
		  ```json
		  {
		    data: "",
		    total: ""
		  }
		  ```
	- `custom` : CRUD 제외한 작업을 할 때 사용