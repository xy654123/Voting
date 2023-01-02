# Voting


## index 

![image](https://user-images.githubusercontent.com/96267331/210189934-3448f0e7-9681-4958-9691-fb53885d5961.png)<br>
처음실행했을때의 화면이다 홈으로를 누리면 이 페이지로 이동된다.
## 후보조회 페이지
![image](https://user-images.githubusercontent.com/96267331/210189958-447dadbd-0845-4dd1-8a97-e80909c3ff50.png)<br>
실행했을때의 화면이다 이페이지를 sql문을 이용하여 테이블을 출력해준다<br>

```sql
select tm.m_no, tm.m_name, tp.p_name, case when p_school = '1' then '고졸' when p_school = '2' then '학사' when p_school = '3' then '석사' else '박사' end as p_school , substr(tm.m_jumin,1,6)||'-'||substr(tm.m_jumin,7) as m_jumin, tm.m_city, tp.p_tel1||'-'||tp.p_tel2||'-'||tp.p_tel3 as p_tal 
from tbl_member_202005 tm, tbl_party_202005 tp 
where tm.p_code = tp.p_code
```
이 sql문은 먼저 두 테이블을 사용하기 때문에 테이블의 별명을 붙여준후 기본키를 통해 조인시켜준다. 이후 조건에 맞게 작성해주는데 이중 p_school의 있는 숫자의 따라 값이 달라지도록
case문을 사용하였고 m_jumin라는 주민등록번호를 받는 값의 값을 앞 뒤를 짤라 '-'를 중간에 추가시켜주었다 p_tel도 맞찬가지로 중간에 '-'문자를 넣어주었다.

## 투표하기 페이지
![image](https://user-images.githubusercontent.com/96267331/210190139-6777defe-249b-4b07-a0d6-d3a5100021de.png)<br>
이 페이지 에서는 빈칸의 값을 넣고 실행시켜야한다 먼저 첫번째는 주민등록번호를 넣어주고 두번째는 이름을 넣어준다 그리고 세번째는 선택할수있는 칸인데<br>
![image](https://user-images.githubusercontent.com/96267331/210190521-34ef5125-6695-4f9d-add3-1398c657f612.png)<br>
이런식으로 선택할수가 있다 그리고 마지막칸있는 라디오 버튼은 누른 값에 따라 Y N 으로 바뀌게 된다 그리고 투표하기 버튼을 누르면 테이블의 업데이트된고 다시하기를 누르면 초기화 된다.
![image](https://user-images.githubusercontent.com/96267331/210190570-59a9b34b-b205-4681-aaa6-962456edb448.png)<br>
테이블의 값이 업데이트될때는 이 jsp파일을 통해서 값이 업데이트된다 방금 전에있는 테이블에서 name값을 이용하기 값을 받아오고 sql를 통해 값을 넣어주는 방식이다.

```
  <script type="text/javascript">
	function remove() {
		alert("모든 내용을 지우고 다시 입력합니다!");
		document.vData.reset();
		document.vData.jumin.focus();
	}
	
	function checkVal(){
		
		if(!document.vData.jumin.value){
			alert("주민번호가 입력되지 않았습니다!");
			document.vData.jumin.focus();
			return false;
		}else if(!document.vData.name.value){
			alert("성명이 입력되지 않았습니다!");
			document.vData.name.focus();
			return false;			
		}else if(!document.vData.vote_num.value){
			alert("후보번호가 입력되지 않았습니다!");
			document.vData.vote_num.focus();
			return false;
		}else if(!document.vData.time.value){
			alert("투표시간이 입력되지 않았습니다!");
			document.vData.time.focus();
			return false;
		}else if(!document.vData.place.value){
			alert("투표장소가 입력되지 않았습니다!");
			document.vData.place.focus();
			return false;
		}else if(!document.vData.confirm.value){
			alert("유권자확인이 선택되지 않았습니다!");
			return false;
		}
		alert("투표하기 정보가 정상적으로 등록되었습니다!");
	}
</script>
<link rel="stylesheet" href="css/style.css">
</head>
<body>
	<jsp:include page="layout/header.jsp"></jsp:include>
	<jsp:include page="layout/nav.jsp"></jsp:include>
	<div class="section">
		<div align="center">투표하기</div>
		<div>
		<form name="vData" method="post" action="page2_p.jsp" onsubmit="return checkVal()">
			<table class="table_list">
				<tr>
					<td>주민번호</td>
					<td class="align-left"><input type="text" size="26" name="jumin"> 예: 8912121111111</td>
				</tr>
				<tr>
					<td>성명</td>
					<td class="align-left"><input type="text" size="20"  name="name"></td>
				</tr>
				<tr>
					<td>투표번호</td>
					<td class="align-left">
						<select name="vote_num">
							<option value="">후보선택</option>
							<option value="1">[1]김후보</option>
							<option value="2">[2]이후보</option>
							<option value="3">[3]박후보</option>
							<option value="4">[4]조후보</option>
							<option value="5">[5]최후보</option>
						</select>
					</td>
				</tr>
				<tr>
					<td>투표시간</td>
					<td class="align-left"><input type="text" size="20"  name="time"></td>
				</tr>
				<tr>
					<td>투표장소</td>
					<td class="align-left"><input type="text" size="20"  name="place"></td>
				</tr>
				<tr>
					<td>유권자확인</td>
					<td class="align-left">
						<input type="radio" id="confirm_y"  name="confirm" value="Y">
						<label for="confirm_y">확인</label>
						<input type="radio" id="confirm_n"  name="confirm" value="N">
						<label for="confirm_n">미확인</label>
					</td>
				</tr>
				<tr>
					<td colspan="2" align="center">
						<input type="submit" value="투표하기">
						<input type="button" value="다시하기" onclick="remove()">
					</td>
				</tr>
			</table>
		</form>
		</div>
	</div>
	<jsp:include page="layout/footer.jsp"></jsp:include>
```
<br>
이코드가 투표하기 코드이다 위에 있는 스크립트를 통새 유효성검사를 진행하게 된다 만약 값이 없으면 경고 문자와 포커스를 이동하게 해준다

![image](https://user-images.githubusercontent.com/96267331/210190745-695a4842-8b60-427d-b8ef-8f90f0745226.png)<br>
![image](https://user-images.githubusercontent.com/96267331/210190759-ed53b8f0-9e24-434e-92e2-e85d750ade42.png)

## 투표검수조회

![image](https://user-images.githubusercontent.com/96267331/210190932-6fd1fe56-5a86-4c09-bf4e-394e2a4ab696.png)<br>
```sql
select V_NAME ,'19'||SUBSTR(V_JUMIN,1,2)||'년'||SUBSTR(V_JUMIN,3,2)||'월'||SUBSTR(V_JUMIN,5,2)||'일생' AS V_BIRTH , TRUNC(MONTHS_BETWEEN(SYSDATE, TO_DATE(SUBSTR('19'||V_JUMIN,1,8), 'YYYYMMDD'))/12) AS V_AGE , CASE SUBSTR(V_JUMIN,7,1) WHEN '1' THEN '남자' WHEN '2' THEN '여자' WHEN '3' THEN '남자' WHEN '4' THEN '여자' END AS V_GENDER , V_NO , SUBSTR(V_TIME,1,2) || ':' || SUBSTR(V_TIME,3,2) AS V_TIME , CASE V_CONFIRM WHEN 'Y' THEN '확인' WHEN 'N' THEN '미확인' END AS V_CONFIRM
from TBL_VOTE_202005 
where V_AREA = '제1투표장'         
```

이페이지가 투표검수조회페이지와 sql문이다 이 페이지는 제1투표장을에서 투표한 사람이 뜨는 페이지이다 그리고 주민등록번호를 나누어 년 월 일생을 뒤에 붙여주고 만 나이를 구해주고 주민등록번호
숫자를 이용해 성별을 구해주고 투표후보의 번호를 가져와 값을 출력해준다 그리고 아까 라디오버튼을 이용해 Y N 로 나누어 확인 미확인으로 구별해주는 sql이다<br>

## 후보자 등수

![image](https://user-images.githubusercontent.com/96267331/210191302-01c7d489-c946-47bf-a4ba-fc6b32d203f1.png)<br>
이페이지는 지금까지 투표한 후보의 투표수를 볼수있다<br>
```sql
select  v_no, m_name, count(*) cnt
from TBL_VOTE_202005 vote, TBL_MEMBER_202005 mem
where vote.v_no=mem.m_no and vote.V_CONFIRM = 'Y'
group by v_no, m_name
order by cnt desc 
```
이 sql문을 통해 유권자확인이 Y인 사람들의 투표개수를 카운트하는 테이블이다.


## 결과

지금까지 만든것을 이용하면

![image](https://user-images.githubusercontent.com/96267331/210191942-7e4b0595-0645-4a78-b317-7474ad2015fc.png)<br>
투표하기 페이지에서 모든 칸을 입력하여 투표하기를 클릭해준다<br>
![image](https://user-images.githubusercontent.com/96267331/210191961-e9024866-65bd-48c8-96b8-06918b136a22.png)<br>
결과 창과 함께 테이블이 업데이트 되어준다.<br>
![image](https://user-images.githubusercontent.com/96267331/210191976-f6b2f766-25f8-4ef0-8d37-c1777a85113d.png)<br>
투표검수조회페이즈를 보면 제1투표장을 이용했기 때문에 값이 추가된것을 볼 수 있다.


