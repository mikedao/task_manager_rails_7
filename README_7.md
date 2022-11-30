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