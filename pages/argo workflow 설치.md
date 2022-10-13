- **cluster install**로 설치
	- event를 사용하기 위해
	  id:: 62aea15f-9237-4790-913d-6f4713f23500
	- 다른 namespace에서 workflow를 생성하고 실행할 수 있음
	- [argo workflow git](https://github.com/argoproj/argo-workflows)를 clone 받은 후 `manifests > cluster-install`에 위치한 `kustomization.yaml` 파일을 이용해 설치
	- ```sh
	  # 설치 전 namespace가 만들어져있어야 함
	  $> kubectl create ns argo
	  
	  # kustomization.yaml 파일을 이용한 설치
	  $> kustomize build ./manifests/cluster-install | kubectl -f -
	  ```
	- **주의** : 문제가 발생하면 service account를 의심해보자!
	- 설치한 argo server는 **port forward**를 통해 web(`localhost:2746`)으로 띄워볼 수 있음
		- ```sh
		  $> kubectl -n argo port-forward svc/argo-server 2746:2746
		  ```
		- k9s > :ns > argo > :svc > argo-server > `shift + f`
	- **service account(sa)**
		- workflow는 `default` sa를 사용
		- argo event에서 `workflow trigger` 사용 시 `operate-workflow-sa` 사용
	- **role** : 위치한 namespace에서만 적용 가능한 role
	- **role binding** : role의 권한을 sa에 binding
	- **cluster role** : cluster 전체에서 적용할 수 있는 role
	- **cluster role binding** : cluster role에 있는 권한을 sa에 binding