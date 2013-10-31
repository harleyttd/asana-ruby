# Asana

This gem is a simple Ruby wrapper for the Asana REST API. It's based on Ryan Bright's [asana gem](https://github.com/rbright/asana) but it handles a few things differently and it supports more API calls, such as tag handling.

It uses
[ActiveResource][] to provide a simple, familiar interface for accessing
your Asana account.

To learn more, check out the [Asana API Documentation][].

## Installation

Add this line to your application's Gemfile:

    gem 'asana-ruby'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install asana-ruby

## Usage

In order to access Asana, you need to provide your [API key][].


```ruby
Asana.configure do |client|
  client.api_key = 'your_asana_api_key'
end
```

As a sanity check, you can fetch the object representing your user.

```ruby
Asana::User.me
```

### [Users][]

> A user object represents an account in Asana that can be given access to
> various workspaces, projects, and tasks.
>
> Like other objects in the system, users are referred to by numerical IDs.
> However, the special string identifier me can be used anywhere a user ID is
> accepted, to refer to the current authenticated user.

**Note:** It is not possible to create, update, or delete a user from
the API.

```ruby
# Get users from all of your workspaces
users = Asana::User.all

# Get a specific user
user = Asana::User.find(:user_id)

# Get the user associated with the API key being used
user = Asana::User.me

# Get all users in a given workspace
workspace = Asana::Workspace.find(:workspace_id)
users = workspace.users
```

### [Workspaces][]

> A workspace is the most basic organizational unit in Asana. All projects
> and tasks have an associated workspace.

**Note:** It is not possible to create or delete a workspace from the API.

```ruby
# Get all workspaces
workspaces = Asana::Workspace.all

# Get a specific workspace
workspace = Asana::Workspace.find(:workspace_id)

# Get all projects in a given workspace
projects = workspace.projects

# Get all tasks in a given workspace that are assigned to the given user
tasks = workspace.tasks(:user_id)

# Get all users with access to a given workspace
users = workspace.users

# Create a new task in a given workspace and assign it to the current user
workspace.create_task(:name => 'Get milk from the grocery store')
```

### [Projects][]

> A project represents a prioritized list of tasks in Asana. It exists in a
> single workspace and is accessible to a subset of users in that workspace
> depending on its permissions.

**Note:** It is not possible to create or delete a project from the API.

```ruby
# Get all projects
projects = Asana::Project.all

# Get a specific project
project = Asana::Project.find(:project_id)

# Get all projects in a given workspace
workspace = Asana::Workspace.find(:workspace_id)
projects = workspace.projects
```

### [Tasks][]

> The task is the basic object around which many operations in Asana are
> centered. In the Asana application, multiple tasks populate the middle
> pane according to some view parameters, and the set of selected tasks
> determine the more detailed information presented in the details pane.

```ruby
# Get all tasks in a given project
project = Asana::Project.find(:project_id)
tasks = project.tasks

# Get all tasks in a given workspace
workspace = Asana::Workspace.find(:workspace_id)
tasks = workspace.tasks

# Get all stories for a given task
task = tasks.first
stories = task.stories

# Create a new task in a given workspace and assign it to the current user
workspace.create_task(:name => 'Get milk from the grocery store')

# Create a new story for the given task
task.create_story(story_settings)
```

### [Stories][]

> A story represents an activity associated with an object in the Asana
> system. Stories are generated by the system whenever users take actions
> such as creating or assigning tasks, or moving tasks between projects.
> Comments are also a form of user-generated story.

**Note:** It is not possible to update or delete a story from the API.

```ruby
# Get all stories for a given task
project = Asana::Project.find(:project_id)
task = project.tasks.first
stories = task.stories

# Get a specific story
story = Story.find(:story_id)

# Create a new story for the given task
task.create_story(story_settings)
```

### [Tags][]

> A tag is a label that can be attached to any task in Asana. It exists in a
> single workspace or organization.

```ruby
# Get a specific tag
tag = Asana::Tag.find(:tag_id)

# Get all tasks that have that specific tag
tasks = tag.tasks
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

[API key]: http://app.asana.com/-/account_api
[ActiveResource]: http://api.rubyonrails.org/classes/ActiveResource/Base.html
[Asana API Documentation]: http://developer.asana.com/documentation/
[Users]: http://developer.asana.com/documentation/#users
[Workspaces]: http://developer.asana.com/documentation/#workspaces
[Projects]: http://developer.asana.com/documentation/#projects
[Tasks]: http://developer.asana.com/documentation/#tasks
[Stories]: http://developer.asana.com/documentation/#stories
[Tags]: http://developer.asana.com/documentation/#tags
