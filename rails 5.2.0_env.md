### Rails 5.2.0 환경변수

--------

rails 5.2.0에서부터는 프로젝트 구조가 조금 변경이 된다. 가장 많이 변경된 부분은 rails 프로젝트를 배포할 때, rails secret 부분이 차이가 난다.



기존의 rails 프로젝트에서는 Bundler 설치하고 실행하여, 앱 의존성`dependency`을 모두 설치한 후,

```Shell
gem install bundler
bundle install
```

 `config/secrets.yml`

```Shell
rake secret
698b2bfc9e8e1...    # 이 줄을 복사
```

생성된 시크릿 키를 복사하여 `config/secrets.yml`의 `{{ ENV["SECRET_KEY_BASE"] }}`부분을 지우고 붙여넣는다.

또한, 기존 환경 변수들을 관리하기 위해서, figaro gem을 사용하던지 아니면, ngnix에 환경 변수로 저장을 했었다.



rails 5.2.0에서는 모든 환경 변수 값들을 `config/credential.yml.enc`에 저장하면 된다. 해당 내용은 `config/master.key`에 의해서 암호화 된 값들이 저장된다. 

`config/master.key`는 .gitignore에 추가하여 github에 올리지 않는 것을 권장한다. 



##### 환경변수 수정하기

환경변수를 수정하기 위해서는 `bin/rails credentials:edit`를 이용해서 수정을 하면 된다.

만약 기본 editor가 지정이 되어 있지 않으면, `EDITOR=vi bin/rails credentials:edit`를 이용해서 기본 vi 에디터로 수정이 가능하다.



프로덕션 환경에서 환경변수 값들을 읽어가고 싶은 경우, `config/enviroments/production.rb`에 

~~~ruby
config.require_master_key = true
~~~

로 바꿔주면 된다.



##### 환경변수 읽어오기

credentials에 있는 변수 내용들이 다음과 같다면 

~~~ruby
production:
  aws:
    access_key_id: 123
    secret_access_key: 345
~~~

접근 할 때에는, 

 `Rails.application.credentials.production[:aws][:access_key_id]` `Rails.application.credentials.production[:aws][:secret_access_key]`  쓰면 접근이 가능하다.



[참조]

https://www.engineyard.com/blog/rails-encrypted-credentials-on-rails-5.2