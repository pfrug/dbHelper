# DbHelper
The method **sqlfromBindings** convine Builder functions toSql() and getBindings() to obtain as a result the SQL that is going to be executed.

## Usage

``` php
		
  $query = Festivity::where('title', 'like', '%test%')
            ->where('user_id', Auth::user()->id )
            ->where('category_id', 5)
            ->where('country_id', 2)
            ->where('date_from', '>' , \DB::raw('now()'))
            ->orderBy('created_at', 'DESC');

  dbHelper::sqlfromBindings($query); 
  /*
  output:
  select *
  from `festivity`
  where `title` like '%test%' and `user_id` = 1
      and `category_id` = 5
      and `country_id` = 2
      and `date_from` > now()
  order by `created_at` desc, `created_at` desc
  */

```


:warning: **The query is only for the example. Please don't write queries like this in your controller, Use Scopes**:

The same query but using scopes:

``` php
  $query = Festivity::search('test')
              ->byAuthUser()
              ->byCategory($categoryIid)
              ->byCountry($countryId)
              ->future();
              ->latest();

```

Other example

``` php
  $query = \DB::table('users')
            ->join('contacts', function ($join) {
                $join->on(function($query){
                    $query->on('users.id', '=', 'contacts.user_id')
                    ->orOn("contacts.phone",'users.phone');
                });
            })
            ->join('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.*', 'contacts.phone', 'orders.price');

  dbHelper::sqlfromBindings($query);

  /*
  output: (without format)
  select `users`.*, `contacts`.`phone`, `orders`.`price` 
  from `users` 
      inner join `contacts` on (`users`.`id` = `contacts`.`user_id` or `contacts`.`phone` = `users`.`phone`)
      inner join `orders` on `users`.`id` = `orders`.`user_id`
  */
```


