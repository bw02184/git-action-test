# git-action-test
### .github/workflows/git_test.yml
- 테스트 코드를 자동으로 실행시켜 주며 마스터에 머지 할 때 Codecov 기반 코드 커버리지를 알려준다.
- 테스트에 실행되는 DB는 mysql이며 캐시를 도입하여 속도가 빠르게 실행되도록 하였다.
- 관련 노션 링크 : https://responsible-cardigan-ecb.notion.site/GitHub-Action-fd515e30adb349e290bfd4b1f4bc1b06

### .github/workflows/eb_deploy.yml
- jar파일을 이용하여 AWS Beanstalk에 빌드 후 배포하는 yml파일이다.
- jobs를 build와 deploy로 나누어 빌드가 완료된 후 배포되도록 작성하였다.
- 캐시를 추가하여 속도가 빠르게 실행되도록 하였으며 AWS 보안그룹 떄문에 RDS가 실행되지 않는 문제점이 발생하여 빌드시에 깃액션의 ip를 보안그룹에 추가 하고 빌드가 완료된 후 무조건(빌드가 실패하더라도) 보안그룹에서 깃 액션의 ip를 다시 제거하는 방식으로 RDS가 작동하도록 작성하였다.
- 관련 노션 링크 : https://responsible-cardigan-ecb.notion.site/Beanstalk-jar-Deploy-cfe18a9a68c54d7fb06ae8883de74e7d

### .github/workflows/eb_docker_deploy.yml
- 도커파일을 이용하여 빌드 후 AWS Beanstalk에 배포하는 yml파일이다.
- 도커파일을 빌드해서 도커허브에 이미지파일을 올려놓으면 AWS상에서 도커허브의 이미지 파일을 이용해서 배포하는 방식이다.
- ./src/main/resources/application.properties 에 중요한 정보(DB id, password 등)들이 많다고 생각되어 깃허브에서 삭제 후 깃 시크릿으르로 저장하여 다운 받는 방식을 사용하였다.
-  Dockerfile과 Dockerrun.aws.json파일이 루트에 있어야 한다.
-  관련 노션 링크 : https://responsible-cardigan-ecb.notion.site/EB-Docker-Deploy-31748ed4210e429d93859d778dd16dfa

### Dockerfile
- 이미지 파일을 나눠 받는 방식을 사용하여 속도를 빠르게 했다.
- MAC OS와 Windows OS의 개행 방식 차이에 따라 개행문자 변경해 주는 dos2unix설치 후 gradlew 변경해 주는 과정을 더해 주었다.(windows 환경이기 때문에 추가했다.)
