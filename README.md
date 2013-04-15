Overview
===
Simplifies the process of dealing with RETS servers. Without having to worry about the various authentication methods or edge cases associated with dealing with RETS. Should work against all 1.x implementations.

Compability
-
Tested against Ruby 1.9.3, 2.0.0, RBX and JRuby, build history is available [here](http://travis-ci.org/Placester/ruby-rets).

<img src="https://secure.travis-ci.org/Placester/ruby-rets.png?branch=master&.png"/>

Documentation
-
See http://rubydoc.info/github/Placester/ruby-rets/master/frames for full documentation.

Examples
-

    client = RETS::Client.login(:url => "http://foobar.com/rets/Login", :username => "foo", :password => "bar")
    client.search(:search_type => :Property, :class => :RES, :query => "(ListPrice=50000-)") do |data|
      # RETS data in key/value format, as COMPACT-DECODED
    end

    client.get_object(:resource => :Property, :type => :Photo, :location => false, :id => "1:0:*") do |headers, content|
      puts "Object-ID #{headers"object-id"]}, Content-ID #{headers["content-id"]}, Description #{["description"]}"
      puts "Data"
      puts content
    end

VCR / WebMock
-
Due to the streaming parser, the search features won't work with a library like VCR or Ephemeral Response. For WebMock, you can use the below patch to enable support for saving the HTTP requests to speed up your own tests.

    module Net
        module WebMockHTTPResponse
            def self.extended(response)
                response.instance_variable_set(:@socket, StringIO.new(response.body))
            end
        end
    end

License
-
Licensed under MIT
