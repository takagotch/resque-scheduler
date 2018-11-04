### resque-scheduler
---
https://github.com/resque/resque-scheduler

```ruby
Resque.enqueue_in(5.days, SendFollowupEmail, argument)
Resque.enquue_at(5.days.from_now, SomeJob, argument)

require 'resque/scheduler/tasks'

require 'resque/tasks'
require 'resque/scheduler/tasks'
namespace :resque do
  task :setup do
    require 'resque'
    Resque.redis = 'localhost:6379'
  end
  task :setup_schedule => :setup do
    require 'resque-scheduler'
    # Resque::Scheduler.dynamic = true
    Resque.schedule = YAML.load_file('your_resque_schedule.yml')
    require 'jobs'
  end
  task :scheduler => :setup_schedule
end

task 'resque:pool:setup' do
  Resque::Pool.after_prefork do |job|
    Resque.redis._client.reconnect
  end
end

Resque.enqueue_in(
  5.days,
  SendFollowUpEmail,
  user_id: current_user.id
)

Resque.enqueue_at_with_queue(
  'queue_name',
  5.days.from_now,
  SendFollowUpEmail,
  user_id: current_user.id
)

Resque.enqueue_at(5.days.from_now, SendFollowUpEmail, :user_id => current_user.id)
Resque.remove_delayed(SendFollowedUpEmail, :user_id => current_user.id)

Resque.enqueue_at(5.days.from_now, SendFollowUpEmail, :account_id => current_account.id, :user_id => current_user.id)
Resque.remove_delayed_selection { |args| args[0]['account_id'] == current_account.id }
Resque.remove_delayed_selection { |args| args[0]['user_id'] == current_user.id }

Resque.enqueue_at()
Resque.remove_delayed_selection() {}
Resque.remove_delayed_selection() {}

Resque.enqueue_at()
Resque.enqueue_delayed_selection {}
Resque.enquque_delayed_selection {}

Resque.schedule = YAML.load_file('your_resque_schedule.yml')

Resque::Scheduler.dynamic = true

name = 'send_emails'
config = {}
config[:class] = 'SendEmail'
config[:args] = 'POC email subject'
config[:every] = ['', {first_in: 5.minutes}]
config[:persist] = true
Resque.set_schedule(name, config)

name = 'send_emails'
Resque.remove_schedule(name)

name = 'send_email'
config = {}
config[:class] = 'SendEmail'
config[:args] = 'POC email subject'
config[:every] = '1d'
Resque.set_schedule(name, config)

cron: ""

ActiveSupport::TimeZone.find_tzinfo(Rails.configuration.time_zone).name

Resque::Scheduler.failure_handler = ExceptionHandlerClass

class FakeLeaderboard < Resque::JobWithStatus
  def perform
  end
end

module Resque
  class JobWithStatus
    def self.scheduled(queue, klass, *args)
      create(*args)
    end
  end
end

require 'resque'
Resque.redis = "redis_server:6379"

require 'resque-scheduler'
require 'resque/scheduler/server'

Resque.schedule = YAML.load_file(File.join(RAILS_ROOT, 'config/resque_schedule.yml'))

Resque::Scheduler.configure do |c|
  c.quiet = false
  c.verbose = false
  c.logfile = nil
  c.logformat = 'text'
end

```

```
gem install resque-scheduler
gem 'resque-scheduler'

rake resque:scheduler
rake environment resque:scheduler

resque-scheduler --help

resque-web ~/yourapp/config/resque-config.rb

PIDFILE=./resque-scheduler.pid BACKGROUND=yes \
  rake resque:scheduler

RESQUE_SCHEDULER_INTERVAL=1 rake resque:scheduler

bundle install
bundle exec rake

vagrant up

```


```
CancelAbandoneOrders:
  cron: ""
queue_documents_for_indexing:
  cron: ""
  class: ""
  queue: high
  args:
  description: ""
clear_leaderboards_contributions:
  cron: ""
  class: ""
  queue: contributors
  description: ""
  
ImportOrdersManual:
  description: ''
  custom_job_class: ''
  never: ""
  queue: high
  description: ""
  args:
    days_in_arrears: 7
 
clear_leaderboards_moderator:
  every:
    - ""
    - :first_in: ''
  class: ""
  queue: daemons
  description: ""
  
create_fake_leaderboards:
  cron: ""
  queue: scoring
  custom_job_class: ""
  args:
  rails_env: demo
  description: ""
  
```
