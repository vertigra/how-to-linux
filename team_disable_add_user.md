# Как запретить регистрацию пользвателей в Team City 10.0.2
*OC: Debian 8 Jessie*

Чтобы запретить регистрацию новых пользователей (Register a new user account на странице авторизации TeamCity) нужно:

* Авторизоваться с правами администратора TeamCity
* Перейти на страницу Administration > Authentication
* В настройках модуля аутентификации (Credentials authentication modules > Edit) снять галку Allow user registration from the login page. 

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-98112747-1', 'auto');
  ga('send', 'pageview');

</script>