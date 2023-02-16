# prueba-2

Para empezar, crearemos una migración para nuestra tabla de tickets:

php artisan make:migration create_tickets_table --create=tickets
Y luego definimos el esquema de nuestra tabla de tickets en la migración:

public function up()
{
    Schema::create('tickets', function (Blueprint $table) {
        $table->id();
        $table->string('user');
        $table->dateTime('created_at');
        $table->dateTime('updated_at');
        $table->enum('status', ['open', 'closed']);
    });
}


Y agregamos la definición de nuestra tabla a nuestro modelo:

<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Ticket extends Model
{
    use HasFactory;

    protected $fillable = ['user', 'status'];
    
    public function scopeByUser($query, $user)
{
    return $query->where('user', $user);
}

public function scopeByStatus($query, $status)
{
    return $query->where('status', $status);
}



}

Ahora que hemos creado nuestra tabla y nuestro modelo, podemos crear nuestros controladores y definir nuestras rutas.

Primero, crearemos un controlador TicketController con los métodos necesarios para manejar todas las operaciones CRUD.

php artisan make:controller TicketController --resource

Luego, definimos nuestras rutas en nuestro archivo routes/web.php:


Route::get('/tickets', 'TicketController@index');
Route::get('/tickets/{ticket}', 'TicketController@show');
Route::post('/tickets', 'TicketController@store');
Route::put('/tickets/{ticket}', 'TicketController@update');
Route::delete('/tickets/{ticket}', 'TicketController@destroy');

Con esto, hemos definido nuestras rutas y controladores para nuestro API de tickets. Ahora, para probar nuestro API en local, necesitamos configurar nuestra base de datos MySQL y ejecutar nuestras migraciones:
// Configuración de la base de datos MySQL en el archivo .env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=tickets_db
DB_USERNAME=root
DB_PASSWORD=

// Ejecutar las migraciones
php artisan migrate
Para probar nuestro API en local, podemos utilizar herramientas como Postman o curl para realizar solicitudes HTTP a nuestras rutas definidas.
