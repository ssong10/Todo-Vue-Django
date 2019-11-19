# Vue - DRF

## 1. 기본 설정

1. Django

   1.  가상환경 설정

      ```bash
      $ python -V
      3.7.4
      $ python -m venv venv
      $ source venv/Scripts/activate
      (venv) $
      ```

   2.  패키지 설치

      ```bash
      (venv) $ pip install django djangorestframework
      (venv) $ pip freeze > requirements.txt
      ```

   3. django 프로젝트 생성

      ```bash
      $ django-admin startproject '__프로젝트명__' .
      ```

      

2. Vue

   1. VueCLI 설치

      ```bash
      $ npm install -g @vue/cli
      ```

   2. Vue 프로젝트 생성

      ```bash
      $ vue create {프로젝트 이름}
      ```

3. 프로젝트 폴더 구조

   ```
   todo-django-vue/
   	.git/
   	todo-django/
   		venv/
   		todo_django/
   		manage.py
   	todo-vue/
   		node_modules/
   		public/
   		src/
   		package.json
   ```




## 2. DRF 모델링

## 3. Vue 모델링

### Vue-router

```bash
$ npm i vue-router
$ npm use router
> Still proceed?  y
> Use history mode for router?(Requires proper server setup for index fallback in production) y
```

## 4. Todos axios 요청

1. getTodos 메소드 정의

   ```javascript
   // Home.vue
   getTodos() {
     //axios 요청
     axios.get('http://127.0.0.1:8000/api/v1/todos/')
     .then(response => {
       this.todos = response.data
       console.log(response)
     })
     .catch(error =>{
     console.log(error)
     })
   }
   ```

2. mounted 에서 호출

   ```javascript
   // Home.vue
   mounted() {
   	this.getTodos()
   }
   ```

3. CORS 오류 발생

   * 해결하기 위해서는 django 서버에서 설정이 필요

4. `django-cors-headers` 패키지 활용

   * [GitHub 참조](https://github.com/adamchainz/django-cors-headers)

    ```bash
   $ pip install django-cors-headers
    ```
   
   * `INSTALLED_APPS` , `MIDDELWARE` 설정
   * `CORS_ORIGIN_ALLOW_ALL` : True시 모든 도메인에서 요청가능
   * `CORS_ORIGIN_WHITELIST` : 위의 옵션을 False로 하고, 화이트리스트에 직접 도메인 등록
   * 기타 옵션들도 확인 해볼 것
   
5. Vue 에서 다시 요청 보내기

## 5. TodoForm component를 통해 Todo 등록하기



## 6. 로그인 기능

> JWT (JSON Web Token) : 토큰 기반 로그인 인증

1. 클라이언트(Vue) 로그인 정보(username,password)를 서버(Django)로 전송
2. 서버는 해당 정보를 바탕으로 Token을 발급 및 암호화
3. 클라이언트는 Token을 받아서 매 요청때마다 헤더에 해당 정보를 추가해서 보냄
4. 서버에서는 매번 Token이 유효한지 확인
5. 클라이언트는 전송된 값을 디코딩하여 사용자 정보 활용

>  JWT 는 기본적으로 헤더, Payload, Verify signature 로 구성된다.
>
>  https://jwt.io 에서 직접 디코딩을 해볼 수 있다.

### 1) Django

```bash
$ pip install djangorestframework-jwt
```

> settings.py

```python
REST_FRAMEWORK = {
    # 모든 views.py : 반드시 인증되어야 한다. (IsAuthenticated)
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    # a모든 views.py : 인증을 JWT 혹은 Session 등을 통해서 인증된다.
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
    ),
}
# 토큰 유지시간을 위해 설정
import datetime
JWT_AUTH = {
    'JWT_EXPIRATION_DELTA': datetime.timedelta(days=1),
}
```

> urls.py

```python
from rest_framework_jwt.views import obtain_jwt_token
urlpatterns = [
    path('api-token-auth/',obtain_jwt_token)
]
```



### 2) Vue

1. 로그인 관련 컴포넌트 생성

2. 이벤트를 통해 axios 요청

3. token 값 저장

   1. `vue-session`

      ```bash
      $ npm i vue-session
      ```

   2. `src/main.js`

      ```javascript
      import VueSession from 'vue-session'
      
      Vue.config.productionTip = false
      Vue.use(VueSession)
      ```

   3. `vue-session` 활용하여 저장

      ```javascript
      this.$session.start()
      this.$session.set('jwt',token)
      ```

      