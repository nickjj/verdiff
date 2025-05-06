# Verdiff

A command line script that will show you exactly what changed between 2
versions of your favorite web framework.

It currently supports Rails, Django, Laravel and Phoenix.

*It's a zero dependency Python script that only requires having Docker
installed. Docker is used to to build specific versions of your framework of
choice without polluting your system with these dependencies.*

## 📑 Sample Output

Running this command:

```
$ verdiff --framework rails 8.0.0 8.0.2
```

Produces this output:

```diff
--- /app/rails/8.0.0/a1746446009/.github/workflows/ci.yml       2025-05-05 11:54:42.000000000 +0000
+++ /app/rails/8.0.2/a1746446009/.github/workflows/ci.yml       2025-05-05 11:54:47.000000000 +0000
@@ -74,7 +74,7 @@

     steps:
       - name: Install packages
-        run: sudo apt-get update && sudo apt-get install --no-install-recommends -y google-chrome-stable curl libjemalloc2 libvips postgresql-client
+        run: sudo apt-get update && sudo apt-get install --no-install-recommends -y build-essential git libpq-dev libyaml-dev pkg-config google-chrome-stable

       - name: Checkout code
         uses: actions/checkout@v4
diff --color -u --exclude vendor -r /app/rails/8.0.0/a1746446009/Dockerfile /app/rails/8.0.2/a1746446009/Dockerfile
--- /app/rails/8.0.0/a1746446009/Dockerfile     2025-05-05 11:54:42.000000000 +0000
+++ /app/rails/8.0.2/a1746446009/Dockerfile     2025-05-05 11:54:47.000000000 +0000
@@ -30,7 +30,7 @@

 # Install packages needed to build gems
 RUN apt-get update -qq && \
-    apt-get install --no-install-recommends -y build-essential git libpq-dev pkg-config && \
+    apt-get install --no-install-recommends -y build-essential git libpq-dev libyaml-dev pkg-config && \
     rm -rf /var/lib/apt/lists /var/cache/apt/archives

 # Install application gems
diff --color -u --exclude vendor -r /app/rails/8.0.0/a1746446009/Gemfile /app/rails/8.0.2/a1746446009/Gemfile
--- /app/rails/8.0.0/a1746446009/Gemfile        2025-05-05 11:54:42.000000000 +0000
+++ /app/rails/8.0.2/a1746446009/Gemfile        2025-05-05 11:54:47.000000000 +0000
@@ -1,7 +1,7 @@
 source "https://rubygems.org"

 # Bundle edge Rails instead: gem "rails", github: "rails/rails", branch: "main"
-gem "rails", "~> 8.0.0"
+gem "rails", "~> 8.0.2"
 # The modern asset pipeline for Rails [https://github.com/rails/propshaft]
 gem "propshaft"
 # Use postgresql as the database for Active Record
diff --color -u --exclude vendor -r /app/rails/8.0.0/a1746446009/config/credentials.yml.enc /app/rails/8.0.2/a1746446009/config/credentials.yml.enc
--- /app/rails/8.0.0/a1746446009/config/credentials.yml.enc     2025-05-05 11:54:42.000000000 +0000
+++ /app/rails/8.0.2/a1746446009/config/credentials.yml.enc     2025-05-05 11:54:47.000000000 +0000
@@ -1 +1 @@
-v7w5ZDUFuH78UIb9h9VgMEE6sQuxB+qZhh302fqxDai9BZ0KtSBPxMmZ72oxmEGVBkW4Aj/tDJzlDCpuJKYz7QHL1FthN93fDR8iEwRdT7amD2W4te0uDZWXIk3TXDyED/+2Di01bXtlsaAB3oEzE7Ze3O76sHoGNWmCcxDHEZ/SwBl6AVFcG11NT2O7i9+zmoJXMqN7rRCEHpNMf3vFx2dEBR4W+NkO0/SRpE1flB97EqHvxrVa9Tl87DIndXKVNUfhTycJeA4asmz/+gNiJtk2mDg4apotEwk9xEjEs+vIE/8C/qRCKKSOVNFCpq6ROEux1Y3mwJMc4Dos2GZBz04kY8C1r5vqZzeT43pqA8hc+NXBJKxD0tXTGsHbDB29O7mH31gQ1wewY99b1GtDG+zwE/EPBNpMUUY9Q9NQ0KBBbQgvOxhmctNKrXaN7DKep2nd9vp9U2bx+HEO24vplAHh3nAggbAvK21eYxId96SPPJLG3UbxLcGj--lxvSQw05L2SZ2lYR--dCVHu7psoZJnWoLybRRUBg==
\ No newline at end of file
+Snij5sIyO/SBxMBCBYAD61/2EIbgBgtPpEqmXGVwZp4G3OG8NHeDcJfZirgjfedpQN94QXWH2MSQbQsqqrhHXHym5HuoyZXeOaxUIvhZPCeQxDMhX378UyTBvAATxp+zeZ0GVl4JlQaI4qVv3ED4hKP3ZRcXe9cawVA39d8upVVAIUgdhZIyOLA9t9bORUxXlCrmATJU7cgM+AbF1AjEFNpfUh24G7wZ2P2iYdaD+2SqRba9VP9CezYC7w4nlU1A6CgXHRAQTaOSmuIv+0A7xIPvQAZGkoh9zsw9cgFEag3DutfJQJEZqQpH79uwxA35GD7VmuUqkpn4TF3ZuRUQn10vOJwNZic3/SgmMh8HlBe2lODc6wRHgtpT1HTtJVpmP/KbqtCgAkBMe9f5HydtQ8eLYyP328z6yMmSjVW6KLqDdfyJg+6tCTZnnGDtqVuF+XZHqIfHXdsaqFe8KiV/SPADEwdj5e0XZhYFENWEfrT1+AzXcVQeX83A--0iwPXfuyq+ymE3iH--unlIM5/ujXmx6k+3Ca3UQA==
\ No newline at end of file
diff --color -u --exclude vendor -r /app/rails/8.0.0/a1746446009/config/master.key /app/rails/8.0.2/a1746446009/config/master.key
--- /app/rails/8.0.0/a1746446009/config/master.key      2025-05-05 11:54:42.000000000 +0000
+++ /app/rails/8.0.2/a1746446009/config/master.key      2025-05-05 11:54:47.000000000 +0000
@@ -1 +1 @@
-6519c481904c343b3a1db522428ca679
\ No newline at end of file
+62354f1ebd6a3e25ee102ca4415c9639
\ No newline at end of file
```

