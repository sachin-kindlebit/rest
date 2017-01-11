# PHP REST (Usefull scaffold for making php rest APIs)

[![php version](https://img.shields.io/badge/php-%3E%3D5.3-blue.svg)]()
[![dependencies](https://img.shields.io/gemnasium/mathiasbynens/he.svg)]()

Mails | Database | Validations | Cache | Helpers

##Url Structure

http://localhost/rest/web/class/method/param_1/param_2/..../param_n

## Database :fire:

[PDO Query Builder](https://github.com/izniburak/PDOx/blob/master/DOCS.md)

## Cache :bowtie:

[Simple PHP Cache](https://github.com/cosenary/Simple-PHP-Cache)

## Laravel helpers :boom:

[Laravel 5 Helpers for Non-Laravel Projects](https://github.com/rappasoft/laravel-helpers)

New helpers:

```php
//Get App name
app(); //App name is defined in config file config/app.php

//Load a view.
$user = array('name' => 'Clark Kent', 'planet' => 'Crypton');
view($viewName = 'index', $data = compact('user')); //views path => resources/views

//Activity logging. Note: Make sure the log file is writeable.
app_log($data = array('foo' => 'bar'), $logType = "INFO"); // log file path => storage/logs/requests.log

//Get storage path
storage_path();
```


## Validations :thumbsup:

[GUMP Validator](https://github.com/Wixel/GUMP)

New Validation Rules:

```php
//unique,table_name or if column name is different in db unique,table_name:col_name
$rules = array('email' => 'required|valid_email|unique,users');

//exist,table_name or exist,table_name:col_name
$rules = array('user_id' => 'required|exist,users:id');

$this->validate($_REQUEST, $rules);

//Now you can insert your data in DB

$this->db->table('users')->insert([
	'name' => $_REQUEST['name'],
	'email' => $_REQUEST['email'],
	//.......
]);
```
 
## Sending Mails :email:

```php
use App\Artisan\Mail;

$user = $this->db->table('users')->where('id',1)->get();

//templates are in resources/email folder

$mail = Mail::send('template_name', compact('user'))
			->to('example@gmail.com', 'John Doe')
			->subject('This is a test subject')
			->deliver();
```

## Sending Push Notifications :bell:

```php
use App\Artisan\Notification;

$user = $this->db->table('users')->where('id',1)->get();

$payload = [
	'message' => 'Test notification.',
	'badge' => 2
];

Notification::send($user, $payload);
```

## Asynchronous HTTP requests :rocket:

```php
$users = $this->db->table('users')->where('user_type', 1)->getAll();

event('sendNewOfferEmail', $users);

//Events will be defined in app/Events.php class

//example:

class Events {

	public function sendNewOfferEmail(){
		$users = json_decode($_POST['data'], true);
		//loop through all $users and send emails
	}
}

//or

$data = json_encode(array('name' => 'Jagroop Singh'));
exec('curl http://localhost/dev/rest/web/events/methodName -d data=' . $data . ' > /dev/null &');
```

[PHP exec](http://php.net/manual/en/function.exec.php)
