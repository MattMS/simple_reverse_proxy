# Simple reverse proxy

	http = require 'http'
	request = require 'request'

The proxies JSON file is given as the first argument.

	proxy_details = require process.argv[0]

	proxies = proxy_details.proxies

	server = http.createServer (req, res)->
		req_path = req.url.slice 1

Loop each of the proxy definitions from the JSON file.

		for proxy in proxies
			match = proxy.re.exec req_path

			if match
				return req.pipe request "#{proxy.base}/#{match[1]}"
				.pipe res

Show a 404 error page if none of the proxies match the request.

		console.log "404: #{req.url}"
		res.writeHead 404
		res.end()

	port = proxy_details.port

	console.log "Reverse proxy started on port #{port}."

	server.listen port
