# Ruby Tutorial

**Disclaimer:** I don't consider myself an expert of Ruby. I am an eternal
student. But, what I describe here should be basic enough to get people going
and hopefully be a reference site. I also assume audience has some prior
programming knowledge.

## Getting Ready

### Install Ruby

**Mac Users**

Best way is to use RVM to install Ruby. If you need to specify Ruby version
number, do so by adding the version like this: `--ruby=1.9.3`.

`\curl -sSL https://get.rvm.io | bash -s stable --ruby=1.9.3`

**Windows Users**

http://rubyinstaller.org/downloads/

### irb

This is your best friend. Use it to test and try different things. When you
develop in Ruby on Rails, rails console will give you the same feel and it
will also give you access to all classes in your code.

```ruby
$ irb
1.9.3-p194 :001 > puts "Hello World"
Hello World
 => nil
```

### Basic Ruby Idioms and Styleguide

https://github.com/styleguide/ruby

* Use came case for class names
* Use snake case for method and variable names

```ruby
class RubyClassName
  def this_is_method_name
    variable_name = "whatever"
  end
end
```

* Write expressive method names (Rubyist should be able to get a sense of what methods do without extensive documentation)

```ruby
# Bad
def number
end

# Good
def get_squared_number
end
```

* Use `?` at the end of method if it returns truthy or falsy value

```ruby
def is_even?(num)
  num % 2 == 0
end
```

* Ruby method return whatever was evaluated last, so for simple method, don't use `return` keyword.

```ruby
# Bad
def double_the_num(num)
  product = num**2
  return product
end

# Good
def double_the_num(num)
  num**2
end
```

## Basics

### Everything is an object

Literally, everything.

https://www.youtube.com/watch?v=F9GVKxSiQVM

```ruby
$ irb
1.9.3-p194 :002 > "string".class
 => String
1.9.3-p194 :003 > 1.class
 => Fixnum
1.9.3-p194 :004 > 1.5.class
 => Float
1.9.3-p194 :005 > [1,2,3].class
 => Array
1.9.3-p194 :006 > {}.class
 => Hash
1.9.3-p194 :013 > String.superclass
 => Object
1.9.3-p194 :014 > Object.superclass
 => BasicObject
1.9.3-p194 :015 > BasicObject.superclass
 => nil
1.9.3-p194 :016 > nil.class
 => NilClass
```

### Variables

Ruby variable names do not need any keyword prefix.

`variable = "normal variable"`

Except the following cases:

`$variable  = "global variable"`  : Global variable

`@@variable = "class variable"`   : Class variable

`@variable  = "instance variable"`: Instance variable

`VARIABLE   = "constants"`        : Constant variable

### Standard Output (STDOUT)

* `puts` adds a carriage return at the end
* `print` does not add a carriage return at the end
* `#{}` means evaluate what's inside `{}`.

```ruby
1.9.3-p194 :051 > print "Hello World"
Hello World => nil 
1.9.3-p194 :052 > puts "Hellow World"
Hellow World
 => nil 
1.9.3-p194 :053 > a = "Hellow World"
 => "Hellow World" 
1.9.3-p194 :054 > puts "#{a} is what I say"
Hellow World is what I say
 => nil 
```

### Basic Data Structures

#### Array

**1.9.3**: http://www.ruby-doc.org/core-1.9.3/Array.html

**2.1.1**: http://www.ruby-doc.org/core-2.1.1/Array.html

Mutable and can contain different types of data.

```ruby
1.9.3-p194 :017 > a = Array.new
 => []
1.9.3-p194 :019 > b = []
 => []
1.9.3-p194 :022 > b << "1"
 => ["1"]
1.9.3-p194 :023 > b << 2
 => ["1", 2]
1.9.3-p194 :024 > b.push(:c => "d")
 => ["1", 2, {:c=>"d"}]
1.9.3-p194 :028 > b << {:d => "e"}
 => ["1", 2, {:c=>"d"}, {:d=>"e"}]
1.9.3-p194 :029 > b.pop
 => {:d=>"e"}
1.9.3-p194 :030 > b
 => ["1", 2, {:c=>"d"}]


1.9.3-p194 :031 > b.length
 => 3
1.9.3-p194 :032 > b.first
 => "1"
1.9.3-p194 :033 > b.last
 => {:c=>"d"}
1.9.3-p194 :034 > b[0]
 => "1"
1.9.3-p194 :035 > b[-1]
 => {:c=>"d"}  
```

