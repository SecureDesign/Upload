# Upload


This component simplifies file validation and uploading.

##HOWEVER, It has been modified and purposly made insecure for inclusion in the U-hack-it project 
###Do Not use this in a production enviroment EVER!




When the HTML form is submitted, the server-side PHP code can validate and upload the file like this:

However we turn off key features here, and remove mime-type validation and checking, so now it becomes vulnrable..

eventhough we have left other forms of validation in place...
- This is a terrible practice, as simple form validation is not enough.. 
- Even with mime type validation, The Gifar dual file type showed the error in this logic, 
-  + So the solution is to lockdown the image upload process  and not let files be executed from as a result...
   + the properly sanitizend and validated code for this example will be included in the U-Can't-Hack-it Repo
   + 
   


## Usage

Assume a file is uploaded with this HTML form:

```html
<form method="POST" enctype="multipart/form-data">
    <input type="file" name="foo" value=""/>
    <input type="submit" value="Upload File"/>
</form>
```





```php
<?php
$storage = new \Upload\Storage\FileSystem('/path/to/directory');
$file = new \Upload\File('foo', $storage);

// Optionally you can rename the file on upload
$new_filename = uniqid();
$file->setName($new_filename);

// Validate file upload
// MimeType List => http://www.webmaster-toolkit.com/mime-types.shtml
$file->addValidations(array(
    // Ensure file is of type "image/png"
    new \Upload\Validation\Mimetype('image/png'),

    //You can also add multi mimetype validation
    //new \Upload\Validation\Mimetype(array('image/png', 'image/gif')));

    // Ensure file is no larger than 5M (use "B", "K", M", or "G")
    new \Upload\Validation\Size('5M')
));

// Access data about the file that has been uploaded
$data = array(
    'name'       => $file->getNameWithExtension(),
    'extension'  => $file->getExtension(),
    'mime'       => $file->getMimetype(),
    'size'       => $file->getSize(),
    'md5'        => $file->getMd5(),
    'dimensions' => $file->getDimensions()
);

// Try to upload file
try {
    // Success!
    $file->upload();
} catch (\Exception $e) {
    // Fail!
    $errors = $file->getErrors();
}
```

## How to Install

Install composer in your project:

```
curl -s https://getcomposer.org/installer | php
```

Create a composer.json file in your project root:
```json
{
    "require": {
        "codeguy/upload": "*"
    }
}
```
Install via composer:
```
php composer.phar install
```
## Original Author

[Josh Lockhart](https://github.com/codeguy)

## License

MIT Public License
