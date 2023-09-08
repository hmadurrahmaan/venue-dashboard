## Laravel 10 + Inertia.js + Vue 3

## Installation

To get started with the installation, follow these steps:

1. Clone the repository

2. Enter to the project root

3. Install/update composer
``composer install | composer update``

4. Install npm
``npm install``

5. Set up the environment variables
``cp .env.example .env``

6. Set up the email configurations in .env

7. Generate an application key
``php artisan key:generate``

8. Configure the database
``php artisan migrate``

9. Configure the database
   ``php artisan db:seed``

10. Start the development server
``npm run dev``

11. Start the development server
   ``php artisan serve``

11. Setup the queues in .env
    ``QUEUE_CONNECTION=database``

12. Start the queue jobs
   ``php artisan queue:work``

13. Visit the application at in the browser.

## Test Cases

To get started with the test cases, follow these steps:

1. Set up the environment variables
``cp .env.example .env.testing``

2. Create new database for testing and mention in .env.testing

3. Configure the database
``php artisan migrate --env=testing``

4. Start the tests
   ``php artisan test``

