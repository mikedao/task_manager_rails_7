# Task Manager

## CRUD in Rails

What does CRUD (in the the programming world) stand for?

* C: Create
* R: Read
* U: Update
* D: Delete

The apps that you will be creating in Mod 2 will make heavy use of these four actions.

Let's use Rails to build an application where we can manage some tasks.

We're going to follow the MVC design pattern, which Rails uses by default, to implement the CRUD actions for our Task Manager app. For a quick explanation of MVC, please read through [this](https://medium.freecodecamp.org/simplified-explanation-to-mvc-5d307796df30) post.

Throughout the module, we'll talk through some conventions and best practices, but for now - we'd like for you to follow along with this tutorial. We highly recommend **not** copying and pasting the code in this tutorial. It's to your advantage to type each line of code on your own.

## Getting Configured

Before creating our new Task Manager app, let's make sure we are all on the same version of Rails.  For this tutorial, you will want to be running Rails 7.0.4.  To check which version of rails you have installed, run `$ rails -v`.  If you see any version other than 7.0.4, you will need to follow [these instructions](./rails_uninstall.md) to get the correct version installed.

After confirming that you are running the correct version of rails, we are ready to get started!

To create your rails app, navigate to your 2module directory and run the following command:

`$ rails new task_manager -T -d="postgresql"`

Let's break down this command to better understand what is happening.  `rails new` is the command to create a new Rails app - this will create *a lot* of directories and files; we will explore this file structure in a moment.  `task_manager` is going to be the name of the directory in which our Rails app lives and is, essentially, the name of our application. `-T` tells Rails that we are not going to use its default testing suite.  We will be using RSpec as our testing framework. `-d="postgresql"` tells Rails that we will be using a postgresql database; Rails can handle many different databases, but this is the one we will be using in Mod2.

Now that we have created our rails app, let's `cd task_manager` and explore some of the structure that Rails builds out for us.

## Project Folder Structure

<p align="center">
  <img src="https://i.imgur.com/28dEKfe.png" title="source: imgur.com" />
</p>

Taking a look at the files that rails creates can be a bit overwhelming at first, but don't worry - this tutorial will only touch on a handful of directories!  The top level directories that we are concered with are:
* *app* - This is where our MVC structure lives.
* *config* - Inside this directory, in the routes.rb file is where we will tell our Rails app which HTTP requests to respond to.
* *db* - Where our database structure will be set up.

In addition to these directories, we will also be dealing with our Gemfile, which is where we will tell Rails about any other gems we might need to run our app. For our task manager we will be adding just one gem to our Gemfile. Open your gemfile and add `pry` to the `:development, :test` group - your Gemfile should now look like this:

```ruby 
source "https://rubygems.org"
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby "3.1.1"

# Bundle edge Rails instead: gem "rails", github: "rails/rails", branch: "main"
gem "rails", "~> 7.0.4"

# The original asset pipeline for Rails [https://github.com/rails/sprockets-rails]
gem "sprockets-rails"

# Use postgresql as the database for Active Record
gem "pg", "~> 1.1"

# Use the Puma web server [https://github.com/puma/puma]
gem "puma", "~> 5.0"

# Use JavaScript with ESM import maps [https://github.com/rails/importmap-rails]
gem "importmap-rails"

# Hotwire's SPA-like page accelerator [https://turbo.hotwired.dev]
gem "turbo-rails"

# Hotwire's modest JavaScript framework [https://stimulus.hotwired.dev]
gem "stimulus-rails"

# Build JSON APIs with ease [https://github.com/rails/jbuilder]
gem "jbuilder"

# Use Redis adapter to run Action Cable in production
# gem "redis", "~> 4.0"

# Use Kredis to get higher-level data types in Redis [https://github.com/rails/kredis]
# gem "kredis"

# Use Active Model has_secure_password [https://guides.rubyonrails.org/active_model_basics.html#securepassword]
# gem "bcrypt", "~> 3.1.7"

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem "tzinfo-data", platforms: %i[ mingw mswin x64_mingw jruby ]

# Reduces boot times through caching; required in config/boot.rb
gem "bootsnap", require: false

# Use Sass to process CSS
# gem "sassc-rails"

# Use Active Storage variants [https://guides.rubyonrails.org/active_storage_overview.html#transforming-images]
# gem "image_processing", "~> 1.2"

group :development, :test do
  # See https://guides.rubyonrails.org/debugging_rails_applications.html#debugging-with-the-debug-gem
  gem "debug", platforms: %i[ mri mingw x64_mingw ]
  gem "pry"
end

group :development do
  # Use console on exceptions pages [https://github.com/rails/web-console]
  gem "web-console"

  # Add speed badges [https://github.com/MiniProfiler/rack-mini-profiler]
  # gem "rack-mini-profiler"

  # Speed up commands on slow machines / big apps [https://github.com/rails/spring]
  # gem "spring"
end
```

Any time we update our Gemfile, we will need to tell our application to install or update the additional gems.  In your terminal, run the command:

```
$ bundle install
```

Great - now we can use `binding.pry` anywhere in our app to debug as we go!

