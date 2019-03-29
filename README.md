# Console

Provides beautiful console logging for Ruby applications. Implements fast, buffered log output.

[![Build Status](https://travis-ci.com/socketry/console.svg)](http://travis-ci.com/socketry/console)
[![Coverage Status](https://coveralls.io/repos/socketry/console/badge.svg)](https://coveralls.io/r/socketry/console)

## Motivation

When Ruby decided to reverse the order of exception backtraces, I finally gave up using the built in logging and decided restore sanity to the output of my programs once and for all!

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'console'
```

And then execute:

	$ bundle

## Usage

As your code executes, it generates interesting events which you want to know about. The general approach is to use an `Console::Logger` which outputs text to the terminal. The text is generated by inspecting the console that occurred.

Capturing structured information allows it to be used in different ways. These events can be sent to a logger or some other system (e.g. web browser, syslog) and analysed in more detail.

### Default Logger

Generally speaking, use `Console.logger` which is suitable for logging to the user's terminal.

### Module Integration

```ruby
require 'console'

# Set the log level:
Console.logger.debug!

module MyModule
	extend Console
	
	def self.test_logger
		logger.debug "GOTO LINE 1."
		logger.info "5 things your doctor won't tell you!"
		logger.warn "Something didn't work as expected!"
		logger.error "The matrix has two cats!"
	end
	
	test_logger
end
```

### Class Integration

```ruby
require 'console'

# Set the log level:
Console.logger.debug!

class MyObject
	include Console

	def test_logger
		logger.debug "GOTO LINE 1."
		logger.info "5 things your doctor won't tell you!"
		logger.warn "Something didn't work as expected!"
		logger.error "The matrix has two cats!"
	end
end

MyObject.new.test_logger
```

### Console Formatting

Console classes are used to wrap data which can generate structured log messages:

```ruby
require 'console'

class MyConsole < Console::Generic
	def format_console(output, terminal, verbose)
		output.puts "My console text!"
	end
end

Console.logger.info("My Console", MyConsole.new)
```

#### Error Events

`Console::Error` represents an error and will log the message and backtrace recursively.

#### Shell Events

`Console::Shell` represents the execution of a shell command, and will log the environment, arguments and options used to execute it.

### Multiple Loggers

### Custom Log Levels

`Console::Filter` implements support for multiple log levels.

```ruby
require 'console'

MyLogger = Console::Filter[noise: 0, stuff: 1, broken: 2]

logger = MyLogger.new(Console.logger, name: "Java")
logger.verbose! # log severity/name/pid etc.

logger.broken("It's so janky.")
```

### Multiple Outputs

```ruby
require 'console/terminal'
require 'console/serialized/logger'
require 'console/logger'
require 'console/split'

terminal = Console::Terminal::Logger.new
file = Console::Serialized::Logger.new(File.open("/tmp/log.json", "w"))

logger = Console::Logger.new(Console::Split[terminal, file])

logger.info "I can go everywhere!"
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License

Released under the MIT license.

Copyright, 2019, by [Samuel Williams](https://www.codeotaku.com).

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
