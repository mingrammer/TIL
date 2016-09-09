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

<br>

## 3. 장고 프로젝트 구성

```
<repository_root>/
    <django_project_root>/
        <configuration_root>/
```

* **repository_root** : README.md, docs/, .gitignore, requirements.txt, 배포 관련 파일 등등
* **django_project_root** : Django project 소스들
* **configuration_root** : settings, uris.py 등등. (유효한 파이썬 패키지)
* Django cookiecutter를 사용할 수도 있음.

<br>

## 4. 장고 앱 디자인의 기본

* 장고 앱 용어    
    * 장고 프로젝트 : 장고 웹 프레임워크를 기반으로 한 웹 애플리케이션
    * 장고 앱 : 프로젝트의 한 기능을 표현하기 위해 디자인된 작은 라이브러리
    * INSTALLED_APPS : 프로젝트에 이용하기 위해 INSTALLED_APPS 세팅에 설정한 장고 앱들
    * 서드 파티 장고 패키지 : 파이썬 패키지 도구들에 의해 패키지화된, 재사용 가능한 플러그인 형태의 장고 앱
* 각 앱은 그 앱의 주어진 임무에만 집중할 수 있어야함
* 앱 이름 정하기
    * 가능한 한 한 단어
    * 일반적으로 앱의 중심이 되는 모델의 복수 형태 (blog등은 예외)
    * URL의 주소가 어떻게 되는지도 고려
    * PEP8 규약에 맞게
* 되도록이면 앱들을 작게 유지
* 공통 앱 모듈 / 비공통 앱 모듈
* 공통 앱 모듈의 이름 규약은 가급적 따르자.

<br>

## 5. settings과 requirements 파일

* (최선의) 장고 설정 방법
    * 버전 컨트롤 시스템으로 모든 설정 파일을 관리 (운영 환경에서 중요)
    * 반복되는 설정을 없앰
    * 암호나 비밀키 등은 안전하게 보관 (버전 컨트롤 시스템에서 제외)
* 버전 관리되지 않는 로컬 세팅은 피한다.
    * 로컬 세팅 방식은 모든 곳에 버전 컨트롤되지 않은 코드가 존재하게 됨
    * 각기 다른 로컬 세팅을 가질 경우, 어느 쪽에서 문제가 생기면 재현하기도 어렵고 원인 추적도 어려워짐
    * 팀원들이 서로의 로컬 세팅을 복사하고 붙여쓰는데에 반복적인 일이 생기고 비효율적이게 됨
* 여러 개의 settings 파일 이용
    * `settings` 아래에 `base.py`, `local.py`, `staging.py`, `test.py`, `production.py`로 각기 다른 세팅 파일을 둔다. (각 세팅 모듈은 독립적인 requirements가 필요)
    * 규모에따라 추가적인 세팅 파일이 필요할 수도 있음 (ex. CI서버 환경 파일)
    * 만약 `local.py`를 이용하여 셸을 시작할 경우, `python manage.py shell --settings=twoscoops.settings.local`를 쓴다 
    * --settings를 사용하는 대신 DJANGO_SETTINGS_MODULE 환경 변수를 조건에 맞는 세팅 모듈 패스로 설정할 수도 있음
* 다중 개발 환경 세팅
    * 팀원별로 각기 다른 개발 환경 세팅 파일을 사용할 경우, `dev_<name>.py` 형식의 파일을 VCS에서 관리하며 사용할 수 있다.
* 코드에서 설정 분리
    * 비밀키들은 설정값들이지, 코드가 아니다.
    * **환경 변수 패턴**
        * bash등의 설정 파일에 설정값들을 export해놓고 코드에서 환경 변수값을 불러와 사용한다.
        * 만약 비밀키가 존재 하지 않을 경우 Django 코어 익셉션의 `ImproperlyConfigured`를 임포트하여 예외를 발생시킨다.
    * 환경 변수를 사용할 수 없을 때
        * Apache, Nginx 환경에서 환경 변수 사용을 못할 수 있다. 이 땐 **비밀 파일 패턴**을 사용한다
    * **비밀 파일 패턴**
        * JSON, Config, YAML 또는 XML중 한 가지 포맷을 선택해 비밀 파일을 생성
        * 비밀 파일을 관리하기 위한 비밀 파일 로더를 추가
        * 비밀 파일의 이름을 .gitignore 또는 .hgignore에 추가
* 여러 개의 requirements 파일 이용
    * `requirements` 아래에 `base.txt`, `local.txt`, `staging.txt`, `production.txt`로 각기 다른 requirements 파일을 둔다.
    * `base.txt`는 공통 의존 패키지
    * 나머지의 경우 상단에 `-r base.txt`로 공통 패키지를 포함하고 아래에 추가 패키지를 적는다.
* settings에서 파일 경로 처리
    * Fixes path가 아닌 Base path 기준의 Relative path를 사용하자.





