# Viewstate란?

페이지 자체를 포함하여 Web Forms 페이지의 각 컨트롤에는 기본 Control 컨트롤에서 상속된 ViewState 속성이 있습니다.
뷰 상태는 ASP.NET 페이지 프레임워크에서 페이지를 렌더링하기 직전에 페이지와 각 컨트롤 값을 자동으로 저장할 때 사용됩니다. 
페이지가 게시될 때 페이지 처리에서 처음 수행되는 작업이 뷰 상태 복원입니다.

### 1. 고려사항

뷰스테이터스를 사용하기위해서는 몇가지 알아야 할 사항이 있습니다.
<br>
<br>저장가능한 데이터 형태는 아래와 같습니다.
- 문자열
- 정수
- 부울 값
- Array 개체
- ArrayList 개체
- 해시 테이블
<br>base64 인코딩됩니다.

<br>뷰스테이터스에 의해 페이지 로드가 느려질수 있으므로 테스트를 해봐야 합니다.
<br>(참고 : MSDN - 상태 관리, ViewStateMode 열거형 )
<br>
<br>용량문제가 발생할수 있는 소지가 있으므로 모바일기기에서는 재대로 동작하지 못할수 있습니다.

### 2. 데이터 넣기

데이터를 넣는 방법은 세션과 비슷합니다.
<br>단지 'ViewState'를 사용한다는 것을 빼면 말이죠.
<br>새로운 페이지를 열고 같이 UI를 만들어 봅시다.
<br>
![image](https://github.com/gami03/TIL/assets/128332485/a853d9dc-1e17-4731-9c23-502a03286f00)
<br>
GridView1
<br>Label1
<br>Button1
<br>Button3
<br>Button2
<br>
<br>클래스 전역변수에 다음과 같이 선언 합니다.
```
string m_sTemp = "초기값";
```
<br>
'Button3'의 비하인드 코드에 다음과 같이 작성합니다.
<br>

```
protected void Button3_Click(object sender, EventArgs e)
{
	//뷰상태에 내용 넣기
	ArrayList result = new ArrayList(4);
 
	result.Add("item 1");
	result.Add("item 2");
	result.Add("item 3");
	result.Add("item 4");
 
	ViewState.Add("ListData", result);
 
	m_sTemp = "후속값";
}
```
<br>
ViewState.Add("ListData", result);
<br>이부분이 뷰스테이트에 값을 넣는 부분입니다.
<br>
<br>매개변수는
<br>넣을 뷰스테이터스이름, 넣을 값

### 3. 값 읽기

'Button1'의 비하인드 코드를 다음과 같이 작성합니다.

```
protected void Button1_Click(object sender, EventArgs e)
{
	//뷰상태 보기
	GridView1.DataSource = (ArrayList)ViewState["ListData"];
	GridView1.DataBind();
 
	Label1.Text = this.m_sTemp;
}
```
값을 읽어오는 것은 세션과 비슷합니다.
<br>'ViewState'자체가 배열처럼 동작하죠.

### 4. 값 지우기

일반적인 배열처럼 동작하기 때문에 모두지우기(Clear)와 하나만 지우기(Remove) 둘다 사용이 가능합니다.

```
protected void Button2_Click(object sender, EventArgs e)
{
	//뷰상태 지우기
	//ViewState.Clear();
	ViewState.Remove("ListData");
}
```

### 5. 테스트

이제 테스트해봅시다.
<br>
뷰스테이터스에 값을 넣고 뷰상태를 확인하면 데이터가 들어가있는 것을 확인 할수 있습니다.
<br>
![image](https://github.com/gami03/TIL/assets/128332485/46b25a77-b5ad-41cd-ade5-c884ea57e75f)
<br>
결과를 보시면 아시겠지만 m_sTemp의 값은 항상 '초기값'인것을 확인 할수 있습니다.
<br>
이것은 'ASP.NET'의 동작방식때문에 페이지가 리플래시(버튼을 누르거나 새로고침을 하는 경우 등등)되면 변수값이 유지 되지 않기 때문입니다.
<br>
<br>뷰스테이터스에 값을 넣고 뷰상태를 확인하면 데이터가 들어가있는 것을 확인 할수 있습니다.

<br>
<br> 객체(object)는 저장이 되지 않는 다는 단점이 있습니다.
