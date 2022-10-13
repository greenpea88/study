- secret 설정
- ```yaml
  # Please edit the object below. Lines beginning with a '#' will be ignored,
  # and an empty file will abort the edit. If an error occurs while saving this file will be
  # reopened with the relevant failures.
  #
  apiVersion: v1
  data:
    accesskey: {base64 accesskey}
    secretkey: {base64 secretkey}
  kind: Secret
  metadata:
    creationTimestamp: "2022-03-28T06:03:59Z"
    name: aws-sqs-secret
    namespace: argo-events
    resourceVersion: "425719173"
    uid: 0bf586aa-fb56-4d71-a9c2-cb01437655fa
  type: Opaque
  ```
- event source 생성