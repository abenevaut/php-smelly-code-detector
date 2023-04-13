<p align="center">
<img src="https://github.com/DeGraciaMathieu/php-smelly-code-detector/blob/master/arts/robot.png" width="250">
</p>

[![testing](https://github.com/DeGraciaMathieu/php-smelly-code-detector/actions/workflows/testing.yml/badge.svg)](https://github.com/DeGraciaMathieu/php-smelly-code-detector/actions/workflows/testing.yml)
[![phpstan](https://github.com/DeGraciaMathieu/php-smelly-code-detector/actions/workflows/phpstan.yml/badge.svg)](https://github.com/DeGraciaMathieu/php-smelly-code-detector/actions/workflows/phpstan.yml)
![Packagist Version](https://img.shields.io/packagist/v/degraciamathieu/php-smelly-code-detector)
![Packagist PHP Version](https://img.shields.io/packagist/dependency-v/degraciamathieu/php-smelly-code-detector/php)

# php-smelly-code-detector

> "A code smell is a surface indication that usually corresponds to a deeper problem in the system."
> ~ Martin Fowler

Code smell is a potentially problematic code indicator with the following formula : `($ccn + $arg) * $loc`

- ccn : [cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity) of the method
- arg : number of method arguments
- loc : number of lines in the method

A high smell value will often reveal that the method is too complex.

This complexity could be detrimental to the maintainability of the method, favoring the appearance of bugs in the future.

This indicator does not replace the expertise of a developer and must above all be considered as an alarm detecting "smelly" code.

# Installation

```
Requires >= PHP 8.1
```

## Phar
This tool is distributed as a [PHP Archive (PHAR)](https://www.php.net/phar):

```
wget https://github.com/DeGraciaMathieu/php-smelly-code-detector/raw/master/builds/smelly-code-detector
```

```
php smelly-code-detector --version
```

## Composer
Alternately, you can directly use composer :

```
composer require degraciamathieu/php-smelly-code-detector --dev
```

# Usage
```
vendor/bin/smellyphpcodedetector inspect {folder}
```
## Options
| options               | description |
|-----------------------|-------------|
| --min-smell= | The minimum smell threshold to show.|
| --max-smell= | The maximum smell threshold to show.|
| --only= | Comma-separated list of smells to show.|
| --ignore= | Comma-separated list of smells to ignore.|
| --limit= | The maximum number of results to show, default 20.|
| --public | Show only public methods.|
| --private | Show only private methods.|
| --protected | Show only protected methods.|
| --without-constructor | Hide constructors.|
| --sort-by=smell | Sort order (smell, loc, arg, ccl), default smell.|


## Examples
```
$ php smelly-code-detector inspect app --limit=5
❀ PHP Smelly Code Detector ❀
   84 [============================]
+-----------------------------------------------------+--------+------------+-----+-----+-----+-------+
| Class                                               | method | visibility | loc | arg | ccl | Smell |
+-----------------------------------------------------+--------+------------+-----+-----+-----+-------+
| app/Http/Controllers/Blog/AdminPostController.php   | update | Public     | 25  | 2   | 3   | 125   |
| app/Http/Controllers/Auth/NewPasswordController.php | store  | Public     | 29  | 1   | 2   | 87    |
| app/Http/Controllers/Blog/AdminPostController.php   | store  | Public     | 26  | 1   | 2   | 78    |
| app/Console/Commands/FetchGoogleFonts.php           | store  | Private    | 26  | 1   | 2   | 78    |
| app/Http/Middleware/RedirectIfAuthenticated.php     | handle | Public     | 11  | 3   | 4   | 77    |
| 5/183 rows displayed                                                                                |
+-----------------------------------------------------+--------+------------+-----+-----+-----+-------+
```
```
$ php smelly-code-detector inspect app --only=Controller.php --limit=10
❀ PHP Smelly Code Detector ❀
   24 [============================]
+-----------------------------------------------------------+--------------------+------------+-----+-----+-----+-------+
| Class                                                     | method             | visibility | loc | arg | ccl | Smell |
+-----------------------------------------------------------+--------------------+------------+-----+-----+-----+-------+
| app/Http/Controllers/Blog/AdminPostController.php         | update             | Public     | 25  | 2   | 3   | 125   |
| app/Http/Controllers/Auth/NewPasswordController.php       | store              | Public     | 29  | 1   | 2   | 87    |
| app/Http/Controllers/Blog/AdminPostController.php         | store              | Public     | 26  | 1   | 2   | 78    |
| app/Http/Controllers/User/ProfileController.php           | updateAvatar       | Public     | 21  | 1   | 2   | 63    |
| app/Http/Controllers/Blog/GalleryController.php           | uploadPicture      | Public     | 21  | 1   | 2   | 63    |
| app/Http/Controllers/Auth/PasswordResetLinkController.php | store              | Public     | 17  | 1   | 2   | 51    |
| app/Http/Controllers/User/ProfileController.php           | updateInformations | Public     | 17  | 1   | 2   | 51    |
| app/Http/Controllers/Auth/RegisteredUserController.php    | store              | Public     | 25  | 1   | 1   | 50    |
| app/Http/Controllers/Blog/ShowPostController.php          | __invoke           | Public     | 12  | 2   | 2   | 48    |
| app/Http/Controllers/Forum/CommentController.php          | store              | Public     | 16  | 2   | 1   | 48    |
| 10/50 rows displayed                                                                                                  |
+-----------------------------------------------------------+--------------------+------------+-----+-----+-----+-------+
```
```
$ php smelly-code-detector inspect app --private
❀ PHP Smelly Code Detector ❀
   84 [============================]
+--------------------------------------------------+---------------------------------+------------+-----+-----+-----+-------+
| Class                                            | method                          | visibility | loc | arg | ccl | Smell |
+--------------------------------------------------+---------------------------------+------------+-----+-----+-----+-------+
| app/Console/Commands/FetchGoogleFonts.php        | store                           | Private    | 26  | 1   | 2   | 78    |
| app/Services/Community/CreatorRepository.php     | instantiateCreatorsFromResponse | Private    | 24  | 1   | 2   | 72    |
| app/Notifications/VerifyEmail.php                | verificationUrl                 | Private    | 10  | 1   | 1   | 20    |
| app/Http/Controllers/Blog/ShowPostController.php | postAreNotDisplayable           | Private    | 3   | 2   | 2   | 12    |
| 4/4 rows displayed                                                                                                        |
+--------------------------------------------------+---------------------------------+------------+-----+-----+-----+-------+
```
```
$ php smelly-code-detector inspect app --sort-by=ccl --limit=3
❀ PHP Smelly Code Detector ❀
   84 [============================]
+---------------------------------------------------+-----------------------+------------+-----+-----+-----+-------+
| Class                                             | method                | visibility | loc | arg | ccl | Smell |
+---------------------------------------------------+-----------------------+------------+-----+-----+-----+-------+
| app/Http/Middleware/RedirectIfAuthenticated.php   | handle                | Public     | 11  | 3   | 4   | 77    |
| app/Http/Controllers/Blog/AdminPostController.php | update                | Public     | 25  | 2   | 3   | 125   |
| app/Providers/RouteServiceProvider.php            | configureRateLimiting | Protected  | 5   | 0   | 2   | 10    |
| 3/183 rows displayed                                                                                             |
+---------------------------------------------------+-----------------------+------------+-----+-----+-----+-------+
```
