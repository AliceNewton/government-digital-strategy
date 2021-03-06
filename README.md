*"Publish, don't send."* - Mike Bracken

# What is this?

This repository contains the code and content behind the Government Digital
Strategy site as seen online at [http://publications.cabinetoffice.gov.uk/digital/](http://publications.cabinetoffice.gov.uk/digital/ "Government Digital Strategy online publication")

By following the instructions below anyone comfortable with using command line can set up a local, working, copy of the site, and view the content as it appears online.

# Quick Start

The following 10 steps should get you up and running pretty quickly. All steps are further documented below.

1. Install [Node.js](http://nodejs.org/), and [npm](https://npmjs.org/). Most installs of Node come with npm. To check, run `node -v` and `npm -v` on the command line.
2. Install [Ruby](http://www.ruby-lang.org/en/) v1.9+ and [RubyGems](http://rubygems.org/). Check these versions with `ruby -v` and `gem -v` (We recommend you don't install over the system version of Ruby. Tools like [rbenv](https://github.com/sstephenson/rbenv) let you manage Ruby versions nicely.)
3. Install the [Bundler gem](http://gembundler.com/) with `gem install bundler`
4. Clone the project: `git clone git@github.com:alphagov/government-digital-strategy.git` (Or you may decide to fork and clone your own version).
5. `cd` into the project directory.
6. Run `npm install` to install all Node dependencies.
7. Run `bundle` (short for `bundle install`) to install all Ruby dependencies.
8. Run the deploy script `./deploy.sh`
9. Run the server: `ruby deploy-server.rb`
10. Visit `http://localhost:9090/digital` to view.

If you're editing the documents, run the watch task, which will auto-compile every time it detects a change.

```
ruby built-server.rb
ruby watch-build.rb
```

Then hit up `http://localhost:8080`. Now everytime a file in `source/` or `assets/` gets updated, it's automatically built. You will need to refresh the browser though, but there's tools out there that will even do that bit for you.


# Before running the build script

Make sure you've got Node & npm installed (see links above), and then CD into the directory and run:

```
npm install
```

This uses the `package.json` file to install dependencies.

Similarly, run `bundle` to make sure all the Gems are installed

```
bundle
```


# Local Build Script

Run the shell script:

```
./local-build.sh
```

This compiles everything into the `built` folder. To view it locally, run `ruby built-server.rb` and head to `http://localhost:8080`


# Production Build Script

The local-build script is great for viewing locally but doesn't do any of the performance stuff we want - minifying CSS, JS and so on. The production build script does.

```
./deploy.sh
```

It will compile and minify the JS. This uses the RequireJS optimizer.

It will also minify all CSS.

Once it's done, you're left with a `deploy/` folder which is the production-ready files. This is what should be deployed to the server.

If you want to test that the deploy folder works fine, run `ruby deploy-server.rb`, which serves up the `deploy/` folder on port 9090.



# Assets

All CSS should be written in Sass (using the SCSS syntax) and live in the `assets/sass` folder. These get compiled to the `assets/css` folder. __Never edit the CSS directly__, as it will get overwritten by the build script.

__NEVER EDIT A FILE in the `built/` folder__. These get overwritten by the build script and are not tracked by Github. The same goes for the `deploy/` folder.

Templates, partials, code, images and so on live in `assets/`

Content goes into `source/`

# Partials and Templates and Syntax

You should check `source/sample-document` for a living demo of how everything works.

Partials live in `source/partials` and should be named with an underscore at the start, eg `_mypartial.html`

A partial can contain Markdown or HTML, just name it as `.md` or `.html`

To include a Markdown, in either your HTML or MD file, use:

```
{include action-text.md}
```

That would look for `source/partials/_action-text.md`

You can put partials within subfolders:

```
{include digital/action-01.md}
```

Looks for `source/partials/digital/_action-01.md`.

If you include a MD partial in an HTML page, the MD is compiled first.

# Templates

Similarly to partials, for the HTML pages we have templates you can use. These are in `assets/templates/`

The main line you need to look for is `<!--REPLACE-->`. The contents of the page that uses this template are put into the file, in place of that shortcut.

The line `<!--TIME-->` is replaced with the compilation time. This is mainly for debugging.

The line `<!--META-->` gets replaced with the contents of `meta.html` within the document directory, if there is one. This is where extra stuff to be put in the `<head>` on a per-document basis exists.

To assign a template to a HTML file, insert, at the very top of the HTML file, a tag, for example, here's one of the HTML files that uses the home_template:

```
{home_template}
      <div class="document-title">
        <div class="title">
          <h2>Digital services so good people prefer to use them</h2>
        </div>
      </div>
      ...
</body>
</html>
```

That would look for `asset/templates/home_template.html` and the above contents would be put into the template where `<!--REPLACE-->` is.

The digital documents use the `digital_doc_template.html`. The others use `generic_template.html`. Individual files can use any template they like, as defined above.
