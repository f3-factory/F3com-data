# Web
The Websocket class can run a RFC6455 compatible websocket server.

Namespace: `\CLI\` <br>
File location: `lib/cli/ws.php`

---

### Basic Usage For Websocket Server
First you will need to create a file that you can run from the command line or through another program such as supervisord.
```php
<?php
	/**
	*	Execute the following command from a terminal to run the WebSocket server
	*	as a background service
	*
	*	php -q /path/to/websocket_test.php [ >> /path/to/rfc6455.log ] & disown
	**/

	/**
	*	Simple console logger
	*	@return NULL
	*	@param $line string
	**/
	function trace($line) {
		echo "\r".date('H:i:s').' ['.memory_get_usage(TRUE).'] '.$line.PHP_EOL;
	}
	if (PHP_SAPI != 'cli') {
		// Prohibit direct HTTP access
		header('HTTP/1.1 404 Not Found');
		die;
	}

	chdir(__DIR__);
	ini_set('default_socket_timeout',3);
	require __DIR__.'/vendor/autoload.php';
	$fw=\Base::instance();
	error_reporting(
		(E_ALL|E_STRICT)&~(E_NOTICE|E_USER_NOTICE|E_WARNING|E_USER_WARNING)
	);
	$fw->DEBUG = 2;
	$fw->ONERROR = function($fw) {
		trace($fw->get('ERROR.text'));
		foreach (explode("\n",trim($fw->get('ERROR.trace'))) as $line)
			trace($line);
	};
	// Instantiate the server
	$ws = new CLI\WS('tcp://0.0.0.0:9000');
	/* If you need a secure WebSocket server:
	$ws = new CLI\WS(
		'ssl://0.0.0.0:9000',
		stream_context_create([
			'ssl'=>[
				'local_cert' => '/path/to/.pem',
				'verify_peer' => FALSE,
				'verify_peer_name' => FALSE,
				'allow_self_signed' => TRUE
			]
		])
	);
	*/
	$ws->
		on('start',function($server) use($fw) {
			trace('WebSocket server started');
			$fw->write('tmp/ws.log','start'.PHP_EOL);
		})->
		on('error',function($server) use($fw) {
			if ($err=socket_last_error()) {
				trace(socket_strerror($err));
				socket_clear_error();
			}
			if ($err=error_get_last())
				trace($err['message']);
		})->
		on('stop',function($server) use($fw) {
			trace('Shutting down');
			$fw->write('tmp/ws.log','stop'.PHP_EOL,TRUE);
		})->
		on('connect',function($agent) use($fw) {
			trace(
				'(0x00'.$agent->uri().') '.$agent->id().' connected '.
				'<'.(count($agent->server()->agents())+1).'>'
			);
			$fw->write('tmp/ws.log','connect'.PHP_EOL,TRUE);
		})->
		on('disconnect',function($agent) use($fw) {
			trace('(0x08'.$agent->uri().') '.$agent->id().' disconnected');
			if ($err=socket_last_error()) {
				trace(socket_strerror($err));
				socket_clear_error();
			}
			$fw->write('tmp/ws.log','disconnect'.PHP_EOL,TRUE);
		})->
		on('idle',function($agent) use($fw) {
			$fw->write('tmp/ws.log','idle'.PHP_EOL,TRUE);
		})->
		on('receive',function($agent,$op,$data) use($fw) {
			switch($op) {
			case CLI\WS::Pong:
				$text='pong';
				break;
			case CLI\WS::Text:
				$data=trim($data);
			case CLI\WS::Binary:
				$text='data';
				break;
			}
			trace(
				'(0x'.str_pad(dechex($op),2,'0',STR_PAD_LEFT).
				$agent->uri().') '.$agent->id().' '.$text.' received'
			);
			if ($op==CLI\WS::Text && $data) {
				$in=json_decode($data,TRUE);
				if($in == 'ping')
					$out = json_encode('pong');
				else	
					$out = json_encode('not a ping');

				// this will propagate the change to all agents connected to the websocket server.
				foreach($agent->server()->agents() as $agent) {
					$agent->send(CLI\WS::Text,$out);
				}
				$fw->write('tmp/ws.log','receive'.PHP_EOL,TRUE);
			}
		})->
		on('send',function($agent,$op,$data) use($fw) {
			switch($op) {
			case CLI\WS::Ping:
				$text='ping';
				break;
			case CLI\WS::Text:
				$data=trim($data);
			case CLI\WS::Binary:
				$text='data';
				break;
			}
			trace(
				'(0x'.str_pad(dechex($op),2,'0',STR_PAD_LEFT).
				$agent->uri().') '.$agent->id().' '.$text.' sent'
			);
			if ($op==CLI\WS::Text) {
				$out=json_decode($data,TRUE);
				$fw->write('tmp/ws.log','send'.PHP_EOL,TRUE);
			}
		})->
		run();
```

### Basic HTML Page Example
This is an example page where you can see how the messages are sent and received.
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>WebSocket Test</title>
	</head>
	<body>
		<h2>WebSocket Test</h2>

		<div id="output"></div>
		<script language="javascript" type="text/javascript">

			var wsUri = "ws://0.0.0.0:9000/";
			var output;

			function init()
			{
				output = document.getElementById("output");
				testWebSocket();
			}

			function testWebSocket()
			{
				websocket = new WebSocket(wsUri);
				websocket.onopen = function(evt) { onOpen(evt) };
				websocket.onclose = function(evt) { onClose(evt) };
				websocket.onmessage = function(evt) { onMessage(evt) };
				websocket.onerror = function(evt) { onError(evt) };
			}

			function onOpen(evt)
			{
				writeToScreen("CONNECTED");
				//doSend("ping");
			}

			function onClose(evt)
			{
				writeToScreen("DISCONNECTED");
			}

			function onMessage(evt)
			{
				console.log(evt);
				writeToScreen('<span style="color: blue;">RESPONSE: ' + evt.data+'</span>');
				//websocket.close();
			}

			function onError(evt)
			{
				writeToScreen('<span style="color: red;">ERROR:</span> ' + evt.data);
			}

			function doSend(message)
			{
				writeToScreen("SENT: " + message);
				websocket.send(JSON.stringify(message));
			}

			function writeToScreen(message)
			{
				var pre = document.createElement("p");
				pre.style.wordWrap = "break-word";
				pre.innerHTML = message;
				output.appendChild(pre);
			}

			window.addEventListener("load", init, false);

		</script>
	</body>
</html>
```