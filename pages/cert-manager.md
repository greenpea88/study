- [📁 cert- manager git hub](https://github.com/cert-manager/cert-manager)
- [📁 cert-manger docs](https://cert-manager.io/docs/)
- ## 역할
	- **Certificate manager controller**
	- Kubernetes 내부에서 HTTPS 통신을 위한 인증서를 생성
	- 인증서의 만료 기간이 되면 자동으로 인증서를 갱신
	- -> HTTPS 통신이 가능하게 해줌
- ## helm으로 설치
	- ```bash
	  helm repo add jetstack https://charts.jetstack.io
	  helm repo update
	  helm install \
	    cert-manager jetstack/cert-manager \
	    --namespace cert-manager \
	    --create-namespace \
	    --version v1.7.1 \
	    --set installCRDs=true
	  ```
	- CRD : Custom Resource Definition -> k8s 자체 제공
		- cert-manager는 인증서 발급 과정을 CRD(커스텀 리소스)로 관리함
- ## ACME
	- **Automated Certificate Management Environment** = 자동으로 인증서를 관리해주는 환경
	- 인증서를 발급받고자 할 때 **challenge**를 받고 해당 challenge를 수행해야 인증서를 받을 수 있음
	- 종류
		- HTTP01
			- 요청 받은 HTTP URL endpoint를 생성하기
		- [**DNS01**](https://cert-manager.io/docs/configuration/acme/dns01/)
			- 해당 도메인 이름 아래의 TXT 코드에 특정 값을 넣어 DNS를 제어하고 있음을 확인
			- 도메인 작성 권한이 있는 resource를 생성하고 **access key가 담긴 secret**을 만들어서 사용
		- -> [Let's encrypt 사용](https://letsencrypt.org/docs/challenge-types/)
		-
- ## component
	- ![img.png](../assets/img_1647825052551_0.png)
	- ### Issuer
		- 인증서 발급의 주체
		- self-signed, let's encrypt,,,,
		- 적용 범위
			- **모든** namespace -> **clusterIssuer**
				- ClusterIssuer.yaml
				  ```yaml
				  apiVersion: cert-manager.io/v1alpha3
				  kind: ClusterIssuer
				  metadata:
				    name: letsencrypt-prod
				  spec:
				    acme:
				      email: <ACME에 등록된 email>
				      privateKeySecretRef:
				        name: letsencrypt-prod
				      # ACME server URL
				      server: https://acme-v02.api.letsencrypt.org/directory
				      solvers:
				     # DNS01 방식 사용
				      - dns01:
				          route53:
				            accessKeyID: AKIAR2SCAHZZTIEE4MQ2
				            region: ap-northeast-2
				            role: ""
				            # NS 액세스 토큰 secret
				            secretAccessKeySecretRef:
				              key: secret-access-key
				              name: acme-route53
				        selector:
				          dnsZones:
				          - projectwith.me
				  ```
			- **특정** namespace -> **Issuer**
	- ### Certificates
		- 인증서가 인증할 도메인 정보등의 **인증서 명세** 정의
		- 생성하지 않고 **webhook**을 이용해서 ingress에 등록을 해둔 **TLS** 주소에 대한 인증서 발급을 **자동**으로 진행하도록 할 수 있음
	- ### Kubernetes Secrets
		- 발급 받은 인증서가 저장됨
	-