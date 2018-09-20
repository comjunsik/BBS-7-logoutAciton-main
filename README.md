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
