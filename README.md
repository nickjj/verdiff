# Verdiff [![Build Status](https://travis-ci.org/nickjj/verdiff.svg?branch=master)](http://travis-ci.org/nickjj/verdiff)

A command line script that will show you exactly what changed between 2
versions of your favorite web framework.

It currently supports Phoenix, Rails, Django and Laravel.

*Verdiff is a zero dependency 3.6+ compatible script that only requires having
Docker installed.*

## Sample Output

Running this command:

```
nick:~$ verdiff --framework phoenix 1.4.4 1.4.8
```

Produces this output:

```diff
--- /app/phoenix/1.4.4/a1561480494/assets/webpack.config.js     2019-06-25 16:35:00.000000000 +0000
+++ /app/phoenix/1.4.8/a1561480494/assets/webpack.config.js     2019-06-25 16:35:02.000000000 +0000
@@ -13,7 +13,7 @@
     ]
   },
   entry: {
-      './js/app.js': ['./js/app.js'].concat(glob.sync('./vendor/**/*.js'))
+    './js/app.js': glob.sync('./vendor/**/*.js').concat(['./js/app.js'])
   },
   output: {
     filename: 'app.js',
diff --color -u --exclude vendor -r /app/phoenix/1.4.4/a1561480494/config/config.exs /app/phoenix/1.4.8/a1561480494/config/config.exs
--- /app/phoenix/1.4.4/a1561480494/config/config.exs    2019-06-25 16:34:59.000000000 +0000
+++ /app/phoenix/1.4.8/a1561480494/config/config.exs    2019-06-25 16:35:02.000000000 +0000
@@ -13,7 +13,7 @@
 # Configures the endpoint
 config :a1561480494, A1561480494Web.Endpoint,
   url: [host: "localhost"],
-  secret_key_base: "cjIVgFpmM8L2HEGUDhECpKUNc/YFfu0cNIYsF5FrjS7G6VYCSzoO/ZLsuLc2AEKA",
+  secret_key_base: "SBIuyXenXwyX4CIGrLt3sfUMAz17jXLlfbxVm5zgCLVywkGMD+aJsmU4SXLFM+7c",
   render_errors: [view: A1561480494Web.ErrorView, accepts: ~w(html json)],
   pubsub: [name: A1561480494.PubSub, adapter: Phoenix.PubSub.PG2]

diff --color -u --exclude vendor -r /app/phoenix/1.4.4/a1561480494/lib/a1561480494_web/endpoint.ex /app/phoenix/1.4.8/a1561480494/lib/a1561480494_web/endpoint.ex
--- /app/phoenix/1.4.4/a1561480494/lib/a1561480494_web/endpoint.ex      2019-06-25 16:35:00.000000000 +0000
+++ /app/phoenix/1.4.8/a1561480494/lib/a1561480494_web/endpoint.ex      2019-06-25 16:35:02.000000000 +0000
@@ -24,7 +24,7 @@
   end

   plug Plug.RequestId
-  plug Plug.Logger
+  plug Plug.Telemetry, event_prefix: [:phoenix, :endpoint]

   plug Plug.Parsers,
     parsers: [:urlencoded, :multipart, :json],
@@ -40,7 +40,7 @@
   plug Plug.Session,
     store: :cookie,
     key: "_a1561480494_key",
-    signing_salt: "NLultLKg"
+    signing_salt: "xzZBCVLE"

   plug A1561480494Web.Router
 end
diff --color -u --exclude vendor -r /app/phoenix/1.4.4/a1561480494/mix.exs /app/phoenix/1.4.8/a1561480494/mix.exs
--- /app/phoenix/1.4.4/a1561480494/mix.exs      2019-06-25 16:35:00.000000000 +0000
+++ /app/phoenix/1.4.8/a1561480494/mix.exs      2019-06-25 16:35:02.000000000 +0000
@@ -33,7 +33,7 @@
   # Type `mix help deps` for examples and options.
   defp deps do
     [
-      {:phoenix, "~> 1.4.4"},
+      {:phoenix, "~> 1.4.7"},
       {:phoenix_pubsub, "~> 1.1"},
       {:phoenix_ecto, "~> 4.0"},
       {:ecto_sql, "~> 3.0"},
diff --color -u --exclude vendor -r /app/phoenix/1.4.4/a1561480494/test/support/data_case.ex /app/phoenix/1.4.8/a1561480494/test/support/data_case.ex
--- /app/phoenix/1.4.4/a1561480494/test/support/data_case.ex    2019-06-25 16:35:00.000000000 +0000
+++ /app/phoenix/1.4.8/a1561480494/test/support/data_case.ex    2019-06-25 16:35:02.000000000 +0000
@@ -45,8 +45,8 @@
   """
   def errors_on(changeset) do
     Ecto.Changeset.traverse_errors(changeset, fn {message, opts} ->
-      Enum.reduce(opts, message, fn {key, value}, acc ->
-        String.replace(acc, "%{#{key}}", to_string(value))
+      Regex.replace(~r"%{(\w+)}", message, fn _, key ->
+        opts |> Keyword.get(String.to_existing_atom(key), key) |> to_string()
       end)
     end)
   end
```

