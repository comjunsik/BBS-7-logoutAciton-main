# 회원세션 관리
세션관리란 현재 접속한 회원에게 할당해 주는 고유의 id. 세션으로 현재 로그인한 회원들을 구분한다.<br>

# loginAction.jsp
```jsp
if (result == 1){
		session.setAttribute("userID",user.getUserID());
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('로그인 되었습니다.')");
		script.println("location.href = 'main.jsp'");
		script.println("</script>");
	}
```
**session.setAttribute("userID",user.getUserID());** "userID" 라는 이름으로 세션 부여해주기, 세션 값으로는 해당 유저의 id넣어주기<br>

# joinAction.jsp
```jsp
UserDAO userDAO = new UserDAO();
	int result = userDAO.join(user);
	if (result == -1){
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('이미 존재하는 아이디입니다.')");
		script.println("history.back()");
		script.println("</script>");
	}
	else {
		PrintWriter script = response.getWriter();
		session.setAttribute("userID",user.getUserID());
		script.println("<script>");
		script.println("location.href = 'main.jsp'");
		script.println("</script>");
	}
	}
```
**session.setAttribute("userID",user.getUserID());** 세션을 부여한 후 main.jsp로 이동

# logoutAction.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content = "text/html; charset=UFT-8">
<title>JSP 게시판 </title>
</head>
<body>
<%
	session.invalidate();
	
%>
<script>
	location.href = 'main.jsp';
</script>

</body>
</html>
```
**session.invalidate();** logout시 자동적으로 session 빼앗기게 구성<br>
**location.href = 'main.jsp';** 그 후에 link로 main.jsp로 이동하게 한다.<br>

# loginAction.jsp
현재 로그인된 사용자(세션이 부여된 사용자)는 회원가입과 로그인 페이지에 들어가지 못하도록 막아야 한다.
```jsp
String userID= null;
	if (session.getAttribute("userID") != null) {
		userID = (String) session.getAttribute("userID");
	}
	if(userID != null) {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('이미 로그인되었습니다.')");
		script.println("location.href = 'main.jsp'");
		script.println("</script>");
	}
