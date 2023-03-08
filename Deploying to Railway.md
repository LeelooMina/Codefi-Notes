# Deploying an app to Railway

Railway is one of many hosting platform to deploy web applications to the internet. It offers a free tier for smaller projects.

Other sites include:

Render
Fly.io
Heroku

These may require payment or only allow a free trial period with credit card information.

## Installing the Railway CLI

Type the folowing code into the command line inside your project folder:

```
npm i -g @railway/cli
```

Test to see if it's installed correctly:

```
railway --version
```

This should display what version of Railway is installed.


## Creating a Railway Account

Go to the Railway website: https://railway.app/

Click on the link to sign up or log into an account. If you use your GitHub account, the whole process will be a lot simpler because it will already be linked directly to your codebase. 

Your new account will be more limited until you take some time to verify it. The verification process is pretty simple. You just need to read and agree to the terms and services.

## Preparing Project

Make sure that your project is located on Github and push the latest changes.

Railway may have issues with names that contain underscores, so it's best to rename your projects.

**project_name** to **project-name**


## Logging into Railway with the CLI

Run the following command in your terminal:

```
railway login
```

The cli will prompt you to open the browser, it should pop up a windows to log you in.

Next run this command:

```
railway init
```

It should ask you if you would like to start an empty project or a starter template. You're going to want to select "Empty Project".

Enter your prject name in the prompt. It should open your browser to send you to the new Railway project. 

## Setting up PostgresSQL and Redis

Inside the Gemfile for your project, locate the sqlite3 gem. It should look similar to the following:

```properties 
# Use dqlite3 as the database for Active Record
gem "sqlite3", "~1.4"
```

This block needs to be moved to the devlopment section, as below:

```ruby
group :development, :test do
# Use dqlite3 as the database for Active Record
gem "sqlite3", "~1.4"
# Other gems such as "debug" may already be listed within this block
end
```

Then add another group inside the Gemfile for production. Include the PostgreSQL gem, as follows:

```ruby
group :production, :test do
# Use PostgreSQL as the database for Active Record
gem "pg", "~1.2"
```

If you have the pg gem outside of the the production block, it may clash with sqlite during testing.

Run:

```
bundle install
```
Then:

```
rails db:migrate
```
These should update the schema of your app.

Be sure to push your changes to the GitHub repo.

## Setting up Railway

Navigate to the project set up on the Railway site. Click the button in the center of the screen to add a new service.

Click on the "Database" choice, then PostgreSQL.

Repeat the action except for this time, click to add Redis.

Go to the purple plus sign labled "New" on the right side. Go to GitHub repo.

In the search bar labled "What can we help with?" search for your application's name.


## Including Enviromental Variables

On the Railway site, click on the box with your app name. It should pop open a menu with a tab called "Variables."

Now, back to your project terminal. Enter in the following code:

```
EDITOR="code --wait" bin/rails credentials:edit
```

If you're on a Windows computer, you may have to use the following code instead:

```
EDITOR="nano --wait" bin/rails credentials:edit
```

This command is going to open up an editor that can handle the credentials file.

The file should be similar to this:

```ruby
secret_key_base: 31123j123lk1l2l3sldf...
```

You need to create a variable on the Railway website. Name the variable SECRET_KEY_BASE (all caps is imporant!) and set it to the long key from your credentials file. Remember to click the "add" button to save the new variable.

Be sure to close the credentials file.

**Wait for your app to finish building.**

## Using the Railway Command Line Interface

```
railway run
```

This command allows you to run regular rails commands in the production enviroment of your application on Railway. 

Now that your app is done building, you need to migrate your database files inside the production enviroment. 

```
railway run rails db:migrate
```

## Check out your site!

Inside the settings of your app on the Railway site, there should be a link to generate a domain for your application. This will be the url you can use to share your creation. You can also use a custom domain if you own one. 



