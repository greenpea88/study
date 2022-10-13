- `stern`을 이용한 k8s namespace log 확인하기
	- ```sh
	  stern -n {namespace} . #해당 namespace의 모든 log 확인 가능
	  ```
- argo workflow의 access token 받기
	- ```sh
	  echo "Bearer $(kubectl -n argo get secret $(kubectl -n argo get sa argo -o jsonpath='{.secrets[0].name}') -o jsonpath='{.data.token}' | base64 -d)" | pbcopy
	  ```
- 삭제 시도 시 진행되지 않는다면 `finalizer`를 확인
	- https://github.com/argoproj/argo-events/issues/1348
	- `finalizer` : delete시 삭제를 알리기를 설정한 곳
		- 알림을 받는 곳이 먼저 삭제가 될 경우 삭제를 알릴 수 없어 block되는 이슈 발생
		- `finalizer : {설정된_finalizer}` -> `finalizer: null`로 변경하면 해결
- 터미널에서 s3 bucket 확인하기
	- ```sh
	  # 존재하는 s3 bucket 불러오기
	  $> aws s3 ls --profile {aws_profile}
	  
	  # 특정 s3 bucket 조회
	  $> aws s3 ls {bucket_name} --profile {aws_profile}
	  
	  # 특정 s3 bucket의 파일 내부 목록 조회
	  $> aws s3 ls s3://{bucket_name}/{path} --profile {aws_profile}
	  ```