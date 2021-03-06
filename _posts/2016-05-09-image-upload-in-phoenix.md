---
layout: post
title: Image Upload in Phoenix
categories:
  - elixir
  - phoenix
  - code
  - rails
  - ruby
---

I’ve been having tons of fun lately learning [Elixir](http://elixir-lang.org/) and, being a Rails developer, it was only a matter of time before I tried out [Phoenix](http://phoenixframework.org). Phoenix is a MVC style web framework for Elixir that follows the convention over configuration style of [Rails](http://rubyonrails.org/). In this article, I will demonstrate a simple example of how to handle image upload, storage, and association in Phoenix.

## Arc
For the actual multipart file upload portion of this example, we are going to take advantage of the [file upload functionality built into Phoenx](http://www.phoenixframework.org/docs/file-uploads), but that will get us only so far. We have to deal with:

* Copying the uploaded images to some publicly accessible directory
* Storing the path to those files in our DB
* Processing or cropping thumbnails with [ImageMagick](http://www.imagemagick.org/script/index.php)
* Integrating with S3

That DOES sound fun, but sometimes I just want to get to the point and build what I want to build!

Enter [Arc](https://github.com/stavro/arc)! Arc is an Elixir library oddly reminiscent of Ruby's [Carrierwave](https://github.com/carrierwaveuploader/carrierwave) gem that handles the majority of what we need to get this task done. Arc will deal with storing and processing our files as well as persisting the association between our model and the uploaded image.

## Building the App

For this example, we will do something simple. We will create an app that stores *Users* who have an avatar, username, and email address.

### Create the New Phoenix App

Lets use [Mix](http://elixir-lang.org/getting-started/mix-otp/introduction-to-mix.html) to create a new Phoenix application. Mix is Elixir’s build-tool that we use for creating files, compiling, testing and a variety of other tasks.

~~~
mix phoenix.new my_app
~~~

Once this command is done running, we can cd into our new app and start up the phoenix server.

~~~
cd my_app && mix phoenix.server
~~~

Everything should start right up and we can see the Phoenix welcome screen at `http://localhost:4000`.

If you run into any problems, please run through Phoenix's [Up and Running](http://www.phoenixframework.org/docs/up-and-running) guide.

### Create the User Model

Now, we will use Mix again to create a User model that has an avatar, username, and email. All these attributes will be strings, including the avatar attribute which is intended to just persist the local path to where our uploaded image is stored. Similar to Rails’ `rails g scaffold` command, `mix phoenix.gen.html` will create any Phoenix views, templates, models, controllers, and test files we need.

~~~
mix phoenix.gen.html User users avatar:string username:string email:string
~~~

Now we need to create the database and run the migration generated by the command above by using the Mix tasks `create` and `migrate` provided by our Phoenix database adapter [Ecto](https://github.com/elixir-lang/ecto). If your `mix ecto.create` command happens to fail, refer to [Phoenix's ecto.create documentation](http://www.phoenixframework.org/docs/mix-tasks#section--ecto-create) to properly configure your database.

~~~
mix ecto.create
mix ecto.migrate
~~~

Lastly, you will need to add a users resource to your routes file. This can be a little bit tricky because order does matter in this file.

Add the following line after the `get "/", PageController, :index` in your `web/routes.ex` file.

**web/routes.ex**

~~~elixir
scope "/", Roblist do
#...
  resources "/users", UserController
#...
end
~~~

Running `mix phoenix.routes` should now show you all the new routes available within your Phoenix app.

~~~
$ mix phoenix.routes
page_path  GET     /                MyApp.PageController :index
user_path  GET     /users           MyApp.UserController :index
user_path  GET     /users/:id/edit  MyApp.UserController :edit
user_path  GET     /users/new       MyApp.UserController :new
user_path  GET     /users/:id       MyApp.UserController :show
user_path  POST    /users           MyApp.UserController :create
user_path  PATCH   /users/:id       MyApp.UserController :update
           PUT     /users/:id       MyApp.UserController :update
user_path  DELETE  /users/:id       MyApp.UserController :delete
~~~

### Adding Arc
For this example, we will be using [v0.3.2 of arc_ecto](https://github.com/stavro/arc_ecto/tree/v0.3.2) which is an Elixir library that, as its name implies, provides integration with Ecto. The latest version of arc_ecto (as of 10 May 2016) is v0.4.1, but that version uses Ecto v2, which is not the version of Ecto that Phoenix (currently) ships with. There is no reason why you cannot upgrade to Ecto v2, but that is outside the scope of this blog post. The version arc_ect v0.3.2 will work just fine.

Add arc_ecto and Arc to your deps in your mix.exs file.

**mix.exs**  

~~~elixir
defp deps do
#...
  {:arc_ecto, "~> 0.3.1"},
  {:arc, "0.2.0"},
#...
end
~~~

Then fetch your new dependencies:

~~~
mix deps.get
~~~

### Creating an Uploader

Now that our dependencies have been updated, we can generate a new uploader called `Avatar`.

~~~
mix arc.g avatar
~~~

This will create an Elixir module that can be found at `web/uploaders/avatar.ex`. This file will also need an additional Elixir using macro `Arc.Ecto.Definition` added to it so we can use arc_ecto.

~~~elixir
defmodule MyApp.Avatar do
  use Arc.Definition
  use Arc.Ecto.Definition

  # ...
end
~~~

Now, for the final step, we need to connect our User module and Avatar uploader together. Let’s add a `Arc.Ecto.Model` using statement to the top of our User model’s code and change the type of our `:avatar` field to `MyApp.Avatar.Type` in the model’s schema. We also need to make some adjustments to our `changeset` function to properly handle uploaded files.

~~~elixir
defmodule MyApp.User do
  use MyApp.Web, :model
  use Arc.Ecto.Model

  schema "users" do
    field :avatar, MyApp.Avatar.Type
    field :username, :string
    field :email, :string

    timestamps
  end

  @required_fields ~w()
  @optional_fields ~w(username email)

  @required_file_fields ~w()
  @optional_file_fields ~w(avatar)

  @doc """
  Creates a changeset based on the `model` and `params`.

  If no params are provided, an invalid changeset is returned
  with no validation performed.
  """
  def changeset(model, params \\ :empty) do
    model
    |> cast(params, @required_fields, @optional_fields)
    |> cast_attachments(params, @required_file_fields, @optional_file_fields)
  end
end
~~~

### Updating Your Controller

Now for our controller. No need for changes here! We can save our attachments like we usually do in our controller.

**web/controllers/user_controller.ex**

~~~elixir
#...
def create(conn, %{"user" => user_params}) do
  changeset = User.changeset(%User{}, user_params)

  case Repo.insert(changeset) do
    {:ok, _user} ->
      conn
      |> put_flash(:info, "User created successfully.")
      |> redirect(to: user_path(conn, :index))
    {:error, changeset} ->
      render(conn, "new.html", changeset: changeset)
  end
end
#...
~~~

### Updating Your User Form

We will need to modify our user form to support multipart uploads by adding `[multipart: true]` to the `form_for` function at the top of our `user/form.html.eex` file. We will also be replacing `text_input` with `file_input` for our `:avatar` field.

**web/templates/user/form.html.eex**

~~~erb
<%= form_for @changeset, @action, [multipart: true], fn f -> %>
  <%= if @changeset.action do %>
    <div class="alert alert-danger">
      <p>Oops, something went wrong! Please check the errors below.</p>
    </div>
  <% end %>

  <div class="form-group">
    <%= label f, :avatar, class: "control-label" %>
    <%= file_input f, :avatar, class: "form-control" %>
    <%= error_tag f, :avatar %>
  </div>

  <div class="form-group">
    <%= label f, :username, class: "control-label" %>
    <%= text_input f, :username, class: "form-control" %>
    <%= error_tag f, :username %>
  </div>

  <div class="form-group">
    <%= label f, :email, class: "control-label" %>
    <%= text_input f, :email, class: "form-control" %>
    <%= error_tag f, :email %>
  </div>

  <div class="form-group">
    <%= submit "Submit", class: "btn btn-primary" %>
  </div>
<% end %>
~~~

### Obtaining the URLs for an Uploaded Image

This is one of the primary reasons why I chose to use Arc. Arc provides us with URL helpers for obtaining serialized URLs to our images. These URLs link directly to our locally-stored images and even include timestamps for cache-busting purposes. This is all provided by Arc and are called on our `Avatar` uploader directly.

**Example from the [Arc_Ecto Source Repo](https://github.com/stavro/arc_ecto/tree/v0.3.2#retrieve-the-serialized-url):**

~~~elixir
user = Repo.get(User, 1)

# To receive a single rendition:
MyApp.Avatar.url({user.avatar, user}, :thumb)
  #=> "https://bucket.s3.amazonaws.com/uploads/avatars/1/thumb.png?v=63601457477"

# To receive all renditions:
MyApp.Avatar.urls({user.avatar, user})
  #=> %{original: "https://.../original.png?v=1234", thumb: "https://.../thumb.png?v=1234"}

# To receive a signed url:
MyApp.Avatar.url({user.avatar, user}, signed: true)
MyApp.Avatar.url({user.avatar, user}, :thumb, signed: true)
~~~

Now, let’s use these URL helpers to display our images in our `index` and `show` templates.

**web/templates/user/index.html.eex**

~~~erb
<!-- ... -->
<%= for user <- @users do %>
    <tr>
      <td><img src="<%= MyApp.Avatar.url({user.avatar, user}) %>"/></td>
      <td><%= user.username %></td>
      <td><%= user.email %></td>

      <td class="text-right">
        <%= link "Show", to: user_path(@conn, :show, user), class: "btn btn-default btn-xs" %>
        <%= link "Edit", to: user_path(@conn, :edit, user), class: "btn btn-default btn-xs" %>
        <%= link "Delete", to: user_path(@conn, :delete, user), method: :delete, data: [confirm: "Are you sure?"], class: "btn btn-danger btn-xs" %>
      </td>
    </tr>
<% end %>
<!-- ... -->
~~~

**web/templates/user/show.html.eex**

~~~erb
<!-- ... -->
<li>
  <strong>Avatar:</strong>
  <img src="<%= MyApp.Avatar.url({@user.avatar, @user}) %>"/>
</li>
<!-- ... -->
~~~

However, when we visit these pages, our images are still not being served up! That is because we need to tell Phoenix to serve static assets from our newly created `uploads/` directory. Add the following line to your `lib/my_app/endpoint.ex` file.

**lib/my_app/endpoint.ex**

~~~elixir
plug Plug.Static,
  at: "/uploads", from: Path.expand('./uploads'), gzip: false
~~~

Now, restart your Phoenix server and you should be able to view any images that you upload via the user creation form at `http://localhost:4000/users/new`!

## In Summary

We created ourselves a brand new Phoenix application, generated a *User* and then leveraged Arc to handle avatar uploading and association. I would also like to take the time to connect S3 and do some image processing but I will talk about that on a late date. Granted, things are a bit more laborious to set up in Phoenix than in Rails, but I think that is only a hallmark of Rails’ maturity. All this code took me about three hours of Googling and debugging to get up and running, and I consider that a win! Now, [download Phoenix](http://www.phoenixframework.org/docs/installation) and give it a try!
