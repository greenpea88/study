- [๐ cert- manager git hub](https://github.com/cert-manager/cert-manager)
- [๐ cert-manger docs](https://cert-manager.io/docs/)
- ## ์ญํ 
	- **Certificate manager controller**
	- Kubernetes ๋ด๋ถ์์ HTTPS ํต์ ์ ์ํ ์ธ์ฆ์๋ฅผ ์์ฑ
	- ์ธ์ฆ์์ ๋ง๋ฃ ๊ธฐ๊ฐ์ด ๋๋ฉด ์๋์ผ๋ก ์ธ์ฆ์๋ฅผ ๊ฐฑ์ 
	- -> HTTPS ํต์ ์ด ๊ฐ๋ฅํ๊ฒ ํด์ค
- ## helm์ผ๋ก ์ค์น
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
	- CRD : Custom Resource Definition -> k8s ์์ฒด ์ ๊ณต
		- cert-manager๋ ์ธ์ฆ์ ๋ฐ๊ธ ๊ณผ์ ์ CRD(์ปค์คํ ๋ฆฌ์์ค)๋ก ๊ด๋ฆฌํจ
- ## ACME
	- **Automated Certificate Management Environment** = ์๋์ผ๋ก ์ธ์ฆ์๋ฅผ ๊ด๋ฆฌํด์ฃผ๋ ํ๊ฒฝ
	- ์ธ์ฆ์๋ฅผ ๋ฐ๊ธ๋ฐ๊ณ ์ ํ  ๋ **challenge**๋ฅผ ๋ฐ๊ณ  ํด๋น challenge๋ฅผ ์ํํด์ผ ์ธ์ฆ์๋ฅผ ๋ฐ์ ์ ์์
	- ์ข๋ฅ
		- HTTP01
			- ์์ฒญ ๋ฐ์ HTTP URL endpoint๋ฅผ ์์ฑํ๊ธฐ
		- [**DNS01**](https://cert-manager.io/docs/configuration/acme/dns01/)
			- ํด๋น ๋๋ฉ์ธ ์ด๋ฆ ์๋์ TXT ์ฝ๋์ ํน์  ๊ฐ์ ๋ฃ์ด DNS๋ฅผ ์ ์ดํ๊ณ  ์์์ ํ์ธ
			- ๋๋ฉ์ธ ์์ฑ ๊ถํ์ด ์๋ resource๋ฅผ ์์ฑํ๊ณ  **access key๊ฐ ๋ด๊ธด secret**์ ๋ง๋ค์ด์ ์ฌ์ฉ
		- -> [Let's encrypt ์ฌ์ฉ](https://letsencrypt.org/docs/challenge-types/)
		-
- ## component
	- ![img.png](../assets/img_1647825052551_0.png)
	- ### Issuer
		- ์ธ์ฆ์ ๋ฐ๊ธ์ ์ฃผ์ฒด
		- self-signed, let's encrypt,,,,
		- ์ ์ฉ ๋ฒ์
			- **๋ชจ๋ ** namespace -> **clusterIssuer**
				- ClusterIssuer.yaml
				  ```yaml
				  apiVersion: cert-manager.io/v1alpha3
				  kind: ClusterIssuer
				  metadata:
				    name: letsencrypt-prod
				  spec:
				    acme:
				      email: <ACME์ ๋ฑ๋ก๋ email>
				      privateKeySecretRef:
				        name: letsencrypt-prod
				      # ACME server URL
				      server: https://acme-v02.api.letsencrypt.org/directory
				      solvers:
				     # DNS01 ๋ฐฉ์ ์ฌ์ฉ
				      - dns01:
				          route53:
				            accessKeyID: AKIAR2SCAHZZTIEE4MQ2
				            region: ap-northeast-2
				            role: ""
				            # NS ์ก์ธ์ค ํ ํฐ secret
				            secretAccessKeySecretRef:
				              key: secret-access-key
				              name: acme-route53
				        selector:
				          dnsZones:
				          - projectwith.me
				  ```
			- **ํน์ ** namespace -> **Issuer**
	- ### Certificates
		- ์ธ์ฆ์๊ฐ ์ธ์ฆํ  ๋๋ฉ์ธ ์ ๋ณด๋ฑ์ **์ธ์ฆ์ ๋ช์ธ** ์ ์
		- ์์ฑํ์ง ์๊ณ  **webhook**์ ์ด์ฉํด์ ingress์ ๋ฑ๋ก์ ํด๋ **TLS** ์ฃผ์์ ๋ํ ์ธ์ฆ์ ๋ฐ๊ธ์ **์๋**์ผ๋ก ์งํํ๋๋ก ํ  ์ ์์
	- ### Kubernetes Secrets
		- ๋ฐ๊ธ ๋ฐ์ ์ธ์ฆ์๊ฐ ์ ์ฅ๋จ
	-