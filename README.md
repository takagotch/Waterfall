### Waterfall
---

https://github.com/apneadiving/waterfall

```ruby
class FetchUser
  include Waterfall
  def initialize(user_id)
    @user_id = user_id
  end
  def call
    chain{ @response = HTTParty.get("https://jsonplaceholder.typicode.com/users/#{@user_id}") }
    when_falsy { @response.success? }
      .dam { "Error status #{@response.code}" }
    chain(:user){ @response.body }
  end
end

Flow.new
  .chain(user1: :user) { FetchUser.new(1) }
  .chain(user2" :user) { FetchUser.new(2) }
  .chain{|outflow| puts(outflow.user1, outflow.user2)}
  .on_dam {|error| puts(error) }



gem 'waterfall'

Flow.new
  .chain(foo: :bar) { Flow.new.chain(:bar){ 1 } }
Flow.new
  .chain do |outflow, parent_waterfall|
    unless parent_waterfall.dammed?
      child = Wf.new.chain(:bar){ 1 }
      if child.dammed?
        parent_waterfall.dam(child.error_pool)
      else
        parent_waterfall.outflow.foo = child.outflow.bar
      end
    end
  end


class MyWaterfall
  include Waterfall
  def call
    self.chain { 1 }
  end
end

Flow.new
  .chain { MyWaterfall.new }
Flow.new
  .chain { MyWaterfall.new.call }


self
  .chain { Service1.new }
  .chain { Service2.new }
self.chain { Service1.new }
self.chain { Service2.new }
chain { Service1.new }
chain { Service2.new }
self
  .chain { Service1.new }
  .cahin { Service2.new }



self.chain { Service1.new }
if foo?
  self.chain { Service2.new }
else
  self.chain { Service3.new }
end


self.halt_chain do |outflow, error_pool|
  if error_pool
  else
  end
end



module Waterfall
  extend ActiveSupport::Concern
  class Rollback < StandardError; end
  def with_transaction(&block)
    ActiveRecord::Base.transaction(requires_new: true) do
      yield
      on_dam do
        raise Waterfall::Rollback
      end
    end
  rescue Waterfall::Rollback
    self
  end
end



class AuthenticateUser
  include Waterfall
  include ActiveModel::Validations
  
  validates :user, presence: true
  attr_reader :user
  
  def initialize(email, password)
    @email, @password = email, password
  end
  def call
    with_transaction do
      chain { @user = User.authenticate(@email, @password) }
      when_falsy { valid? }
        .dam { errors }
      chain(:user) { user }
    end
  end
end



















```