```
**if (session.getAttribute("userID") != null)** 만약 session이 존재한다면,<br>
userID = (String) session.getAttribute("userID"); String으로 userID 저장<br>
**if(userID != null) {** userID(session)가 존재하면 경고창과 함께 자동으로 main.jsp로 이동하여 로그인 못하게 막기<br>
joinAction.jsp에도 동일한 코드를 상단에 집어넣어 회원가입도 막아주기.<br>

# main.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.io.PrintWriter" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content = "text/html; charset=UFT-8">
<meta name = "viewport" content = "width = device-width", initial-scale="1">
<link rel="stylesheet" href = "css/bootstrap.css">
<link rel="stylesheet" href = "css/custom.css">
<title>JSP 게시판 </title>
</head>
<body>
	<%
		String userID = null;
	if (session.getAttribute("userID") != null) {
		userID = (String) session.getAttribute("userID");
	}
	%>
<nav class="navbar-default">
		<div class = "navbar-header">
		<button type="button" class="navbar-toggle collapsed"
			data-toggle="collapse" data-target="#bs-example-navbar-collapse-1"
			aria-expanded="false">
			<span class="icon-bar"></span>
			<span class="icon-bar"></span>
			<span class="icon-bar"></span>
		</button>
		<a class = "navbar-brand" href="main.jsp">JSP웹사이트</a>
	</div>
	<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
	<ul class="nav navbar-nav">
		<li class="active"><a href ="main.jsp">Main</a></li>
		<li><a href="bbs.jsp">게시판</a></li>
	</ul>
	<%
		if(userID == null){
	%>
		<ul class="nav navbar-nav navbar-right">
         <li class="dropdown">
           <a href="#" class="dropdown-toggle" 
            data-toggle="dropdown" role="button" aria-haspopup="true" 
            aria-expanded="false">접속하기 <span class="caret"></span></a>
        <ul class="dropdown-menu">
              <li><a href="login.jsp">로그인</a></li>
              <li><a href="join.jsp">회원가입</a></li>
            </ul>    
         </li>
       </ul>
	<%
		} else {
	%>
		<ul class="nav navbar-nav navbar-right">
         <li class="dropdown">
           <a href="#" class="dropdown-toggle" 
            data-toggle="dropdown" role="button" aria-haspopup="true" 
            aria-expanded="false">회원관리 <span class="caret"></span></a>
        <ul class="dropdown-menu">
              <li><a href="logoutAction.jsp">로그아웃</a></li>
              
            </ul>    
         </li>
       </ul>
	<% 
		}
	%>
	
	</div>
</nav>
	<div class="container">
		<div class="jumbotron">
			<div class="container">			
				<h1>웹사이트 소개</h1>
				<p>이 웹사이트는 이하생략</p>
				<p><a class="btn btn-primary btn-pull" href="#" role="button">자세히알아보기</a></p>
				<p><a class="btn btn-primary btn-pull" href="bbs.jsp" role="button">게시판 바로가기</a></p>
			</div>
		</div>
	</div>
	<div class="container">
		<div id="myCarousel" class="carousel slide" data-ride="carousel">
			<ol class="carousel-indicators">
				<li data-target="#myCarousel" data-slide-to="0" class="active"></li>
				<li data-target="#myCarousel" data-slide-to="1"></li>
				<li data-target="#myCarousel" data-slide-to="2"></li>
			</ol>
			<div class="carousle-inner">
				<div class="item active">
					<img src="images/1.jpeg">
				</div>
				<div class="item">
					<img src="images/2.jpg">
				</div>
				<div class="item">
					<img src="images/3.jpg">
				</div>
			</div>
			<a class="left carousel-control" href="#myCarousel" data-slide="prev">
				<span class="glyphicon glyphicon-chhevron-left"></span>
			</a>
			<a class="right carousel-control" href="#myCarousel" data-slide="next">
				<span class="glyphicon glyphicon-chhevron-right"></span>
			</a>
		</div>
	</div>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
	<script src="js/bootstrap.min.js"></script> 
</body>
</html>
```
**page import="java.io.PrintWriter"** 스크립트 문장을 수행하기 위해 import 해주기<br>
```jsp
<%
		String userID = null;
	if (session.getAttribute("userID") != null) {
		userID = (String) session.getAttribute("userID");
	}
	%>
```
사용자의 id를 String으로 변환하여 저장<br>

```jsp
<ul class="nav navbar-nav">
		<li class="active"><a href ="main.jsp">Main</a></li>
		<li><a href="bbs.jsp">게시판</a></li>
	</ul>
```
네비게이션 바에서 Main이라고 되어 있는 항목을 현재 들어가 있는 페이지라고 표시해주기 위해 class="active" 넣어줌.<br>

```jsp
<%
		if(userID == null){
	%>
		<ul class="nav navbar-nav navbar-right">
         <li class="dropdown">
           <a href="#" class="dropdown-toggle" 
            data-toggle="dropdown" role="button" aria-haspopup="true" 
            aria-expanded="false">접속하기 <span class="caret"></span></a>
        <ul class="dropdown-menu">
              <li><a href="login.jsp">로그인</a></li>
              <li><a href="join.jsp">회원가입</a></li>
            </ul>    
         </li>
       </ul>
	<%
		} else {
	%>
		<ul class="nav navbar-nav navbar-right">
         <li class="dropdown">
           <a href="#" class="dropdown-toggle" 
            data-toggle="dropdown" role="button" aria-haspopup="true" 
            aria-expanded="false">회원관리 <span class="caret"></span></a>
        <ul class="dropdown-menu">
              <li><a href="logoutAction.jsp">로그아웃</a></li>
              
            </ul>    
         </li>
       </ul>
	<% 
		}
	%>
```
**if(userID==null) {... }** 안에 로그인, 회원가입 네비게이션 항목을 넣어 줌으로써, 로그인 하지 않았을때만 표시되게 수정<br>
**else{...}** 로 로그인이 되어있지 않은 상태에서는 회원관리, 로그아웃 네비게이션바 

