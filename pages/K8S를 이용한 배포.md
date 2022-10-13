- # 도메인 연결하기
	- 발급 받은(산) 도메인 이름과 사용할  **Name Server**를 연결해줌
	- **DNS 질의** 명령어
		- `dig`
		- `nslookup`
- # AWS
	- ## route53
		- aws의 DNS 관리 서비스
		- `create hosted zone` 을 통해 hosted zone에 사용할 도메인을 등록
		- domain name : 사용할 domain 이름
		- type
			- private : 내부 ip를 사용하는 경우
			- public :  private 외의 모든 상황
		-
		- 등록 완료 시 NS(Name Server) 생성 -> 생성한 NS를 도메인 관리 페이지에 입력해주면 됨
			- => hosted zone에 등록 = 도메인 이름에 대한 ip 주소를 받아올 수 있는 곳을 등록
	-
		- **DNS record type**
			- `create record`를 통해 생성
			- **A** : **특정한 IP 주소**로 보냄
			- **CNAME** :  IP 주소를 얻을 수 있은 **다른 도메인 주소**를 알려줌
			- +) MX : mail과 관련
	- ## EKS cluster 생성하기
		- cluster 생성
		  ```
		  eksctl create cluster \
		  --name <생성할 cluster 이름> \
		  --version <kubernetes version> \ 
		  --region <aws region>
		  
		  # kubernetes version으로는 1.21을 사용했음
		  ```
		- local에 생성한 cluster 추가
		  ```
		  aws eks update-kubeconfing --name <cluster 이름>
		  ```
		- **cloud formation**을 만들어내고 실행
		- **VPC, EKS cluster, node group**(default로 2개) 생성됨
	- ## K8S 구성 요소 생성
		- cluster 생성 후에는 **[[deployment]], [[service]], [[ingress]]를 `yaml` 파일을 이용해서 생성**해주면 됨
			- `yaml` 파일은 `kubectl example`(확장 프로그램)을 통해서 template을 받아올 수 있음
	- ## 인증서 발급
		- [[cert-manager]]를 통해 사용할 수 있음
		-