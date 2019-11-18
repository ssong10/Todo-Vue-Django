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

