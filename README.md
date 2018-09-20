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
