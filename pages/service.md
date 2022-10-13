- yaml
  ```
  apiVersion: v1
  kind: Service
  metadata:
    name: <service 이름>
  spec:
    ports:
      - port: 8000 # the port that this service should serve on
        # the container on each pod to connect to, can be a name
        # (e.g. 'www') or a number (e.g. 80)
        targetPort: 80
        protocol: TCP
    # just like the selector in the deployment,
    # but this time it identifies the set of pods to load balance
    # traffic to.
    selector:
      app: nginx
  ```
- `servicePort`의 값은 ingress의 `spec > ports > port`의 값과 동일해야함
	- upgrade 되면서 `servicePort` 가 아닌 `ports > port`로 변경됨
	  ~-> 위와 같은 yaml 파일로 생성해도 위치 값을 자동으로 고쳐주는 듯 함~
-