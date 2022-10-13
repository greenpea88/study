- **reverse proxy**
	- 뒷단에 있는 서비스와 요청을 연결해주는 proxy
	- 특정 서비스로 요청이 왔을 때 서비스로 연결해주는 역할
- 사용하고 있는 방식
- 1. ingress 역할을 하는 service
  2. type을 **load balancer**로 설정
  3. **application load balancer**가 생성됨
  4. 해당 ingress(실제 service)를 이용해서 요청을 받을 수 있음
  5. 도메인 주소를 생성된 application load balancer에 연결
- ### ingress 등록
	- 종류
		- [[nginx]]
		- [[kong]]
	- **helm**을 이용해 설치
		- [📁 사용할 helm chart 검색](https://artifacthub.io/)
		- 설치
		  ```
		  helm repo add ingress <생성할 이름> <helm chart 다운로드 주소>
		   
		  helm repo update
		  
		  helm install <release 이름> <chart 이름(repo 이름/chart 이름) -n <namespace 이름>
		  # namespace가 없을 경우에는 -create--namespace option을 사용
		  ```
		- 설치를 하면 **application load balancer**가 생성됨
	- **설치한 ingress를 사용**하기 위해서는 **ingress의 `metadata > annotations > kubernetes.io/ingress.class`** 의 값을 **사용할 ingress로 지정**을 해줘야 함
- yaml
  ```
  apiVersion: networking.k8s.io/v1beta1
  kind: Ingress
  metadata:
    name: <ingress 이름>
  spec:
    rules:
      - host: nginx.kong.projectwith.me
        http:
          paths:
            - backend:
                serviceName: nginx-service
                servicePort: 80
  ```
- `spec > ports > port`의 값은 service의 `servicePort` 값과 동일해야함