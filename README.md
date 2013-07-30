#### This is a fork of github.com/syhunak/twitter-bootstrap-rails

# Twitter Bootstrap 3 for Rails 4 Asset Pipeline
Bootstrap is a toolkit from Twitter designed to kickstart development of webapps and sites. It includes base CSS and HTML for typography, forms, buttons, tables, grids, navigation, and more.

twitter-bootstrap-rails project integrates Bootstrap CSS toolkit for Rails 4 Asset Pipeline (Rails 3.1 and Rails 3.2 supported)

## Installing the Gem

The [Twitter Bootstrap Rails gem](http://rubygems.org/gems/twitter-bootstrap-rails) can provide the Twitter Bootstrap stylesheets in two ways.

The plain CSS way is how Twitter Bootstrap is provided on [the official website](http://twitter.github.com/bootstrap/).

The [Less](http://lesscss.org/) way provides more customisation options, like changing theme colors, and provides useful Less mixins for your code, but requires the
Less gem and the Ruby Racer Javascript runtime (not available on Microsoft Windows).

### Installing the Less stylesheets

To use Less stylesheets, you'll need the [less-rails gem](http://rubygems.org/gems/less-rails), and one of [Javascript runtimes supported by CommonJS](https://github.com/cowboyd/commonjs.rb#supported-runtimes).

Modify these lines in the Gemfile to install the gems from [RubyGems.org](http://rubygems.org):

```ruby
# We want to disable sass, by commenting it out, since we're using less.
# gem 'sass-rails'
gem 'therubyracer' # Add this if missing
gem 'less-rails' # Add this - Sprockets (what Rails 4 uses for its asset pipeline) supports LESS
gem 'twitter-bootstrap-rails', :git => 'git://github.com/katzenbaer/twitter-bootstrap-rails.git' # Add this
```

or you can install from latest build;

```ruby
gem 'twitter-bootstrap-rails', :git => 'git://github.com/katzenbaer/twitter-bootstrap-rails.git', :branch => 'bootstrap-3.0.0'
```

Then run `bundle install` from the command line:

    bundle install

Then run the bootstrap generator to add Bootstrap includes into your assets:

    rails g bootstrap:install less

To update the gem when new builds are pushed, run `bundle update` from the command line:

    bundle update

## Generating layouts and views

You can run following generators to get started with Twitter Bootstrap quickly.

###Layout  
Generates Twitter Bootstrap compatible layout. *Haml and Slim supported*  

Usage:

    rails g bootstrap:layout [LAYOUT_NAME] [*fixed or fluid]

Example of a fixed layout:

    rails g bootstrap:layout application fixed

Example of a responsive layout:

    rails g bootstrap:layout application fluid

###Themed  
Generates Twitter Bootstrap compatible scaffold views. *Haml and Slim supported*  

Usage:

    rails g bootstrap:themed [RESOURCE_NAME]

Example:

    rails g scaffold Post title:string description:text --no-stylesheets
    rake db:migrate
    rails g bootstrap:themed Posts

**Note the plural usage of the resource to generate bootstrap:themed.**  

## Using Less

Bootstrap was built with Preboot, an open-source pack of mixins and variables to be used in conjunction with Less, a CSS preprocessor for faster and easier web development.

## Using stylesheets with Less

You have to require Bootstrap LESS (bootstrap_and_overrides.css.less) in your application.css

```css
/*
 *= require bootstrap_and_overrides
 */

/* Your stylesheets goes here... */
```

To use individual components from bootstrap, your bootstrap_and_overrides.less could look like this:

```css
// Core variables and mixins
@import "twbs/bootstrap/variables.less";
@import "twbs/bootstrap/mixins.less";

// Reset
@import "twbs/bootstrap/normalize.less";

// Core CSS
@import "twbs/bootstrap/scaffolding.less";
@import "twbs/bootstrap/type.less";
@import "twbs/bootstrap/code.less";
@import "twbs/bootstrap/grid.less";

@import "twbs/bootstrap/tables.less";
@import "twbs/bootstrap/forms.less";
@import "twbs/bootstrap/buttons.less";

// Components: common
@import "twbs/bootstrap/input-groups.less";
@import "twbs/bootstrap/dropdowns.less";
@import "twbs/bootstrap/close.less";

// Components: Nav
@import "twbs/bootstrap/navs.less";
@import "twbs/bootstrap/navbar.less";

// Components: Popovers
@import "twbs/bootstrap/modals.less";
@import "twbs/bootstrap/tooltip.less";

// Components: Misc
@import "twbs/bootstrap/alerts.less";
@import "twbs/bootstrap/progress-bars.less";
@import "twbs/bootstrap/accordion.less";
@import "twbs/bootstrap/carousel.less";
@import "twbs/bootstrap/jumbotron.less";

// Utility classes
@import "twbs/bootstrap/utilities.less"; // Has to be last to override when necessary
@import "twbs/bootstrap/responsive-utilities.less";
```

If you'd like to alter Bootstrap's own variables, or define your LESS
styles inheriting Bootstrap's mixins, you can do so inside bootstrap_and_overrides.css.less:

```css
@navbar-brand-color: #ff0000;
@navbar-brand-hover-color: #00ff00;
```

### Icons

By default, this gem overrides standard Bootstraps's Glyphicons with Font Awesome (http://fortawesome.github.com/Font-Awesome/).
If you would like to restore the default Glyphicons, inside the _bootstrap_and_overrides.css.less_ remove the FontAwesome declaration and uncomment the line:

```css
/* Font Awesome (default - comment to disable) */
//@import "fontawesome/font-awesome";

/* Glyphicons */
@import "twbs/bootstrap-glyphicons/bootstrap-glyphicons";
```

## Using Javascripts

Require Bootstrap JS (bootstrap.js) in your application.js

```js
//= require twbs/bootstrap

$(function(){
  /* Your javascripts goes here... */
});
```

If you want to customize what is loaded, your application.js would look something like this

```js
//= require jquery
//= require jquery_ujs
//= require twbs/bootstrap/bootstrap-transition
//= require twbs/bootstrap/bootstrap-alert
//= require twbs/bootstrap/bootstrap-modal
//= require twbs/bootstrap/bootstrap-button
//= require twbs/bootstrap/bootstrap-collapse
```

...and so on for each bootstrap js component.

## Using Coffeescript (optionally)

Using Twitter Bootstrap with the CoffeeScript is easy.
twitter-bootstrap-rails generates a "bootstrap.js.coffee" file for you
to /app/assets/javascripts/ folder.

```coffee
jQuery ->
  # For performance reasons, the Tooltip and Popover data-apis are opt in.
  # Uncomment the following line to enable tooltips
  # $("[data-toggle='tooltip']").tooltip()

  # Uncomment the following line to enable popovers
  # $("[data-toggle='popover']").popover()
```

## Using Helpers

### Modal Helper
You can create modals easily using the following example. The header, body, and footer all accept content_tag or plain html.
The href of the button to launch the modal must matche the id of the modal dialog.

```ruby
<%= content_tag :a, "Modal", :href => "#modal", :class => 'btn', :data => {:toggle => modal'} %>

<%= modal_dialog :id => "modal",
		 :header => { :show_close => true, :dismiss => 'modal', :title => 'Modal header' },
                 :body   => 'This is the body',
                 :footer => content_tag(:button, 'Save', :class => 'btn') %>
```

### Navbar Helper
It should let you write things like:

````
<%= nav_bar :fixed => :top, :brand => "Fashionable Clicheizr 2.0", :responsive => true do %>
	<%= menu_group do %>
		<%= menu_item "Home", root_path %>
		<%= menu_divider %>
		<%= drop_down "Products" do %>
			<%= menu_item "Things you can't afford", expensive_products_path %>
			<%= menu_item "Things that won't suit you anyway", harem_pants_path %>
			<%= menu_item "Things you're not even cool enough to buy anyway", hipster_products_path %>
			<% if current_user.lives_in_hackney? %>
				<%= menu_item "Bikes", fixed_wheel_bikes_path %>
			<% end %>
		<% end %>
		<%= menu_item "About Us", about_us_path %>
		<%= menu_item "Contact", contact_path %>
	<% end %>
	<%= menu_group :pull => :right do %>
		<% if current_user %>
			<%= menu_item "Log Out", log_out_path %>
		<% else %>
			<%= form_for @user, :url => session_path(:user), html => {:class=> "navbar-form pull-right"} do |f| -%>
			  <p><%= f.text_field :email %></p>
			  <p><%= f.password_field :password %></p>
			  <p><%= f.submit "Sign in" %></p>
			<% end -%>
		<% end %>
	<% end %>
<% end %>
````

### Navbar scaffolding

In your view file (most likely application.html.erb) to get a basic navbar set up you need to do this:

````
<%= nav_bar  %>
````

Which will render:

	<div class="navbar">
	  <div class="navbar-inner">
	    <div class="container">
	    </div>
	  </div>
	</div>


### Fixed navbar

If you want the navbar to stick to the top of the screen, pass in the option like this:

````
<%= nav_bar :fixed => :top  %>
````

To render:

	<div class="navbar navbar-fixed-top">
	  <div class="navbar-inner">
	    <div class="container">
	    </div>
	  </div>
	</div>

### Static navbar

If you want a full-width navbar that scrolls away with the page, pass in the option like this:

````
<%= nav_bar :static => :top  %>
````

To render:

	<div class="navbar navbar-static-top">
	  <div class="navbar-inner">
	    <div class="container">
	    </div>
	  </div>
	</div>


### Brand name

Add the name of your site on the left hand edge of the navbar. By default, it will link to root_url. Passing a brand_link option will set the url to whatever you want.

````
<%= nav_bar :brand => "We're sooo web 2.0alizr", :brand_link => account_dashboard_path  %>
````

Which will render:

	<div class="navbar">
	  <div class="navbar-inner">
	    <div class="container">
			<a class="brand" href="/accounts/dashboard">
			  We're sooo web 2.0alizr
			</a>
	    </div>
	  </div>
	</div>


### Optional responsive variation

If you want the responsive version of the navbar to work (One that shrinks down on mobile devices etc.), you need to pass this option:

````
<%= nav_bar :responsive => true %>
````
Which renders the html quite differently:


	<div class="navbar">
	  <div class="navbar-inner">
	    <div class="container">
	      <!-- .btn-navbar is used as the toggle for collapsed navbar content -->
	      <a class="btn btn-navbar" data-toggle="collapse" data-target=".nav-collapse">
	        <span class="icon-bar"></span>
	        <span class="icon-bar"></span>
	        <span class="icon-bar"></span>
	      </a>
	      <!-- Everything in here gets hidden at 940px or less -->
	      <div class="nav-collapse">
	        <!-- menu items gets rendered here instead -->
	      </div>
	    </div>
	  </div>
	</div>


### Nav links

This is the 'meat' of the code where you define your menu items.

You can group menu items in theoretical boxes which you can apply logic to - e.g. show different collections for logged in users/logged out users, or simply right align a group.

The active menu item will be inferred from the link for now.

The important methods here are menu_group and menu_item.

menu_group only takes one argument - :pull - this moves the group left or right when passed :left or :right.

menu_item generates a link wrapped in an li tag. It takes two arguments and an options hash. The first argument is the name (the text that will appear in the menu), and the path (which defaults to "#" if left blank). The rest of the options are passed straight through to the link_to helper, so that you can add classes, ids, methods or data tags etc.

````
<%= nav_bar :fixed => :top, :brand => "Ninety Ten" do %>
	<% menu_group do %>
		<%= menu_item "Home", root_path %>
		<%= menu_item "About Us", about_us_path %>
		<%= menu_item "Contact", contact_path %>
	<% end %>
	<% if current_user %>
		<%= menu_item "Log Out", log_out_path %>
	<% else %>
		<% menu_group :pull => :right do %>
			<%= menu_item "Sign Up", registration_path %>
			<% form_for @user, :url => session_path(:user) do |f| -%>
			  <p><%= f.text_field :email %></p>
			  <p><%= f.password_field :password %></p>
			  <p><%= f.submit "Sign in" %></p>
			<% end -%>
		<% end %>
	<% end %>
<% end %>
````

### Dropdown menus

For multi-level list options, where it makes logical sense to group menu items, or simply to save space if you have a lot of pages, you can group menu items into drop down lists like this:

````
<%= nav_bar do %>
	<%= menu_item "Home", root_path %>

	<%= drop_down "Products" do %>
		<%= menu_item "Latest", latest_products_path %>
		<%= menu_item "Top Sellers", popular_products_path %>
		<%= drop_down_divider %>
		<%= menu_item "Discount Items", discounted_products_path %>
	<% end %>

	<%= menu_item "About Us", about_us_path %>
	<%= menu_item "Contact", contact_path %>
<% end %>
````

### Dividers

Dividers are just vertical bars that visually separate logically disparate groups of menu items

````
<%= nav_bar :fixed => :bottom do %>
	<%= menu_item "Home", root_path %>
	<%= menu_item "About Us", about_us_path %>
	<%= menu_item "Contact", contact_path %>

	<%= menu_divider %>

	<%= menu_item "Edit Profile", edit_user_path(current_user) %>
	<%= menu_item "Account Settings", edit_user_account_path(current_user, @account) %>
	<%= menu_item "Log Out", log_out_path %>
<% end %>
````

### Forms in navbar

At the moment - this is just a how to...

You need to add this class to the form itself (Different form builders do this in different ways - please check out the relevant docs)

````css
.navbar-form
````
To pull the form left or right, add either of these classes:
````css
.pull-left
.pull-right
````

If you want the Bootstrap search box (I think it just rounds the corners), use:
````css
.navbar-search
````
Instead of:
````css
.navbar-form
````

To change the size of the form fields, use .span2 (or however many span widths you want) to the input itself.

### Component alignment

You can shift things to the left or the right across the nav bar. It's easiest to do this on grouped menu items:

````
<%= nav_bar :fixed => :bottom do %>
	<% menu_group do %>
		<%= menu_item "Home", root_path %>
		<%= menu_item "About Us", about_us_path %>
		<%= menu_item "Contact", contact_path %>
	<% end %>
	<% menu_group :pull => :right do %>
		<%= menu_item "Edit Profile", edit_user_path(current_user) %>
		<%= menu_item "Account Settings", edit_user_account_path(current_user, @account) %>
		<%= menu_item "Log Out", log_out_path %>
	<% end %>
<% end %>
````

### Text in the navbar

If you want to put regular plain text in the navbar anywhere, you do it like this:

````
<%= nav_bar :brand => "Apple" do %>
	<%= menu_text "We make shiny things" %>
	<%= menu_item "Home", root_path %>
	<%= menu_item "About Us", about_us_path %>
<% end %>
````
It also takes the :pull option to drag it to the left or right.


### Flash helper

Add flash helper `<%= bootstrap_flash %>` to your layout (built-in with layout generator)

### Breadcrumbs Helpers

*Notice* If your application is using [breadcrumbs-on-rails](https://github.com/weppos/breadcrumbs_on_rails) you will have a namespace collision with the add_breadcrumb method. 
You do not need to use these breadcrumb gems since this gem provides the same functionality out of the box without the additional dependency. 

Add breadcrumbs helper `<%= render_breadcrumbs %>` to your layout.

```ruby
class ApplicationController
  add_breadcrumb :index, :root_path
end
```

```ruby
class ExamplesController < ApplicationController
  add_breadcrumb :index, :examples_path

  def index
  end

  def show
    @example = Example.find params[:id]
    add_breadcrumb @example.name, example_path(@example)
    # add_breadcrumb :show, example_path(@example)
  end
end
```

###i18n Internationalization Support
The installer creates an english translation file for you and copies it to config/locales/en.bootstrap.yml

NOTE: If you are using Devise in your project, you must have a devise locale file
for handling flash messages, even if those messages are blank. See https://github.com/plataformatec/devise/wiki/I18n

## Changelog
<ul>
  <li><strong>July 29 2013</strong>  
    <ul>
      <li>Forked from diowa/twitter-bootstrap-rails</li>
      <li>Updated menu_helpers to Bootstrap 3.0 RC1 standards</li>
      <li>Merged bootstrap-3.0.0 into master</li>
    </ul>
  </li>
</ul>

## Contributors & Patches & Forks
*All those listed at http://github.com/seyhunak/twitter-bootstrap-rails*

### Contact me
Terrence Katzenbaer - me [at] katzenbaer net

## Thanks
@seyhunak for the original twitter-bootstrap-rails and all its contributors.  
@diowa for starting the Bootstrap 3 migration which this fork is based on.  

Twitter Bootstrap (http://getbootstrap.com/)
http://twitter.github.com/bootstrap

## License

Copyright (c) 2013 Terrence Katzenbaer

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

###Original License
Copyright (c) 2012 Seyhun Akyürek

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
