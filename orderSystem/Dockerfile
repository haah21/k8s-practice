# 멀티 스테이지 빌드 방법 사용

# 첫번째 stage
FROM openjdk:11 as stage1
WORKDIR /app

#/app/gradlew 파일로 생성
COPY gradlew .
#/app/gradle 폴더로 생성
COPY gradle gradle

COPY src src
COPY build.gradle .
COPY settings.gradle .

RUN ./gradlew bootJar
# Bootjar만 필요함
# 두번째 stage (= 새로운 리눅스 환경의 컨테이너 만들음) -> 첫번째 스테이지의 bootjar만 받아옴 -> 두번째 container의 크기 줄어들음
FROM openjdk:11
WORKDIR /app
#stage1에 있는 jar를 stage2의 app.jar라는 이름으로 copy
COPY --from=stage1 /app/build/libs/*.jar app.jar

#   CMD 또는 ENTRYPOINT를 통해 컨테이너를 실행
ENTRYPOINT ["java","-jar","app.jar"]

#docker컨테이너 내에서 밖의 전체 Host를 지칭하는 도메인 : host.docker.internal
#실행하면서 환경변수 주입
#docker db : 연결 안됨 - docker-compose 써서 연결해야할듯
#docker run -d -p 8080:8080 -e SPRING_DATASOURCE_URL=jdbc:mariadb://host.docker.internal:3307/ordersystem:latest

# docker컨테이너 실행시에 볼륨을 설정할 때에는 -v 옵션
# docker run -d -p 8081:8080 -e SPRING_DATASOURCE_URL=jdbc:mariadb://host.docker.internal:3307/board -v (호스트 경로):(내부컨테이너 경로) spring_test:latest
# docker run -d -p 8080:8080 -e SPRING_DATASOURCE_URL=jdbc:mariadb://host.docker.internal:3307/board -v /Users/hannah/Desktop/tmp_logs:/app/logs ordersystem:latest