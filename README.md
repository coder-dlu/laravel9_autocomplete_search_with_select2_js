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







