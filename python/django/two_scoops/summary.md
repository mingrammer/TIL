# Summary of Two Scoops of Django

## 1. 코딩 스타일

* 읽기 쉬운 코드
  * 축약적이거나 함축적인 변수명은 피한다.
  * 함수 인자의 이름들은 꼭 써 준다.
  * 클래스와 메서드를 문서화한다.
  * 코드에 주석은 꼭 달도록 한다.
  * 재사용 가능한 함수 또는 메서드 안에서 반복되는 코드들은 리팩토링을 해둔다.
  * 함수와 메서드는 가능한 한 작은 크기를 유지한다. 어림잡아 스크롤없이 읽을 수 있는 길이가 적합하다.

* PEP8를 따른다.
* 임포트 \(import\)
  * 임포트 순서
    * 일반적인 경우
      * 표준 라이브러리
      * 연관 외부 라이브러리
      * 로컬 애플리케이션 또는 라이브러리에 한정된 임포트

    * Django
      * 표준 라이브러리
      * 코어 장고 임포트
      * 장고와 무관한 외부 앱 임포트
      * 프로젝트 앱 임포트


  * 임포트 방식
    * 절대 임포트 : from app.module import something
    * 명시적 상대 : from .module import something
    * 암묵적 상대 : from module import something
    * 가급적 명시적 상대 임포트를 사용

  * Never use `import *`

* 장고 코드 스타일
  * URL 패턴 이름엔 dash 대신 underscore
  * 템플릿 블록 이름에 dash 대신 underscore

* HTML, CSS, JS 스타일은 경우에 따라 적당한걸 사용한다.
* IDE나 Editor에 종속되는 코딩 스타일은 지양한다.

<br>

## 2. 최적화된 장고 환경 꾸미기

* 개발과 운영환경에서 같은 DB를 이용
    * 서로 다른 DB는 데이터와 타입 호환이 완벽하지 않음.
    * PostgreSQL 추천
* pip와 virtualenv 사용
    * pip를 이용해 장고와 의존 패키지 설치
* VCS 활용
* (선택) 개발과 운영의 환경을 동일하게 구성
    * VirtualBox, Docker, ...
    * 단점
        * 필요하지 않는 기능들까지 제공되어 복잡해짐. OS레벨에서 고려해야할 필요가 없는 경우, 안하는 편이 더 편리하다.

 




