Routing
=======

There's lots of ways to specify routes in the routes.rb file but the simplest is using the `resources` magic word.

`resources :user` is a shorthand for all the routes for a standard user resource using CRUD actions.

    resources :users

is the equivalent of writing out

    get    '/users',          to: 'users#index', as: 'users'
    post   '/users',          to: 'users#create'
    get    '/users/new',      to: 'users#new',   as: 'new_user'
    get    '/users/:id/edit', to: 'users#index', as: 'edit_user'
    get    '/users/:id',      to: 'users#show',  as: 'user'
    put    '/users/:id',      to: 'users#update'
    patch  '/users/:id',      to: 'users#update'
    delete '/users/:id',      to: 'users#destroy'

If you've got a singular object (say, perhaps the only user anyone can see is themselves) then you can use `resource`

    resource :user

which is equivalent to

    post   '/user',      to: 'user#create'
    get    '/user/new',  to: 'user#new',   as: 'new_user'
    get    '/user/edit', to: 'user#index', as: 'edit_user'
    get    '/user',      to: 'user#show',  as: 'user'
    put    '/user',      to: 'user#update'
    patch  '/user',      to: 'user#update'
    delete '/user',      to: 'user#destroy'

You can customize the routes generated by passing only: or except: options:

    resources :users, except: [:destroy]
    resources :users, only: [:index, :show]


Path helpers aka named routes
-----------------------------

Routes that have an as: option are called named routes. (The name is what appears on the leftmost column when you run rake routes.)
If you've got any named routes in the routes (including those that come from specifying resources) then path helpers for that route will be available in the views and controllers.

    # routes.rb
    resources :users
    get '/terms', to: 'terms#show',  as: 'terms'

    # any controller, view or helper

    users_path       #=> '/users'
    new_users_path   #=> '/users/new'
    user_path(@user) #=> '/users/19'
    terms_path       #=> '/terms'

There are also *_url versions, which work identically but also have the hostname (which are useful e.g. when generating emails.)