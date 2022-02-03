# project_pknu [홈페이지명]
- 농산물 홈쇼핑 페이지
- 시간이 된다면, 공동구매 메신져 서비스/경매서비스도 제공!   
- with 한호님   
# 🔥 1st day(2022-01-28)
### 📜 today's task
- page 구조 ✅ 
- db 설계 ✅
### 🐬result
- db 설계를 해보긴 했으나 잘 된건지 확신할 수 없다...💦
- 더 추가할 사항이 있어 보인다.   
--> 문의사항<페이지 내에 추가 / 따로 만들기>,   
--> 카테고리를 나눠 주지 않아도 되는가..? (db의 상품 테이블에 카테고리를 추가해 주는 게 좋을 것 같아 보인다..!)

### 💠페이지 구조
<span style="color:yellow">페이지구조</span>  
- 크게 소비자가 접근할 수 있는 페이지와 판매자가 접근할 수 있는 페이지, 공동으로 접근 가능한 페이지로 나눈다.   
- 틀만 잡아둔 것이기 때문에 후에 필요하다면 더 추가될 수 있다.   
- 회원가입은 소비자/판매자를 나눈다.   

##### [페이지 구성도]
![페이지 구성도](https://user-images.githubusercontent.com/72913881/151560438-69c04b9f-a0d3-4a0b-a68a-d55c2a211879.png)   
+) 공지사항   

### 📦 Database 
- 테이블은 업무논리를 적용해 설계한다.   
#### 🧠 [업무논리]
- 판매자가 **회원 등록**을 하면, 판매자 회원 정보를 **'판매자 승인'** 테이블로 옮긴다.     
  -> 승인이 **완료**되면, **판매자 테이블로** 정보를 옮겨준다.   
  -> 승인이 **거부** 되어도 정보를 **남겨 둔다**. => 차후 해당 판매자의 로그인을 방지하기 위함.(혹은 회원가입 방지)   
  -> 승인 여부는 1 또는 2와 같이 **숫자**로 표시해 둔다.   
  
- 주문 테이블에 담긴 **정보**는 **주문이 끝난 후**(배송이 마무리된 후) 정보를 **삭제**한다.   
  -> 구매확정 개념!(일주일간 기간을 두었다가, 정보 삭제)   
- 상품에 대한 **세부설명**은 **사진**이 들어가도록 한다. -> 사진 테이블은 해당 상품의 상품아이디를 FK로 참조한다.