## Getting the App Running

Before we can see what our new rails app can do, we need to do some more set up. First, let's create our app's database. Do this from the command line with:

`rails db:create`

You should see some output like this:

```
Created database 'task_manager_development'
Created database 'task_manager_test'
```

Notice how this created two databases.

Now we will need to get our server up and running and ready for HTTP requests. To do this, run either

`rails server` or `rails s`

You should see something like this:

```
=> Booting Puma
=> Rails 7.0.4 application starting in development
=> Run `bin/rails server --help` for more startup options
Puma starting in single mode...
* Puma version: 5.6.5 (ruby 3.1.1-p18) ("Birdie's Version")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 66200
* Listening on http://127.0.0.1:3000
* Listening on http://[::1]:3000
Use Ctrl-C to stop
```

Navigate to [http://localhost:3000/](http://localhost:3000/) and you should see some Rails magic!

Now, let's take a look back at your terminal and walk through what just happened.

```
Started GET "/" for ::1 at 2022-11-29 18:51:51 -0700
Processing by Rails::WelcomeController#index as HTML
  Rendering /Users/mdao/.rbenv/versions/3.1.1/lib/ruby/gems/3.1.0/gems/railties-7.0.4/lib/rails/templates/rails/welcome/index.html.erb
  Rendered /Users/mdao/.rbenv/versions/3.1.1/lib/ruby/gems/3.1.0/gems/railties-7.0.4/lib/rails/templates/rails/welcome/index.html.erb (Duration: 1.4ms | Allocations: 635)
Completed 200 OK in 13ms (Views: 4.9ms | ActiveRecord: 0.0ms | Allocations: 5094)
```

On the first line, you are seeing a snapshot of the HTTP request that was received by our server when we navigated to localhost:3000 - `GET "/"`.  This is basically telling us that our application received a request for the information that lives at a certain address.

On the last line, we can see a snapshot of the response that our server sent back to the browser `Completed 200 OK`. Meaning, we received a request, were able to process that request and succesfully send back a response.  The response is what contains the information that allows the browser to render the default page for your Rails app!

## Using Views

Let's update our app to show something other than the default Rails welcome - this is, after all, *your app*!

The first thing we need to do, is tell our app how to handle specific requests, we will do this by adding the following code to our `config/routes.rb` file:

```ruby
Rails.application.routes.draw do
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"

  get "/", to: "welcome#index"
end
```

This line that we added is telling our application that anytime we receive an HTTP `GET` request for the URI `'/'`, we should perform the `index` action within our `welcome` controller.  

Go back to your browser and refresh the page - you should now be seeing an error telling you that you have a `Routing Error`; specifically, that you have an `uninitialized constant WelcomeController`.  This is a good thing; at this point, we have told our application which controller action to perform when this request is received, but we haven't created that controller (or the action) yet.  So, let's go do that.

Open your `app/controllers` directory and create a file inside called `welcome_controller.rb`.  In that file, add the following code (remember that it is *strongly* recommended that you not copy/paste, but practice typing out this code).

```ruby
# app/controllers/welcome_controller.rb

class WelcomeController < ApplicationController


end
```

Go back to your browser and refresh the page again.  You should now be seeing a new error `The action 'index' could not be found for WelcomeController`.  Rails is telling us that it was able to get to the controller that we wanted, but wasn't able to find the action that we had specified for this request.  Update your `welcome_controller.rb` to include this action:

```ruby
# app/controllers/welcome_controller.rb

class WelcomeController < ApplicationController
  def index

  end
end
```

Refresh the page again. New error! You should now see the following text (with some additional info): `WelcomeController#index is missing a template for request formats: text/html`.  Rails is telling us that now it was able to find the controller and action we wanted, but once there, it didn't know what information to send back to the browser in the response body.  It was expecting to find some HMTL that it could send back for the browser to render, but it found nothing. Let's go create that HTML in our views directory.

In your `app/views` directory, add a sub-directory called `welcome` and, within that directory, a file called `index.html.erb`.  When creating views, we name the files the same as our action: `def index` to `index.html.erb`. And, those files will live in a directory with the same name as our controller: `WelcomeController` to `views/welcome/`. The path for your new file should be `app/views/welcome/index.html.erb`.

In that file, add the following HTML:

```erb
<h1>Welcome to the Task Manager</h1>

<ul>
  <li><a href="/tasks">Task Index</a></li>
  <li><a href="/tasks/new">New Task</a></li>
</ul>
```

We have an h1 tag for our welcome message, then an unordered list (ul) with two list items (li) inside. If you are unfamiliar with HTML tags, try one of the HTML tutorials before continuing.

Inside of each li tag, we have an `<a>` tag. The href of the tag is the path where the link will go. In the first a tag, the path will be [http://localhost:3000/tasks](http://localhost:3000/tasks). The second path will be [http://localhost:3000/tasks/new](http://localhost:3000/tasks/new).

Refresh your browser and you should now see an error-free view! What do you think will happen if you click on one of these links? More errors - but that's ok! We can use these errors to help guide us in our development.