# Common build system
require 'rubygems'
require 'rake'

ERLC_TEST_FLAGS = "-pa ../ebin/eunit -I .. -I ../etest -I ../include/eunit -DTEST"
ERLC_FLAGS = "+debug_info -W2 -I ../include -I ../include/yaws -o ../ebin"
FUZED_VERSION = "0.4.13"

task :default do
  cd "elibs"
  sh "erlc  #{ERLC_FLAGS} #{ERLC_TEST_FLAGS} #{Dir["**/*.erl"].join(" ")}"
end

task :build_dist do
  cd "elibs"
  sh "erlc  +hipe #{ERLC_FLAGS} #{ERLC_TEST_FLAGS} #{Dir["**/*.erl"].join(" ")}"
end

task :econsole do
  sh "erl +Bc +K true -smp enable -pz ./etest -pz ./ebin/yaws -pz ./ebin/ -pz ./ebin/eunit -sname local_console_#{$$} -kernel start_boot_server true"
end

task :console do
  sh "irb -I rlibs/"
end

task :distel do
  puts "Starting distel editing session emacs@localhost"
  sh "erl +Bc +K true -smp enable -pz ./etest -pz ./ebin/yaws -pz ./ebin/ -pz ./ebin/eunit -name emacs@chisai.local -detached"
end

task :test => [:default] do
  mods = []
  mod_directives = ""
  env_peek = ENV['MOD'] || ENV['MODS'] || ENV['MODULE'] || ENV['MODULES']
  if env_peek
    mods = env_peek.split(",")
  else 
    mods = Dir["etest/*_test.erl"].map { |x| x.match(/etest\/(.*)_test.erl/)[1] }
  end
  mod_directives = mods.map {|m| "-run #{m} test"}.join(" ")
  # -run #{ENV['MOD']} test
  sh %Q{erl +K true -smp enable -pz ./etest -pz ./ebin/yaws -pz ./ebin/ -pa ./ebin/eunit -sname local_console_#{$$} -noshell #{mod_directives} -run erlang halt}
end

task :docs do
  #files = (Dir["elibs/*.erl"] - ["elibs/json.erl"]).sort.map { |x| "\'../" + x + "\'"}.join(" ")
  #sh %|cd doc && erl -noshell -run edoc_run files #{files}|
  files = Dir["elibs/*.erl"].map { |x| "'../" + x + "'"}.join " "
  sh %|cd doc && erl -noshell -s init stop -run edoc files #{files}|
end
