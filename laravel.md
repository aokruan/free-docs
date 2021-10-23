### Для решения проблем с кодировкой после сериализации  
```php
json_encode($users->toArray(), JSON_UNESCAPED_UNICODE)

$currentUser = User::with('roles')->get();
dd(json_encode($currentUser, JSON_UNESCAPED_UNICODE));
$user = DB::table('students')
    ->select('id', 'fullname as name', 'age')
    ->where('id', '=', 1)
    ->get();

$courses_id = DB::table('students_courses')
    ->select('id_courses as id')
    ->where('id', '=', '1')
    ->get()
    ->map(function ($item) {
        return get_object_vars($item);
    });

$list_courses = DB::table('courses')
    ->whereIn('id', $courses_id)
    ->select('id', 'title')
    ->get();

$data = collect([$user, $list_courses]);
dd(json_encode($data, JSON_UNESCAPED_UNICODE));
```

### Создание `JSON` с именованием  
```php
$items = array('users_all' =>  DB::table('users')->get());
$items = json_encode(['users_all' =>  DB::table('users')->get()], JSON_UNESCAPED_UNICODE);
```
