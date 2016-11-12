"Openstruct is ruby's javascript object"

Why use openstruct?
  - Consume an API
  - Configuration object
  - Simple test double

```ruby
class APICall < OpenStruct
  def self.execute
    response = Net::HTTP.get(uri...)
    new(JSON.parse(response))
  end
end
results = APICall.execute
```

Thinking of data as an object

```ruby
class User < OpenStruct
  def name
    "#first_name} #{last_name}"
  end
end

api_call = JSON.parse(API.get('/users/1'))
User.new(api_call).name
```

Configuration Object

```ruby
MyGem.configure do |configuration|
  configuration.setting = true
end

class MyGem
  def self.configure
    yield configuration
  end

  def self.configuration
    @configuration ||= OpenStruct.new
  end
end
```

## Test double

```ruby
payment_gateway = OpenStruct.new(charge: :PAID)
assert something was added to payment_gateway, etc.
```

OpneStruct defines attribute getter/setter methods on the object's singleton class
i.e.

```ruby
foo = Object.new
def foo.bar
  "hello from bar"
end
foo.bar # "hello from bar"
```

Hierarchical method cache
jamesgolic.com/2013/4/14/mris-method-cache
Ariel Caplan
@amcaplan
amcaplan.ninja
Works for Vitals


How he profiled

```ruby
gem 'stackprof'

StackProf.run(mode: :cup, out: 'tmp/stackprof.dump') do
  # block to test
end

stackprof tmp/stackprof.dump --text
```

##

OpenFastStruct
Overwrites method messing, bypasses defining methods (slow)

PersistentOpenStruct

```ruby
class Animal < PersistentOpenStruct
  def speak
    "The #{type} makes a #{sound} sound!"
  end
end

dog = Animal.new(type: 'dog', sound: :woof)

Animal.methods => [type, type=, etc.]
```

Defines methods on the *Class*, so further things that inherit don't have to redefine those methods (expensive!)

Benchmarking

```ruby
require 'benchmark/ips'
Benchmark.ips do |x|
  x.report('old code') do
    # old code block
  end

  x.report('new code') do
    # new code block
  end

  x.report
end
```

OpenStruct
  - Define OpenStruct attributes lazily (dramatic improvement when some keys are never accessed)

DynamicClass

```ruby
Animal = DynamicClass.new do
  def speak
    "The #{type} makes a #{sound} sound!"
  end
end

def add_methods@!(ey)
   class_exec do
    attr_writer key unless method defined
    ...
   end
end
```
