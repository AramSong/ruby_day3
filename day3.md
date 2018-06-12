# 1. 월요일 오전 과제

```
 점심 메뉴/메뉴
-> 근처에 있는 메뉴 중에 하나를 선택해서 보여준 것
```



### /webtoon

1. rest-client, json, nokogiri
2. 다음 웹툰, 네이버 웹툰에서 웹툰 데이터를 크롤링한다.
3. 두 사이트에서 받아온 데이터 형태를 일치시킨다. 데이터는 (웹툰제목, 썸네일 이미지, 웹툰을 볼 수 있는 링크)
4. 랜덤으로 비복원 추출으르 하려면 배열 형태로 데이터를 만든다.
5. 배열 안에 있는 웹툰 1개의 데이터는 해쉬(딕셔너리)형태로 만든다.
6. 웹툰 3개를 뽑아서 <table> 태그를 이용해서 표로 보여준다.



### 추가과제

- 각 요일 별 추출하기
- 버튼에 일~토요일까지 버튼을 만들고 /webtoon?day=mon 의 형태로 접속하면 월요일 웹툰만 샘프링하도록

### /check_file

1. 데이터는 기본적으로 1번만 받아온다.

2. 만약에 데이터가 있으면, 전체 목록을 불러오는 /로 리디렉션을 해준다.

3. 만약에 데이터가 없으면, 모든 정보를 저장하는 CSV파일을 새로 만들어준다.

   ~~~
   1. 신의탑, 네이버어디, 누구, 누구, 링트, 네이버
   2. 롱리브더킹, 카카오어디,누구,누구, 링크,카카오
   ...
   ~~~

   

4. CSV파일이 새로 만들어 졌을 때

   #### 해시

   해시에서는 키 값으로 어떤 객체를 사용해도 상관없지만, 배열은 정수(integer)만 사용할 수 있다.
   해시 리터럴은 대괄호 대신 중괄호 `{ }`를 사용한다. => 기호의 왼쪽에 있는 것이 키이고, 오른쪽은 값이다.
   특정 해시 안에서 키는 유일한 값이어야 한다.
   주어진 키에 해당하는 객체가 없을 때는 기본적으로 nil을 반환한다.

   ```
   instSection = {
     ‘첼로‘ => ‘현악기‘,
     ‘클라리넷‘ => ‘관악기‘,
     ‘드럼‘  => ‘타악기‘,
     ‘오보에‘  => ‘관악기‘,
     ‘트럼펫‘  => ‘금관악기‘,
     ‘바이올린‘ => ‘현악기‘
   }
   ```

    메서드에서는 루비의 `yield`문을 이용해서 **코드 블록**을 여러 차례 실행할
   수 있다. `yield`문은 현 위치의 메서드와 결합되어 있는 코드 블록을 불러오는 메서드 호출
   로 생각해도 무방하다.

   ```
   def call_block
     puts “Start of method“
     yield
     yield
     puts “End of method“
   end
   
   call_block { puts “In the block“ }
   ```

   결과는 아래와 같다. 코드 불록 안에 있는 코드 `puts “In the blocks”`는 `yield`가 불릴 때마다 한 번씩 실행한다.

   ```
   Start of method
   In the block
   In the block
   End of method
   ```

   코드 블록을 반복자(Iterators)를 구현하기 위해 사용한다.

   > 반복자란 배열 등의 집합에서 구성요소를 하나씩 반환해주는 함수

   ```
   a = %w{개미 벌 고양이 개 엘크}  # 배열을 생성
   a.each {|animal| puts animal } # 배열의 내용을 반복
   ```

   ### CDN (Content deliver network)

   콘텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워크에 데이터를 저장하여 제공하는 시스템. CDN 이란 용어는 필요한 자바스크립트 라이브러리 또는 스타일시트 파일을 자신의 서버에 두지 않고 다른 사이트에서 가져올 때 사용한다. 

### yield

`yield` 메소드는 레이아웃에서 뷰에 삽입해야할 장소를 지정할 때 사용합니다. `yield`의 가장 단순한 사용법으로는 `yield`를 하나만 사용하고, 지정된 뷰의 컨텐츠 전체를 그 위치에 삽입하는 것입니다.

`yield`을 여러 곳에서 호출하는 레이아웃을 작성할 수도 있습니다.

뷰의 메인 부분은 언제나 '이름이 없는' `yield`에서 랜더링 됩니다. 컨텐츠를 이름이 붙어있는 `yield`로서 랜더링 하는 경우에는 `content_for` 메소드를 사용합니다.

### 와일드 카드

파일을 지정할 때, 구체적인 이름 대신에 여러 파일을 동시에 지정할 목적으로 사용하는 특수 기호. `＊', `？' 따위. 

추가과제
각 요일 별로 추출하기
버튼에 일 ~ 토요일까지 버튼을 만들고 /webtoon?day=mon 의 형태로 접속하면 월요일 웹툰 중에서만 샘플링 하도록.
views/weboon.erb

