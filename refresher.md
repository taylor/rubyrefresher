# The Ruby Refresher (Draft)

## Preface

This document aims to refresh one's memory of Ruby syntax, semantics, common
operations, and standard libraries as quickly and efficiently as possible. The
reader is expected to have some past experience with the Ruby language.

Newcomers to Ruby should read some of the
[beginner's documentation][] before
working through this refresher.

## Instructions

Each line is designed to introduce a new language rule, or reinforce a
previously established rule.

1. Read each line and try to evaluate it in your head.
2. If you can't figure it out, execute it in `irb`.
3. Think (for just a few seconds) about how it differs from the previous lines.
4. If you get confused at any point, look up the relevant documentation at
   [Ruby-Doc.org](http://www.ruby-doc.org/).


## Basics

    a = 1
    b = "boo"
    d = 8
    Constant_begins_with_caps = 20
    ANOTHER_CONSTANT = 20
    $global_variable = 30

    # This is a comment

    madman = "I have #{a + d} baboons"
    madman2 = "I also have %s baboons" % [a + d]

    puts "This string is printed with a newline"
    print "This string is printed without a newline"

    triple_x = "X" * 3

    print 'This string begins with a single quote.'
    puts 'In Ruby, the string "b = #{b}" evaluates to "b = boo"'

    valueless = nil
    valueless.nil?

    truthiness = true
    falsiness = false

    puts(truthiness == falsiness)

## Arrays

    primes = [2, 3, 5, 7]
    people = ["Bob", "Alice", "Eve"]

    puts "The first prime is " << primes[0].to_s
    puts "The last person is " << people[-1]

    puts "The first person is " << people.first
    puts "The last prime is " << primes.last
    primes.pop
    puts "The last prime is now " << primes.last
    primes.push 7
    puts "The last prime is #{primes.last} again"
    primes << 11
    puts "The last prime is now #{primes.last}"

    puts "There are #{ people.size } people"
    people.shift
    puts "There are now #{ people.length } people"
    people.unshift "Bob"
    puts "#{people.first}'s back!"

    sorted_people = people.sort
    puts "#{people.first} is still Bob"
    puts "#{sorted_people.first} is not Bob"
    people.sort!
    puts "#{people.first} is not Bob"

    puts primes[0...3]    # => [2, 3, 5]
    puts primes[0,3]      # => [2, 3, 5]
    puts primes.first(3)  # => [2, 3, 5]

    people.each { |person| puts person }
    people.each do |person|
      puts person
    end
    puts people

    people.select { |person| person =~ /^B/ }     # => ["Bob"]
    people.grep(/^B/)                             # => ["Bob"]

    sum_of_primes = primes.inject { |sum, x| sum + x }
    sum_of_primes = primes.inject(:+)

    sentence = "I am your father"
    words = sentence.split(" ")       # => ["I", "am", "your", "father"]
    tweet = "I can't find my father"
    relevant = words.any? { |word| tweet.include?(word) }

    reconstructed_sentence = words.join(" ")

    [1, 4, 8, 3, 54, 34].select { |n| n > 30 }      # => [54, 34]
    [1, 4, 8, 3, 54, 34].partition { |n| n > 30 }   # => [[54, 34], [1, 4, 8, 3]]
    [1, 3, 5].map { |n| n ** 2 }                    # => [1, 9, 25]
    [[1, 4], [4, 5], [6, 6]].flatten!               # => [1, 4, 4, 5, 6, 6]

## Hashes

    capitals = {
      "India" => "New Delhi",
      "Canada" => "Ottawa",
      "U.S.A" => "Washington D.C"
    }

    person = {
      first_name: "Albert",
      last_name: "Einstein",
      email: "al@wormhole.com"
    }

    puts "The capital of India is #{capitals['India']}"
    puts "Contact me at #{person[:email]}"

    capitals.each do |k, v|
      puts "The capital of #{k} is #{v}"
    end

    capitals.has_key?("Canada")
    capitals.key? "Canada"

    capitals.keys.sort.each do |k|
      puts "The capital of #{k} is #{capitals[k]}"
    end

    more_capitals = {
      "China" => "Beijing",
      "Russia" => "Moscow",
      "Germany" => "Berlin"
    }

    capitals.merge!(more_capitals)
    puts capitals.fetch('China')

    capitals.delete("China")

    begin
      puts "The capital of China is #{capitals.fetch('China')}"
    rescue KeyError
      puts "OMG! China has no capital"
    end

    capitals['China']                    # => nil
    capitals.fetch('China', 'Unknown')   # => "Unknown"

## Control Flow and Ranges

    while true
      puts "Forever"
    end

    x = 0
    loop do
      x = x + 1
      next if (x % 3) == 0
      puts x
      break if x >= 100
    end

    100.times { puts "Hello!!!!!!!!!" }
    1.upto(10) { |x| puts x }
    10.downto(1) { |x| puts x }
    (1..100).each { |x| puts x }
    (1..100).to_a
    puts (1..100).to_a
    puts *1..100

    if person == "Bob"
      puts "Hi Bob!"
    elsif person == "Charlie"
      puts "Hi Charlie!"
    else
      puts "Goodbye, fool!"
    end

    case person
    when/^[Bb]ob/
      puts "Hi Bob!"
    when /^[Cc]harlie/
      puts "Hi Charlie!"
    else
      puts "Goodbye, fool!"
    end

    case rand(6) + 1
    when 1..5
      puts "You lose!"
    when 6
      puts "You win!"
    else
      raise "What just happened?"
    end

## Getting Stuff Done

    Process.exit
    Process.exit 21

    first_argument = ARGV.first
    
    puts "#{ENV['HOME']} is where the heart is."
    ENV.each {|k, v| puts "#{k} = #{v}" if k =~ /^JAVA/ }

    while true
      puts "What is your name?"
      name = gets.chomp
      break unless name.empty?
    end

    begin
      puts "What is your name?"
      name = gets.chomp
    end while name.empty?

    ARGF.each_line { |line| puts line.capitalize }

    f = File.open("filename", "r")
    f.each_line { |line| puts line.chomp.reverse }
    f.close

    File.open("filename") do |f|
      f.each_line { |line| puts line.chomp.reverse }
    end

    f = File.open("destroy_this_file", "w")
    f.truncate(0)
    f.write("Destroyed\n")
    f.seek(0, IO::SEEK_SET)
    f.write("Destroyed Again\n")
    f.close

    puts File.stat("myfile").size
    puts File.stat("myfile").mtime
    puts File.stat("myfile").gid
    puts File.stat("myfile").writable?

    puts File.dirname("/boo/blah/haha")
    puts File.dirname(__FILE__)

    puts File.read("myfile") if File.exists?("myfile")
    File.open("/etc/passwd").chown(0, 0)
    File.open("/etc/passwd").chmod(0644)

    require 'fileutils'
    FileUtils.chown('root', 'wheel', '/etc/passwd')
    FileUtils.chown 'root', nil, Dir.glob('/usr/bin/*'), :verbose => true
    FileUtils.chown_R 'boo', 'boo', '/home/boo'
    FileUtils.cp %w(boo.html boo.js boo.css), '/home/boo/www'
    FileUtils.rm Dir.glob('*.trash')

    FileUtils.cd('/etc') do
      puts File.open("passwd").read
    end

    passwd_data = File.read("/etc/passwd")          # Returns one string
    passwd_lines = File.readlines("/etc/passwd")    # Returns array of strings

    puts Time.now
    seconds_since_epoch = Time.now.to_i
    puts Time.at(seconds_since_epoch).hour
    puts Time.at(seconds_since_epoch).min
    puts Time.at(seconds_since_epoch).sec

    hex_unixtime_ns = "0x44bf1c1f0258e"
    puts Time.at(hex_unixtime_ns.to_i(16) / 1000000.0)

    start_time = Time.now
    sleep 10
    end_time = Time.now
    puts end_time - start_time

    probability = rand
    random_float = rand * 100
    random_int = rand(100)
    dice_roll = rand(6) + 1
    100.times { puts rand(6) + 1 }

    boo = "boo"
    boo.each_byte { |x| puts x }
    boo.each_byte { |x| puts x.chr }
    "BORED".each_byte {|b| print b.to_s(base=16)}

    a = [ "a", "b", "c" ]
    n = [ 65, 66, 67 ]
    a.pack("A3A3A3")   #=> "a  b  c  "
    a.pack("a3a3a3")   #=> "a\000\000b\000\000c\000\000"
    n.pack("ccc")      #=> "ABC"
    [4, 5, 6].pack("c*")
    "abc \0\0abc \0\0".unpack('A6Z6')   #=> ["abc", "abc "]
    "abc \0\0".unpack('a3a3')           #=> ["abc", " \000\000"]
    "abc \0abc \0".unpack('Z*Z*')       #=> ["abc ", "abc "]
    "aa".unpack('b8B8')                 #=> ["10000110", "01100001"]
    "aaa".unpack('h2H2c')               #=> ["16", "61", 97]

    if "storm:4300" =~ /^(\S+):(\d+)$/
      host = $1
      port = $2
    end

## Methods, Blocks, and Procs

    def say_hello
      puts "Hello"
    end

    def say_something(thing)
      puts thing
    end

    def say_these_things(thing1="boo", thing2="blah")
      puts thing1
      puts thing2
    end
    say_these_things(thing2="ha")

    def show_people(*people)
      people.each { |p| puts p }
    end

    callback1 = proc { |name| puts "Hello #{name}." }
    callback2 = lambda { |name| puts "Hello #{name}." }
    callback1.call "world"
    callback2.call "world"     # proc behaves like lambda in this case
    callback1.call
    callback2.call             # lambda raises ArgumentError, unlike proc

    on_log = proc { |l| puts "#{ Time.now } -- #{l}" }
    def do_stuff(args, logger)
      # blah
      logger.call("Starting deextrapulator...")
      # blah
    end
    args = [ 1, 2, 3 ]
    do_stuff(args, on_log)

    def do_thing(*args, &job)
      job.call(args)
    end
    do_thing :blah { |x| puts x.to_s }

    def repeat(num)
      while num > 0
        yield num
        num -= 1
      end
    end
    repeat(3) { |x| puts x }

    def repeat num, &job
      num.downto 1, &job
    end
    repeat(3) { |x| puts x }

    trap "SIGINT", proc { puts "^C pressed." }

## Exceptions

    begin
      file = open("some_file")
    rescue
      file = STDIN
    end

    raise "String exception"
    raise

    fail "String exception"
    fail

    begin
      file = open("some_file", "w")
      file.write("blah")
    rescue Exception => e
      puts "Error: %s" %[e.message]
      e.backtrace.inspect
    ensure
      file.close
    end

    raise ArgumentError, 'Invalid argument'
    raise IndexError, 'Out of range'

    class RetryException < RuntimeError
      attr :can_retry
      def initialize(can_retry)
        @can_retry = can_retry
      end
    end

    def send_message(endpoint, message)
      bytes_written = endpoint.write(message)
      raise RetryException.new(true) if bytes_written.nil?
    end

    require 'socket'
    begin
      endpoint = TCPSocket.open("localhost", 2000)
      send_message(endpoint, "Hello endpoint!")
    rescue RetryException => e
      retry if e.can_retry
      raise
    ensure
      endpoint.close
    end

## Modules and Classes

    module Music
      A = 440
      def tune_up(note)
        # do blah
      end
      module_function :tune_up
    end

    puts Music::A
    Music.tune_up(:c_sharp)

    include Music
    puts A
    tune_up(:b_flat)

    class Person
      attr_reader :first_name, :last_name
      attr_accessor :age

      def initialize
        @first_name = "Blah"
        @last_name = "Boo"
      end

      def name
        @first_name + " " + @last_name
      end

      alias :fullname :name
      alias surname last_name
    end

    al = Person.new
    al.age = 10
    puts "#{al.name} is #{al.age} years old"

    class Employee < Person
      attr_reader :id
      def initialize(id, firstname, lastname)
        @employee_id = id
        @first_name = firstname
        @last_name = lastname
      end
    end

    scumbag = Employee.new(2300, "Scumbag", "Steve")

## Systems and Networking

    puts "My Process ID is #{ $$ }"

    system 'ls'
    `ls`.split(/\n/).length
    `ls`.lines.to_a.length

    include Process
    puts uid
    puts euid
    puts gid
    puts egid
    puts pid
    puts ppid
    kill 42

    p1 = fork { sleep 0.1 }
    p2 = fork { sleep 0.2 }
    Process.detach(p1)
    Process.waitpid(p2)
    sleep 2
    system("ps -ho pid,state -p #{p1}")

    pid = fork do
      trap :USR1 do
        $debug = !$debug
        puts "Debug now: #$debug"
      end
      trap :TERM do
        puts "Terminating..."
        shutdown()
      end
    end
    Process.detach(pid)
    Process.kill(:USR1, pid)
    Process.kill(:USR1, pid)
    Process.kill(:TERM, pid)

    Process.daemon
    Process.times

    require 'socket'
    host, path = ARGV
    port = 80
    s = TCPSocket.open(host, port)
    s.puts "GET #{path}"
    until s.eof?
      puts s.readline
    end
    s.close

    require 'socket'
    dts = TCPServer.new('localhost', 20000)
    loop do
      Thread.start(dts.accept) do |s|
        s.write(Time.now)
        s.close
      end
    end

    require 'net/http'
    h = Net::HTTP.new('www.amazon.com', 80)
    resp, data = h.get('/index.html')
    if resp.message == "OK"
      data.scan(/<img src="(.*?)"/) { |x| puts x }
    end

    require 'open-uri'
    open("http://www.ruby-lang.org/") {|f|
      f.each_line {|line| p line}
    }

## Ruby on the Command-line

    $ ls | ruby -ne 'puts $_ if $_ =~ /\.html$/'
    $ ls | ruby -pe '$_.capitalize!'
    $ ls | ruby -ne 'if /(.+)\.html$/ then puts `echo #{$1}` end'
    $ cat file.txt | ruby -ne 'BEGIN{$/="\n\n"}; puts $_.chomp'
    $ ruby -pe 'next unless $_ =~ /regexp/' < myfile.txt

## This Document

The source to this document is hosted on [GitHub][] and located
at [The Ruby Refresher Source][].

Feel free to send comments, changes, patches, criticism to me
at [http://0xfe.blogspot.com][].

[GitHub]: http://github.com
[The Ruby Refresher Source]: https://github.com/0xfe/rubyrefresher
[http://0xfe.blogspot.com]: http://0xfe.blogspot.com
[Beginner's Documentation]: http://www.ruby-lang.org/en/documentation/
