# SQL Theory Refresher

SQL query that parses the users table
>``` shell
>SELECT * FROM users WHERE user_name='leon'
>```

SQL Query Embedded in PHP Login Source Code
>``` shell
><?php
>$uname = $_POST['uname'];
>$passwd = $_POST['password'];
>
>$sql_query = "SELECT * FROM users WHERE user_name= '$uname' AND password='$passwd'";
>$result = mysqli_query($con, $sql_query);
>?>
>```