<table>
    <thead>
        <th>이미지</th>
        <th>제목</th>
        <th>링크</th>
    </thead>
    <tbody>
        <% @daum_webtoon.each do |toon| %>
        <tr>
            <td><img src="<%= toon["image"] %>"></td>
            <td><%= toon["title"] %></td>
            <td><a href="<%= toon["link"] %>">보러가기</a></td>
        </tr>
        <% end %>
    </tbody>
</table>
app.rb

require 'sinatra'
require 'sinatra/reloader'
require 'rest-client'
require 'json'

get '/webtoon' do
    # 내가 받아온 웹툰 데이터를 저장할 배열생성
    toons = []
    # 웹툰 데이터를 받아올 url파악 및 요청보내기
    url = "http://webtoon.daum.net/data/pc/webtoon/list_serialized/mon"
    result = RestClient.get(url)
    # 응답으로 온 내용을 해쉬 형태로 바꾸기
    webtoons = JSON.parse(result)
    # 해쉬에서 웹툰 리스트에 해당하는 부분 순환하기
    webtoons["data"].each do |toon|
        # 웹툰 제목
        title = toon["title"]
        # 웹툰 이미지 주소
        image = toon["thumbnailImage2"]["url"]
        # 웹툰을 볼 수 있는 주소
        link = "http://webtoon.daum.net/webtoon/view/#{toon['nickname']}"
        # 필요한 부분을 분리해서 처음만든 배열에 push
        toons << { "title" => title,
                   "image" => image,
                   "link" => link
                 }
    end
    # 완성된 배열 중에서 3개의 웹툰만 랜덤 추출
    @daum_webtoon = toons.sample(3)
    erb :webtoon
end
/check_file
데이터는 기본적으로 1번만 받아온다.
만약에 데이터가 있으면, 전체 목록을 불러오는 / 로 리디렉션 해준다.
만약에 데이터가 없으면, 모든 정보를 저장하는 CSV파일을 새로 만들어준다.
1,신의탑,네이버어디,누구,누구,링크,네이버
2,롱리브더킹,카카오어디,누구,누구,링크,카카오
...
CSV파일이 새로 만들어 졌을 경우에는 <h1>CSV 파일이 생성되었습니다.</h1>가 있는 check_file.erb 파일을 랜더링하고 기존에 있는 파일을 읽어 올 경우 각 데이터를 보여주는 <table> 이 있는 webtoons.erb 파일을 랜더링 해준다.
views/check_file.erb

<h1>CSV 파일이 생성되었습니다.</h1>
views/webtoons.erb

<table>
    <thead>
        <th>글번호</th>
        <th>이미지</th>
        <th>제목</th>
        <th>링크</th>
    </thead>
    <tbody>
        <% @webtoons.each do |toon| %>
        <tr>
            <td><%= toon[0] %></td>
            <td><img src="<%= toon[2] %>"></td>
            <td><%= toon[1] %></td>
            <td><a href="<%= toon[3] %>">보러가기</a></td>
        </tr>
        <% end %>
    </tbody>
</table>
app.rb

...
get '/check_file' do
    unless File.file?('./webtoon.csv')
        # CSV 파일을 새로 생성하는 코드
        CSV.open('./webtoon.csv', 'w+') do |row|
            # 크롤링 한 웹툰 데이터를 CSV에 삽입
            row << data
        end
        erb :check_file
    else
        # 존재하는 CSV 파일을 불러오는 코드
        @webtoons = []
        CSV.open('./webtoon.csv', 'r+').each do |row|
            @webtoons << row
        end
        erb :webtoons
    end
end
...
layout.erb
sinatra나 rails에서는 우리가 <html>, <head> 태그를 추가하지 않아도 자동적으로 해당 태그를 추가해주고 완성된 HTML 문서를 만들어준다. layout은 하나의 틀로써 sinatra나 rails에서 만든 모든 액션에 대해서 기본적인 view를 잡아준다. 이런 layout을 사용하는 이유는 많은 양의 공통적으로 등장하는 코드를 줄이기 위함인데 예를 들어, bootstrap CDN코드와 같이 매 페이지마다 들어가야할 내용을 하나의 공통으로 사용하는 파일을 만들어 중복을 줄인다.
views/layout.erb

<html>
  <head>
    <title>멋사 화이팅</title>
  </head>
  <body>
    <%= yield %>
  </body>
</html>
여기에서 처음보는 <%= yield %>라는 코드가 등장하는데 기본적으로 루비에서 yield 는 흐음의 제어를 양보하는 역할을 한다. 제어를 다른 코드로 넘겨주어 반복문 등에서 활용할 수 있다.
여기에서는 layout.erb 파일 중간에 우리가 호출한 액션에 해당하는 erb 파일이 <%= yield %> 부분에 삽입되어 응답으로 전달된다.
새로운 형태의 parameter
그동안 우리는 url에서 ? 뒤에 등장하는 내용으로 파라미터를 설정했다. 하지만 이는 REST api 규칙에 어긋나는 것이다. REST api에서는 /board?id=1 의 형태가 아닌 /board/1 의 형태를 권장하고 있다.
app.rb

...
get '/board/:id' do
	puts params[:id]
end
위와같은 형태로 액션을 만들면 /board/1에 해당하는 요청을 받을 수 있으며, 1이라는 숫자는 params에서 우리가 설정해 놓은 이름으로 불러올 수 있다.