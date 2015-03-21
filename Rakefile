# Add your own tasks in files placed in lib/tasks ending in .rake,
# for example lib/tasks/capistrano.rake, and they will automatically be available to Rake.

require File.expand_path('../config/application', __FILE__)

Rails.application.load_tasks

namespace :fastly do
  desc "Purge all cached content from Fastly"
  task :purge do
    require_relative "./config/initializers/fastly"
    FastlyRails.service.purge_all
  end
end

namespace :stripe do
  desc "Send plans to Stripe"
  task :plans do
    require_relative "./config/initializers/stripe"
    current_ids = Stripe::Plan.all.map(&:id)

    MembershipPlan.all.values.
      reject{|plan| current_ids.include?(plan.id) }.
      each{|plan| Stripe::Plan.create(plan.to_h) }
  end
end
