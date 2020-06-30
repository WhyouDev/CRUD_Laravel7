<h2>Learning CRUD with laravel 7</h2>



<p align="center">
<img src="https://res.cloudinary.com/dtfbvvkyp/image/upload/v1566331377/laravel-logolockup-cmyk-red.svg" width="400">
</p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/d/total.svg" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>
</p>

## Step 1 Install Laravel 7

composer create-project --prefer-dist laravel/laravel (your project name)

## Step 2: Database Configuration

-setup your database, if you don't have .env file you can simply added with copy default .env_example
 ex: cp .env_example .env 

-then setting your database in your .env file

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=here your database name(your DB name)
DB_USERNAME=<i>Your DB Username</i>
DB_PASSWORD=<i>Your DB password</i>

## Step 3: Create Migration 

Create migration for "product" table using laravel 7 php artisan command.

> php artisan make:migration create_products_table --create=products

><?php
 
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
  
class CreateProductsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->text('detail');
            $table->timestamps();
        });
    }
  
    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('products');
    }
}


>run this = php artisan migrate 

## Step 4: Add Resource Route

>routes/web.php 

Route::resource('products','ProductController');

## Step 5: Add Controller and Model

Command for add controller
>php artisan make:controller ProductController --resource --model=Product

will create 7 methods :
1)index()
2)create()
3)store()
4)show()
5)edit()
6)update()
7)destroy()

app/Http/Controllers/ProductController.php

## for index()

    public function index()
    {
        $products = Product::latest()->paginate(5);
  
        return view('products.index',compact('products'))
            ->with('i', (request()->input('page', 1) - 1) * 5);
    }

## for create()
public function create()
    {
        return view('products.create');
    }

## for store()
 public function store(Request $request)
    {
        $request->validate([
            'name' => 'required',
            'detail' => 'required',
        ]);
  
        Product::create($request->all());
   
        return redirect()->route('products.index')
                        ->with('success','Product created successfully.');
    }

## for show()
public function show(Product $product)
    {
        return view('products.show',compact('product'));
    }

## for edit()
public function edit(Product $product)
    {
        return view('products.edit',compact('product'));
    } 

## for update()
 public function update(Request $request, Product $product)
    {
        $request->validate([
            'name' => 'required',
            'detail' => 'required',
        ]);
  
        $product->update($request->all());
  
        return redirect()->route('products.index')
                        ->with('success','Product updated successfully');
    }   

## for destroy()
    public function destroy(Product $product)
    {
        $product->delete();
  
        return redirect()->route('products.index')
                        ->with('success','Product deleted successfully');
    }


> add this for model products app/Product.php

namespace App;
  
use Illuminate\Database\Eloquent\Model;
   
class Product extends Model
{
    protected $fillable = [
        'name', 'detail'
    ];
}


## Step 6: Add Blade Files

1) layout.blade.php
2) index.blade.php
3) create.blade.php
4) edit.blade.php
5) show.blade.php

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
