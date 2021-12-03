# Simple Task Application with Author and Tasks

```
docker-compose run --no-deps web rails new . -T --force --database=postgresql
```

### Add gems

```
gem "bootstrap_form", "~> 5.0"

group :development, :test do
  gem 'rspec-rails', '~> 5.0.0'
  gem 'guard-rspec', require: false
  #...
end
```

### Preparing environment

database.yml

```yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password: password
  pool: 5

development:
  <<: *default
  database: docker_ror6_tasks_development

test:
  <<: *default
  database: docker_ror6_tasks_test
```

```
docker-compose run web rails generate rspec:install
docker-compose run web bundle exec guard init rspec
```

Guardfile

```
guard :rspec, cmd: 'bundle exec rspec' do
  watch('spec/spec_helper.rb')                        { "spec" }
  watch('config/routes.rb')                           { "spec/routing" }
  watch('app/controllers/application_controller.rb')  { "spec/controllers" }
  watch(%r{^spec/.+_spec\.rb$})
  watch(%r{^app/(.+)\.rb$})                           { |m| "spec/#{m[1]}_spec.rb" }
  watch(%r{^app/(.*)(\.erb|\.haml|\.slim)$})          { |m| "spec/#{m[1]}#{m[2]}_spec.rb" }
  watch(%r{^lib/(.+)\.rb$})                           { |m| "spec/lib/#{m[1]}_spec.rb" }
  watch(%r{^app/controllers/(.+)_(controller)\.rb$})  { |m| ["spec/routing/#{m[1]}_routing_spec.rb", "spec/#{m[2]}s/#{m[1]}_#{m[2]}_spec.rb", "spec/acceptance/#{m[1]}_spec.rb"] }
end
```

### Active Storage

```
docker-compose run web rails active_storage:install
```

and gem of him

```
gem 'image_processing', '~> 1.2'
```

### Rebuild

```
docker-compose build --no-cache
```

### Create database

```
docker-compose run web rails db:drop db:create db:create
```
