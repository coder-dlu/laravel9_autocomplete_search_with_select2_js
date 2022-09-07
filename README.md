# laravel9_autocomplete_search_with_select2_js
## 1: Install Laravel 9
```Dockerfile
composer create-project laravel/laravel laravel9_autocomplete_search_with_select2_js
```
## 2: Add Dummy Users
```Dockerfile
php artisan migrate
```
- Chạy 
```Dockerfile
php artisan tinker
User::factory()->count(20)->create()
```
## 3: Create Controller
- Vao app/Http/Controllers/SearchController.php
```Dockerfile
<?php
  
namespace App\Http\Controllers;
 
use Illuminate\Http\Request;
use App\Models\User;
  
class SearchController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        return view('searchDemo');
    }
    
    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function autocomplete(Request $request)
    {
        $data = [];
  
        if($request->filled('q')){
            $data = User::select("name", "id")
                        ->where('name', 'LIKE', '%'. $request->get('q'). '%')
                        ->get();
        }
    
        return response()->json($data);
    }
}
```
## 4: Create Routes
- routes/web.php
```Dockerfile
<?php
  
use Illuminate\Support\Facades\Route;
  
use App\Http\Controllers\SearchController;
  
/* 
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/
  
Route::controller(SearchController::class)->group(function(){
    Route::get('demo-search', 'index');
    Route::get('autocomplete', 'autocomplete')->name('autocomplete');
});
```
## 5: Create View File
- resources/views/searchDemo.blade.php
```Dockerfile
<!DOCTYPE html>
<html>
<head>
    <title>Laravel 9 Select2 JS Autocomplete Search Example - ItSolutionStuff.com</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.13/css/select2.min.css"/>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.13/js/select2.min.js"></script>
</head>
<body>
     
<div class="container">
    <h1>Laravel 9 Select2 JS Autocomplete Search Example - ItSolutionStuff.com</h1>   
    <select class="form-control" id="search" style="width:500px;" name="user_id"></select>
</div>
     
<script type="text/javascript">
    var path = "{{ route('autocomplete') }}";
  
    $('#search').select2({
        placeholder: 'Select an user',
        ajax: {
          url: path,
          dataType: 'json',
          delay: 250,
          processResults: function (data) {
            return {
              results:  $.map(data, function (item) {
                    return {
                        text: item.name,
                        id: item.id
                    }
                })
            };
          },
          cache: true
        }
      });
  
</script>
     
</body>
</html>
```
## 6: Run Laravel App:
```Dockerfile
php artisan serve
```
- Vào http://localhost:8000/demo-search

![Container](img1.png)
