- ## helm으로 설치
	- ```
	  helm repo add kong https://charts.konghq.com
	  helm repo update
	  helm install kong kong/kong --set ingressController.installCRDs=false -n kong --create-namespace
	  ```
	- **values.yaml** 파일 생성하고 적용
		- 사용할 이미지 변경하고 적용 -> 한 곳의 값만 변경한 것이기 때문에 `--set` option을 사용하여 변경도 가능
		- ```
		  helm upgrade kong kong/kong -n kong -f my_kong_values.yaml
		  ```
	- service이지만 type을 load balancer로 열어서 ingress로 사용
	- DB-less 설정이 가능하기 때문에 **yaml 파일에** 값을 넣어 선언적으로 사용
	- 구성
		- ingress-controller :  특정 주소로 가는 것을 캐치해서 proxy로 전달해줌
		- proxy : 실제로 요청을 처리하는 것 -> 특정 주소에 따라 처리할 서비스로 연결해줌
	- **삭제**
	  ```
	  helm uninstall kong -n kong
	  ```
- ## kong을 ingress로 사용하기
	- **ingress**의 yaml 파일에 **kong을 ingress로 사용할 것을 명시해주어야 사용 가능**
	- ```
	  metadata:
	  	annotations:
	      	kubernetes.io/ingress.class: kong
	  ```
	- 명시하지 않을 경우 nginx를 default 값으로 가짐
	- ingress의 종류가 1개만 있을 경우에는 써주지 않아도 됨
- ## plugin 사용 가능
	- 다양한 plugin의 적용을 통해 기능을 확장시킬 수 있음
	- 여러개의 plugin을 사용할 때에는 `,`로 연결하여 작성
	- **service** yaml의 `metadata > annotations` 아래에 사용할 plugin의 값을 추가 
	  ```
	  metadata:
	  	.
	      .
	      .
	      annotations:
	      	konghq.com/plugins: <plugin 이름>
	  ```