## ❓ But Why?

A lot of popular web frameworks allow you to generate a new project with a
command line tool. For example with Rails you can run `rails new hello`, or
with Django you can run `django-admin startproject hello`.

This is how most of us start a new project with those frameworks.

So let's say you were developing your project for 6 months and since you
started there were a few small patches and a minor bump in your web framework
of choice's version.

You've been hacking away on your project and it's going great and today is
the day you decide to upgrade your web framework's version to the latest
version. How do you approach this?

You're most likely not going to generate a brand new project and start from
scratch because you already have a project. You'll likely read the docs on what
changed but the docs don't include exactly what files changed between versions.

That's where this tool comes into play. You can run it against your current
version of your framework of choice along with the version you want to upgrade
to and it will show you exactly what files you need to change in your project
to get up to date.

### What I Was Doing before This Project Existed

Without this tool, it was a really tedious process to see what changed. I would
typically:

- Launch a Docker container for XYZ language (let's say Ruby)
- Install my project's current version of Rails (a web framework for Ruby)
- Generate project A in a directory
- Install the version of Rails I want to upgrade to
- Generate project B in a different directory
- Run the `diff` tool to see what changed between both versions

It was a whole ceremony and it was necessary because you can't just generate
a new project and diff it against your current project because chances are you
made a ton of changes in the files that were generated and that creates too
much noise in the diff output. Plus you were still on the hook for running a
bunch of commands manually.

This tool pretty much does the exact workflow described above and it cleans up
after itself so nothing is left on your dev box since it's using Docker to
install and run all of the commands.

### But Wait, There's More!

I don't know about you but I have all of my web applications in Docker images
and I do not have most programming run-times installed directly on my dev box.

So when it comes to creating a brand new project, I would typically launch a
Docker container for Ruby, install the latest version of Rails, generate
my project and through the power of volume mounts I would end up with a new
project on my dev box.

That's a tedious workflow and it can be avoided. Verdiff supports an optional
`--save` flag so you can persist the generated projects to your dev box, which
means this tool doubles as a way to quickly generate brand new projects too.

## ⚡️ Installation

Since this project uses Docker you'll need to install Docker. If you're not
sure what Docker is or why it's useful, I wrote over 100+ blog posts about
Docker which you may want to check out [on my
website](https://nickjanetakis.com/blog/tag/docker-tips-tricks-and-tutorials).

The next step is to download `verdiff`, make sure it's executable and place it
somewhere on your system path.

#### 1 liner to get `verdiff` downloaded to `/usr/local/bin`:

```sh
sudo curl \
  -L https://raw.githubusercontent.com/nickjj/verdiff/v0.2.0/verdiff \
  -o /usr/local/bin/verdiff && sudo chmod +x /usr/local/bin/verdiff
```

You can confirm it works by running `verdiff --help`.

That will download the latest release. If you want to download the bleeding
edge version you can replace the *version number* with *master* in the above
command.

## 🚀 Usage Examples

You can run `verdiff` from anywhere you want but keep in mind it will
temporarily write out a custom Dockerfile in the current directory, but this
file has both a project name and time stamp in its file name so there's a very
good chance it won't conflict with any Dockerfile you have in the current
directory.

If you run `verdiff --help` you can see exactly what's available but here's a
few usage examples that demonstrate using every available flag and option.

```sh
# 1. Compare versions of a Rails project (the default --framework)
verdiff 8.0.0 8.0.2

# 1. Compare versions of a Django project
# 2. Set DEBUG=1 to see the Dockerfile that got used and all Docker commands that were run
DEBUG=1 verdiff --framework django 5.1.8 5.2

# 1. Compare versions of a Laravel project
# 2. Supply a custom project name for the generated projects
# 3. Persist the projects to your dev box in a laravel/ folder in the current directory
verdiff --framework laravel 12.0.0 12.0.7 --project hello --save

# 1. Compare version of a Phoenix project
# 2. Keep the Docker image around after it completes in case you want to check it out
# 3. Keep the Dockerfile around in the current directory in case you want to check it out
verdiff --framework phoenix 1.7.0 1.7.21 --keep-image --keep-dockerfile
```

It's also worth mentioning that once `verdiff` finishes running, that temporary
Dockerfile, the Docker image that was built and the Docker container that ran
will be removed unless you use any of the flags that keep them around.

If you find the `diff` output to be not enough or you prefer looking at diffs
in your favorite code editor then you may want to use the `--save` flag so
you can diff the projects on your own. They will always be saved in the current
directory with a directory name of whatever `--framework` you passed in.

## 👀 About the Author

I'm a self taught developer and have been freelancing for the last ~20 years.
You can read about everything I've learned along the way on my site at
[https://nickjanetakis.com](https://nickjanetakis.com/). There's hundreds of
blog posts and some video courses.
