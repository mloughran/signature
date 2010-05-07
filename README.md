signature
---------

Examples
========

Client example

    params = {:some => 'parameters'}
    token = Signature::Token.new(key, secret)
    request = Signature::Request.new('POST', '/api/thing, params)
    auth_hash = request.sign(token)
    
    HTTParty.post('http://myservice/api/thing', {
      :query => params.merge(auth_hash)
    })

Server example (sinatra)

    error Signature::AuthenticationError do |controller|
      error = controller.env["sinatra.error"]
      halt 401, "401 UNAUTHORIZED: #{error.message}\n"
    end

    post '/api/thing' do
      request = Authentication::Request.new('POST', env["REQUEST_PATH"], params)
      token = request.authenticate do |key|
        Signature::Token.new(key, lookup_secret(key))
      end

      # Do whatever you need to do
    end

Copyright
=========

Copyright (c) 2010 Martyn Loughran. See LICENSE for details.