##### [테이블]
![KakaoTalk_20220128_162613905](https://user-images.githubusercontent.com/72913881/151563855-56589256-ddc5-4d9f-9eac-3bb1bd7fdffa.png)

##### [E-R 다이어그램](속성 표시X)
![e-r](https://user-images.githubusercontent.com/72913881/151571607-e4064432-7d73-44e3-affe-43ff57d20272.png)
----
# 🔥 2st day(2022-01-30)<혼자해보기>
### 📜 today's task
- 검색창 자동완성기능🔼

### 🐬result   
-> 예전 코드 참고 및 구글링.   
-> 다만, 검색 결과가 여러 개 있을 때, 따로따로 표시되도록 하는 데에는 실패...!

### 💬검색창 자동 완성
---
~ 2022-02-02 설 연휴
---
# 🔥 3rd day(2022-02-03)
### 📜 today's task
- 로그인 기능 ✅
- 회원가입 기능 ✅
+ 메인페이지에 사용자 아이디    
[한호님]
- 상품 페이지
- 상품

### 🐬result   
- 한글 깨짐 현상 발생 -> 한호님 덕분에 해결!(charset, pageencoding을 둘다 utf-8로 변경/Util파일 change함수 utf-8 -> 8859_1로 변경/해결된 이유는 모름..)
- 회원가입 시, db에 회원정보 저장되는 것 확인
- 로그인 시, db에서 해당 아이디에 대한 비밀번호를 잘 가져오는지 확인

### 👤회원가입 기능
![소비자reg-tile](https://user-images.githubusercontent.com/72913881/152312027-2dee816e-73b7-4e3a-9736-ea553b4c96da.PNG)
- css는 따로 추가하지 않았다.
- 회원가입이 완료되면, **로그인 화면으로 바로 이동**하도록 작성하였다.   
[동작 과정]

1) 미리 정해진 주소로 접속   
<**register**()>
- 회원가입 **페이지로 이동**시키는 메소드 
- localhost:8080/project/register.pj 접속 시, 회원가입 페이지로 이동

2) 위 주소 접속 시, **account.jsp**로 이동
- 회원가입 페이지는 구매자/판매자 분리하지 않고, **한 페이지에서 가능하도록** 한다.   
<**account.jsp**>
- 소비자 버튼 클릭 -> #customer section 활성화
- 판매자 버튼 클릭 -> #seller section 활성화
- 각 **섹션** 내 form 위치 -> customer / seller 별 다른 회원가입 Model로 이동(db가 다르기 때문!)
- form의 **submit** 클릭 시, 각 모델로 **정보 이동**

3) selaccount / cusaccount로 account.jsp에서 입력한 정보 전달   
<**cusaccount()**/**selaccount()**>
- springDao에 위치한 register함수에 정보를 담고있는 SpringVO와 cus/sel mode 전달

4) cus/sel에 따라 **preparedStatment**형태의 sql문(sql) 적용
- sql문은 **PreparedStatementSetter**(pss)를 이용해 **?** 채워넣기
- 위 sql과 pss를 이용해 **jdbcTemplate**의 **update()** 메소드로 데이터베이스에 정보 추가.

5) 위 동작이 모두 끝나면, cusaccount/selaccount 메소드에서 **login페이지로 리다이렉트**

### 🔓로그인 기능
<img width="512" alt="로그인" src="https://user-images.githubusercontent.com/72913881/152327137-d1ef35a7-a781-4a74-836c-d495e1917b6e.PNG">
- 로그인도 회원가입과 마찬가지로 한 페이지에서 **판매자, 구매자 모두 로그인 가능 ** 하도록 구현
- 로그인이 성공하면 메인view로 이동한다. 이때, 사용자id가 넘어가 화면에 표시된다.   
<img width="374" alt="메인" src="https://user-images.githubusercontent.com/72913881/152329780-94174d87-d8fd-4a53-aa17-9058e6f5de42.PNG">

[동작과정]           
1) 미리 정해진 주소로 접속            
< **login()** >          
- 로그인 **페이지로 이동**시키는 메소드             
- localhost:8080/project/login.pj 접속 시, 로그인 페이지로 이동    

2) 위 주소 접속 시, **login.jsp**로 이동
- 회원가입 페이지는 구매자/판매자 분리하지 않고, **한 페이지에서 가능하도록** 한다.
<**account.jsp**>   
- 소비자 버튼 클릭 -> #customer section 활성화
- 판매자 버튼 클릭 -> #seller section 활성화
- 각 **섹션** 내 form 위치 -> customer / seller 별 다른 로그인 Model로 이동(db가 다르기 때문!)
- form의 **submit** 클릭 시, 각 모델로 **정보 이동**

3) selaccount / cusaccount로 login.jsp에서 입력한 정보 전달      
<**cusLogin()**/**selLogin()**>   
- springDao에 위치한 login함수에 정보를 담고있는 SpringVO와 cus/sel mode 전달

4) mode와 함께 넘어온 SpringVO에 저장된 아이디에 맞는 사용자의 비밀번호를 조회한다.      
<**login()**>   
- 아이디와 그 아이디에 매핑된 비밀번호는 하나만 존재한다.   
-> jdbcTemplate의 **queryForObject**(sql, new Object[] {? 채울 대상}, 리턴 타입) 메소드를 이용한다.
- 위 쿼리문의 결과와 현재 사용자가 입력한 비밀번호가 일치하면, true 리턴

5) 위 인증 결과가 controll의 cuslogin/sellogin() 메소드로 이동
- 인증 결과 true이면, main 페이지로 이동
- 인증 결과 false이면, login페이지로 실패 코드와 함께 리다이렉트
### ➰메인 페이지에 사용자 id표시하기
- 각 Login 함수에서 session에 'userid'라는 이름으로 인증된 사용자의 id 담기   
- main.jsp에서 jstl문법(${userid})을 이용해 화면에 표시   