## But Why?

A lot of popular web frameworks allow you to generate a new project with a
command line tool. For example with Phoenix you can run `mix phx.new hello` and
with Rails you can run `rails new hello`.

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

- Launch a Docker container for XYZ language (let's say Elixir)
- Install my project's current version of Phoenix (a web framework for Elixir)
- Generate project A in a directory
- Install the version of Phoenix I want to upgrade to
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
and I do not have Elixir or Ruby installed directly on my dev box.

So when it comes to creating a brand new project, I would typically launch a
Docker container for Elixir, install the latest version of Phoenix, generate
my project and through the power of volume mounts I would end up with a new
project on my dev box.

That's a tedious workflow and it can be avoided. Verdiff supports an optional
`--save` flag so you can persist the generated projects to your dev box, which
means this tool doubles as a way to quickly generate brand new projects too.

## Installation

Since this project uses Docker you'll need to install Docker. If you're not
sure what Docker is or why it's useful, I wrote over 100+ blog posts about
Docker which you may want to check out [on my
website](https://nickjanetakis.com/blog/tag/docker-tips-tricks-and-tutorials).

The next step is to download the `verdiff` script, make sure it's executable
and place it somewhere on your system path.

#### 1 liner to get `verdiff` downloaded to `/usr/local/bin`:

```sh
sudo curl \
  -L https://raw.githubusercontent.com/nickjj/verdiff/master/verdiff \
  -o /usr/local/bin/verdiff && sudo chmod +x /usr/local/bin/verdiff
```

You can confirm it works by running `verdiff --help`.

## Usage Examples

You can run `verdiff` from anywhere you want but keep in mind it will
temporarily write out a custom Dockerfile in the current directory, but this
file has both a project name and time stamp in its file name so there's a very
good chance it won't conflict with any Dockerfile you have in the current
directory.

If you run `verdiff --help` you can see exactly what's available but here's a
few usage examples that demonstrate using every available flag and option.

```sh
# 1. Compare version 5.1.2 and 5.2.3 of a new Rails project
verdiff --framework rails 5.1.2 5.2.3

# 1. Compare version 1.4.4 and 1.4.8 of a new Phoenix project (the default --framework)
# 2. Supply a custom project name for the generated projects
# 3. Persist the projects to your dev box in a phoenix/ folder in the current directory
verdiff 1.4.4 1.4.8 --project hello --save

# 1. Compare version 2.1.9 and 2.2.2 of a new Django project
# 2. Set DEBUG=1 to see the Dockerfile that got used and all Docker commands that were run
DEBUG=1 verdiff --framework django 2.1.9 2.2.2 --save

# 1. Compare version 5.6.33 and 5.8.17 of a new Laravel project
# 2. Keep the Docker image around after it completes in case you want to check it out
# 3. Keep the Dockerfile around in the current directory in case you want to check it out
verdiff --framework laravel 5.6.33 5.8.17 --keep-image --keep-dockerfile
```

It's also worth mentioning that once `verdiff` finishes running, that temporary
Dockerfile, the Docker image that was built and the Docker container that ran
will be removed unless you use any of the flags that keep them around.

If you find the `diff` output to be not enough or you prefer looking at diffs
in your favorite code editor then you may want to use the `--save` flag so
you can diff the projects on your own. They will always be saved in the current
directory with a directory name of whatever `--framework` you passed in.

## About the Author

I'm a self taught developer and have been freelancing for the last ~20 years.
You can read about everything I've learned along the way on my site at
[https://nickjanetakis.com](https://nickjanetakis.com/). There's hundreds of
blog posts and some video courses.
