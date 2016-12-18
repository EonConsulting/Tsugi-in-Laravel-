# Tsugi-in-Laravel-
This just a show about how to set Tsugi in Laravel 

#Tsugi inLaravel 

This is all about how to set Tsugi in Laravel.Note that this is donw without using no "Middleware". Any amendment is threfore welcome.

### Tsugi installation by using the Tsugi Application  management console. 

-First install the tsugi management console from https://github.com/tsugiproject/tsugi and configure it and create all the database tables. This can be done on a completely outside of the web environment than your Laravel code.However, it does need access to the same database as your Laravel instance to set up keys, etc.By default, there will be "12345" secret key inserted and usable if you are in DEVELOPER mode.

### Configuring Tsugi in Your Laravel App/ Some minors chnages will be found in the PHPpackagestencil 

### Step 1 
-First, make a copy  of the ```config.php``` from Tsugi into the top level directory of your UNISA App (Laravel). Do not put this in the ```app``` folder it needs to finds the ```vedor``` folder.



###  Step 2
-Edit the file to set up the various bits. Edit the file to set up the various bits. Mostly it will be the same as your Tsugi management controller ```config.php``` but with a different ```wwwroot```.


### Step 3
Now we're gonna add the dependency to the composer.json.You must the following line. 

```
tsugi/lib": ""dev-master#60bc5574df95e7bd657c18ecce5baed08680ada5"

```
Then run following command.

``` composer update ```

Because this could cause some buggs we need to bypass the CSRF

### Step 4
### Bypassing the CSRF

-First in order to allow a POST without CSRF, you need to bypass it in LARAVE(UNISA app) go->to 
```
app/Http/Middleware/VerifyCsrfToken.php

```

-Second you need to edit the file to look similar to the following: 

```
namespace App\Http\Middleware;

use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as BaseVerifier;

class VerifyCsrfToken extends BaseVerifier
{
    /**
     * The URIs that should be excluded from CSRF verification.
     *
     * @var array
     */
    protected $except = [
        'lti', 'lti/*'
    ];
}

```

See more details abbout CSRF protection. 
https://laravel.com/docs/5.1/routing#csrf-protection

### Step 5
### We make a new controller and Add a route
Note that this can also be done by using an existing controller
Go to the 

``` routes\web.php ```  and point it to your controller by doing this. 

```
Route::get('/lti', 'ltiSample@hello');
Route::post('/lti', 'ltiSample@hello');

```
Create/Make the controller:

```
php artisan make:controller ltiSample

vi ./app/Http/Controllers/ltiSample.php

```
If Step 5 is successfully done then is time to run a Test

### Run a Test

```
phph artisan serve

```
If using  MAMP, browse through your http://localhost:8888/lti or http://localhost/ltiif using XAMPP. However you should see an errormessage since it is not launched using LTI.

If you encountered some errors
Dr chuck has the sakai-api-test for the learning tool management.
Go to the https://online.dr-chuck.com/sakai-api-test/lms.php and copy  your local URL 
```
http://localhost:8888/lti
```

As the URL, and press "Launch". If all goes well, your Tsugi Laravel application should process the POST and do the rediect and dump out the Launch data.


You can afterwards  run the autoload command. 


