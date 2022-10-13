- secret 생성
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
	- ```yaml
	  # Please edit the object below. Lines beginning with a '#' will be ignored,
	  # and an empty file will abort the edit. If an error occurs while saving this file will be
	  # reopened with the relevant failures.
	  #
	  apiVersion: argoproj.io/v1alpha1
	  kind: EventSource
	  metadata:
	    finalizers:
	    - eventsource-controller
	    name: aws-sqs
	    namespace: argo-events
	  spec:
	    sqs:
	      example:
	        accessKey:
	          key: accesskey
	          name: aws-sqs-secret
	        endpoint: ""
	        jsonBody: true
	        queue: test2
	        region: ap-northeast-2
	        secretKey:
	          key: secretkey
	          name: aws-sqs-secret
	        waitTimeSeconds: 20
	  
	  ```
- sensor 생성
	- event를 감지했을 때 실행할 동작을 `trigger` 하위에 작성해주면 됨
	- ```yaml
	  # Please edit the object below. Lines beginning with a '#' will be ignored,
	  # and an empty file will abort the edit. If an error occurs while saving this file will be
	  # reopened with the relevant failures.
	  #
	  apiVersion: argoproj.io/v1alpha1
	  kind: Sensor
	  metadata:
	    finalizers:
	    - sensor-controller
	    name: aws-sqs
	    namespace: argo-events
	  spec:
	    dependencies:
	    - eventName: example
	      eventSourceName: aws-sqs
	      name: test-dep
	    template:
	      serviceAccountName: operate-workflow-sa
	    triggers:
	    - template:
	        k8s:
	          operation: create
	          parameters:
	          - dest: spec.arguments.parameters.0.value
	            src:
	              dataKey: body
	              dependencyName: test-dep
	          source:
	            resource:
	              apiVersion: argoproj.io/v1alpha1
	              kind: Workflow
	              metadata:
	                generateName: aws-sqs-workflow-
	              spec:
	                arguments:
	                  parameters:
	                  - name: message
	                    value: hello world
	                entrypoint: whalesay
	                templates:
	                - container:
	                    args:
	                    - '{{inputs.parameters.message}}'
	                    command:
	                    - cowsay
	                    image: docker/whalesay:latest
	                  inputs:
	                    parameters:
	                    - name: message
	                  name: whalesay
	        name: sqs-workflow
	  ```