# Capistrano::Maintenance

## Installation

Add this line to your application's Gemfile:

    gem 'capistrano', github: 'capistrano/capistrano', branch: 'v3'
    gem 'capistrano-maintenance', github: "capistrano/capistrano-maintenance"


And then execute:

    $ bundle

Or install it yourself as:

    $ gem install capistrano-maintenance

## Usage

Before using maintenance tasks, you need no configure webserver.
Here is an example config for nginx:

```
if (-f $document_root/system/maintenance.html) {
  return 503;
}

location @503 {
  # Serve static assets if found.
  if (-f $request_filename) {
    break;
  }

  rewrite ^(.*)$ /system/maintenance.html break;
}
```

Now you can require the gem in `Capfile`:

    require 'capistrano/maintenance'

### Enable task

Present a maintenance page to visitors. Disables your application's web
interface by writing a "#{maintenance_basename}.html" file to each web server. The
servers must be configured to detect the presence of this file, and if
it is present, always display it instead of performing the request.

By default, the maintenance page will just say the site is down for
"maintenance", and will be back "shortly", but you can customize the
page by specifying the REASON and UNTIL environment variables:

  $ cap maintenance:enable \\
        REASON="hardware upgrade" \\
        UNTIL="12pm Central Time"

You can use a different template for the maintenance page by setting the
`:maintenance_template_path` variable in your deploy.rb file. The template file
should either be a plaintext or an erb file.

Further customization will require that you write your own task.

### Disable task

    cap maintenance:disable

Makes the application web-accessible again. Removes the
"#{maintenance_basename}.html" page generated by maintenance:disable, which (if your
web servers are configured correctly) will make your application web-accessible again.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
