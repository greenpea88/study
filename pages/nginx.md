- ## helm으로 설치
	- [✏️ 설치할 버전의 ingress-nginx](https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx)
	- ```
	  helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
	  helm repo update
	  helm install <release 이름> ingress-nginx/ingress-nginx
	  ```
	- **삭제**
	  ```
	  helm uninstall <release 이름>
	  ```