#### Hash

**1.9.3**: http://www.ruby-doc.org/core-1.9.3/Hash.html

**2.1.1**: http://www.ruby-doc.org/core-2.1.1/Hash.html

`=>` is called **hash rocket**.

```ruby
1.9.3-p194 :036 > a = Hash.new
 => {}
1.9.3-p194 :037 > b = {}
 => {}
1.9.3-p194 :039 > c = Hash.new([])
 => {}
1.9.3-p194 :040 > a[:a]
 => nil
1.9.3-p194 :041 > a[:a] = "a"
 => "a"
1.9.3-p194 :042 > a
 => {:a=>"a"}
1.9.3-p194 :043 > b[:a] = "b"
 => nil
1.9.3-p194 :045 > c[:a]
 => []
1.9.3-p194 :047 > c.size
 => 0
1.9.3-p194 :048 > a.size
 => 1
1.9.3-p194 :049 > a[:a]
 => "a"
```

### Basic Control Structure

#### If, elsif, else

```ruby
if condition_a
  do_something
elsif condition_b
  do_something_else
else
  do_yet_another
end
```

But if you only care about one condition use single line.

`do_something if condition_a`

#### Unless

Don't use unless if there is no else condition. Use if instead. It's more readable that way.

`do_something_else unless condition_b`

### Iteration

Enumerable, 1.9.3: http://ruby-doc.org/core-1.9.3/Enumerable.html

Enumerable, 2.1.1: http://ruby-doc.org/core-2.1.1/Enumerable.html

In Ruby, most iterations happen over a collection of elements and you pass
**block** that will be iterated on each element. Enumerable mixin or module provides a lot more functionalities than standard Array/Hash methods. Enumerable methods work both on arrays and hashes. 

Use `do/end` if multiple lines are required. Use `{}` for single line.

```ruby
numbers = [1, 2, 3]

numbers.each do |num|
  puts num**2
  puts num
end

numbers.each { |num| puts num }

hash = {:a => "A", :b => "B"}

hash.each { |key, value| puts "#{hash[key]} = #{value}" }
```

### Working with HTTP request/response

Net::HTTP, 1.9.3: http://www.ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTP.html

JSON, 1.9.3     : http://ruby-doc.org/stdlib-1.9.3/libdoc/json/rdoc/JSON.html

Net::HTTP, 2.1.1: http://www.ruby-doc.org/stdlib-2.1.1/libdoc/net/http/rdoc/Net/HTTP.html

JSON, 2.1.1     : http://ruby-doc.org/stdlib-2.1.1/libdoc/json/rdoc/JSON.html

Net::HTTP, URI, and JSON are part of stanard libraries in Ruby. To use them, you need to `require` them first.

Examples can be found here: http://www.rubyinside.com/nethttp-cheat-sheet-2940.html

**GET**

```ruby
1.9.3-p194 :055 > require 'json'
 => true 
1.9.3-p194 :056 > require 'net/http'
 => true 
1.9.3-p194 :057 > require 'uri'
 => false 
1.9.3-p194 :065 > uri = URI.parse("http://www.google.com")
 => #<URI::HTTP:0x007fb80446efb0 URL:http://www.google.com> 
1.9.3-p194 :066 > response = Net::HTTP.get_response(uri)
 => #<Net::HTTPOK 200 OK readbody=true> 
1.9.3-p194 :067 > response.code
 => "200" 
1.9.3-p194 :068 > response.body
 # Buch of HTML codes
```

**POST**

Using form data is easy.

`response = Net::HTTP.post_form(uri, {"q" => "My query", "per_page" => "50"})`

Using JSON data in body requires more typing

```ruby
uri = URI.parse('https://myapp.com/api/v1/resource')
req = Net::HTTP::Post.new(uri, initheader = {'Content-Type' =>'application/json'})
req.body = {param1: 'some value', param2: 'some other value'}.to_json
response = Net::HTTP.start(uri.hostname, uri.port) do |http|
  http.request(req)
end
```
