- ## 서비스가 동작하는 환경(AWS)
	- **EKS**(Elastic Kubernetes Service)
	- **Route53** : DNS 관리 서비스
	- ### flow
		- 사용자 -> ALB(아마존 로드밸런서) -> EKS 접속 -> kong ingress : host/path에 따라 보내줄 서비스를 결정해서 보내줌 -> web(glob-web) : Next.js
		  
		  kong ingress가 auth와 계속해서 통신을 하면서 권한이 맞는지 확인을 함
		  **kong ingress** : L7 router인데 programming 기능이 존재함 / path 별로 다른 서비스에게 보내줌
		  token에 대해서 처리를 해서 header에 사용자에 대한 정보를 담아서 api로 보내줌
- ## service들(module)
	- course : 강의 정보
	- showroom : 첫 페이지 구성 -> banner
	- web
	- ops : 서비스 배포를 위한 설정이 적혀있음
	- permission : 권한 관리 >> 강의에 대한 접근 권한 / user-course 매칭 -> 내가 구매한 강의 목록 등 확인할 때
	- auth : 인증 서비스 >> 회원 가입, 로그인, 비밀번호 변경, 토큰 발급/폐기
		- login 상태 아님 -> auth.project.lion 보냄 -> login -> code를 받아서 project-lion -> code로 token 요청
	- play-log : 플레이 이력 >> progress server가 값을 받음 >> play 이력의 대한 정보를 **수신**받는 곳
	- progress : 진도 >> 진도에 대한 정보를 **요청**할 때
	- notification : 알림 >> 카카오톡/이메일/문자
	- payment : 결제 처리 >> 중간에 import(?)가 껴있음
	- coupon : 쿠폰 발행(admin에서)
	- inquiry : 1대1 문의 등록 및 처리
	-
	- peer group(big pie) : 스터디 그룹(?)
	- my-chew(BE), greek(FE) : 파트너 사이트 -> 강사 q&a >> 강사가 바로 답변할 수 있는 서비스
	- espresso(FE) : 인증 서비스 구현을 위한 FE boiler plate
	-
	- swagger : 모든 api를 한 번에 확인할 수 있는 곳
- ## api
	- ### api 종류
	- public api : public, 사용자에 상관 없이 같은 화면을 보여줌
		- /api/[module]/[endpoint]
	- user api : private, 사용자 별로 access token을 필요로 함
		- /api/**my**/[module]/endpoint
		- 요청을 보낼 때 항상 token을 받아서 실행함
	- admin api : /api/**admin**/[module]/endpoint
-
	- ### api 구성 방식
		- **host + path**의 모양으로 구성되어있음
		- **/api/[module명]/[version]**
		- /api/showroom/v1/banners
		  /api/course/v1/promotions