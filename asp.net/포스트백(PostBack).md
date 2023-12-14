# C#, ASP.NET POASTBACK, 포스트백이란?

이벤트를 처리하기 위하여 페이지 자체적으로 페이지를 다시 로드하여 처리하게 되는데 이걸 포스트백(PostBack)이라고 한다.
submit 같은 버튼을 통해 다시 자신에 페이지가 새로 고침이 일어나는 현상
<br>
<b>Page.IsPostBack</b> : 포스트백이 일어난 것인지 판단하는 속성, !Page.IsPostBack 이라고 묻는것은 페이지가 처음 로드 되었는지를 확인하는 것

```
[hello.aspx]

<%@ page language="C#" %>
<script runat="server">
    void Page_Load(object sender, EventArgs e)
    {
        //페이지가 처음 로드되는 경우, 이부분이 빠지면 버튼을 클릭할때
        //textbox1에는 늘 asp.net 안녕 으로 초기화 된다. 
        //if (!Page.IsPostBack)
        //{
            textbox1.Text = “asp.net 안녕";
        //}
    }
    void Button1_Click(object sender, System.EventArgs e) {
label1.Text = textbox1.Text + "님 환영<br>";
}
</script>
<html><head></head>
<body>
    <form id="form1" runat="server" mathod="post">
        <asp:Label ID="label1" runat="server">Name : </asp:Label>   
        <asp:textbox ID="textbox1" runat="server" />
        <asp:Button ID="button1" OnClick="Button1_Click" runat="server" Text="Click me!"/>
    </form>
</body>
</html>
```
<br><br>
<%@ page language=“C#” %> : Page 지시자 이며 반드시 <%@ page로 시작

```
<script runat="server“>
void Page_Load(object sender, EventArgs e)
    {
        if (!Page.IsPostBack)
        {
            textbox1.Text = "asp.net 안녕";
        }
    }
void Button1_Click(object sender, System.EventArgs e) {
		label1.Text = textBox1.Text + "님 환영<br>";	
	}
</script>
```

서버 측 스크립트이며 서버에서 처리될 스크립트인 경우 반드시 runat=“server”라고 표기 해야 한다. 이 부분의 코드는 실행 시에만 인식되는 코드이며 클라이언트 측 결과물에는 이러한 코드는 나타나지 않는다. (사용자는 볼 수 없다.)
<br><br>
```
<form runat=“server”>
```
form 태그에 runat=“server”가 있다는 것은 이 폼은  흔히 말하는 웹 폼 이라는 것이다. 웹 폼은 ASP.NET에서 제공하는 서버 컨트롤을 포함 할 수 있으며 그것들을 제어 할 수 있는 서버 측의 논리적인 구역을 의미한다. 모든 서버 컨트롤은 반드시 웹 폼 구역 안에 와야 한다.(<form.>과 </form.> 사이), 하나의 ASP.NET 페이지 안에는 하나의 웹 폼이 올 수 있다.
<br><br>
```
<asp:Label id=“label1" runat="server"> 이름: </asp:Label>
```	
Label 컨트롤은 단지 문자열의 값을 출력하는 역할을 담당하는 단순한 기능의 컨트롤 이다.이 컨트롤은 서버 컨트롤 이기에 반드시 runat=“server” 설정을 해야 한다. 그리고 프로그래밍적으로 이 컨트롤의 여러 속성에 접근 해야 하는데 반드시 고유한 ID를 가져야 한다. <asp:Label> 컨트롤 이지만 컴파일 되고 실행되어 출력 결과물이 만들어 질 때는 html의 <span> 태그로 바뀌게 된다. <span id="Label1">홍길동님 환영 합니다</span>
<br>
이러한 서버 컨트롤은 서버 측 개발자에게만 의미가 있고 클라이언트에게는 의미가 없는 컨트롤 이다. 이러한 사실에 근거하면 클라이언트는 원래의 aspx 소스를 알아 내거나 살펴 볼 수 있는 방법이 전혀 없다는 것이다. 즉 결과가 서버 컨트롤을 쓴 건지 원래 <span> 태그 였는 지는 모른다는 의미다.
<br><br>

```
<asp:Button id="Button1" OnClick="Button1_Click" runat="server" Text="확인"/>
```

다음은 버튼 컨트롤 이다. Label과 마찬가지로 runat=“server”를 지정해야 하며 id 값도 부여 해야 한다. 그리고 Text 속성의 값도 가지고 있다. 또한 OnClick 이벤트를 처리 할 Method도 정의 하고 있다. 이것이 바로 서버 컨트롤에서의 이벤트 지정 이다. 사용자의 웹브라우저에서 버튼이 눌러 졌을때 지정된 메소드 이름과 같은 메소드를 서버 측에서 찾아 그 함수를 수행 하는 것이다.(정확히 말하면 버튼을 클릭시 일단 폼을 서브밋 하고 서버 측에서 해당 버튼의 클릭 이벤트가 수행 되는 것이다.) 특히 함수 명의 대소문자 구별은 중요 하다. C# 언어는 대.소문자를 구분 하는 언어 이다.

Button 컨트롤도 aspx 상에서는 <asp:Button~> 이지만 컴파일 되어 실행되면 클라이언트에서 확인 할 때는 다음과 같이 바뀌게 된다.

```
<input  type="submit" name="Button1" value="확인" id="Button1" />
```

최초 실행 후 브라우저에서 소스보기를 하면 
```
<input type="hidden" name="__VIEWSTATE" value=“ssDwxNTgXXXEzNzFOzs+gryesRQ0yJFHTVVDiw819etd=" />
```
와 같은 hidden 값이 하나 보일 것이다.
	
이것의 이름은 _VIEWSTATE이고 알아보기 힘든 값으로 채워져 있다. 이 컨트롤은 서버측에서 필요 한 정보를 base64 방식으로 인코딩 하여 내부적으로 숨겨 두기 위하여 사용하는 컨트롤 인데 주로 컨트롤들의 상태를 유지하기 위한 방법으로 사용 된다. 이렇게 하면 POSTBACK을 하더라도 컨트롤내의 값들이 사라 지지 않고 유지 될 수 있다.

최초 상태에서 이름을 입력 하고 서브밋을 한 후 소스보기를 하면 위의 내용과 다르게 나타 날것이다. <input type="hidden" name="__VIEWSTATE" value="dXXXXXXXXXXXXXXXXXXX" />


만약 <form action=“otherPage.aspx”> 처럼 태그에 action이 있어 자신의 페이지를 가리키지 않고 다른 페이지를 가리 킨다면 버튼을 클릭시 호출되는 이벤트는 otherPage.aspx의 Button1_Click이 실행되는 것일까? 만일 otherPage.aspc에 Button1_Click 함수가 없다면? 이러한 것을 ASP.Net에서는 PostBack으로 해결 하고 있다. 다시 말해 개발자가 폼의 action을 바꾸어도 그 내용은 무시해 버리고 자기 자신 페이지로 포스트 백 하도록 하는 것이다.
