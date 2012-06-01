# Copyright 2010-2012 Manolis Papadakis <manopapad@gmail.com>,
#                     Eirini Arvaniti <eirinibob@gmail.com>
#                 and Kostis Sagonas <kostis@cs.ntua.gr>
#
# This file is part of PropEr.
#
# PropEr is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# PropEr is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with PropEr.  If not, see <http://www.gnu.org/licenses/>.

# Author:      Manolis Papadakis
# Description: Instructions for make
require 'rubygems'
def rebar_verbose_flag
    ENV["VERBOSE"] && "-v"
end
task :default => :compile


def rebar(param_string)
  rebar = File.join(File.dirname(__FILE__), 'rebar')
  sh "\"#{rebar}\" #{param_string} #{rebar_verbose_flag}" do |ok, res|
      if ! ok
          message = "\e[0;31m \n\n************************************\n\t"
          message += "Something failed running: \e[m'rebar #{param_string}'"
          message += "\e[0;31m\n************************************\e[m"
          fail "#{message}"
      end
  end
end

desc "Build proper"
task :compile => :'include/compile_flags.hrl' do
	rebar 'compile'
end

desc "Use rebar to get the deps"
task :get_deps do
  rebar 'get-deps'
end

desc "Generate build flags"
task :'include/compile_flags.hrl' do
	`escript.exe write_compile_flags include/compile_flags.hrl`
end

task :dialyzer => [:compile] do
	`dialyzer -Wunmatched_returns ebin`
end

desc "Check escripts"
task :'check_scripts' do
	`check_escripts.sh make_doc write_compile_flags`
end

desc "Tests"
task :eunit => [:compile] do
	rebar 'eunit'
end

desc "Doc"
task :make_doc do
	`make_doc`
end

desc "clean"
task :clean do
	`clean_temp.sh`
end

desc "distclean"
task :distclean => [:clean] do
	FileUtils.rm_f(File.join("include", "compile_flags.hrl"))
	rebar 'clean'
end

desc "rebuild"
task :rebuild => [:distclean, :'include/compile_flags.hrl'] do
	rebar 'compile'
end

desc "retest"
task :retest => [:compile] do
	FileUtils.rm_rf('.eunit')
	rebar 'eunit'
end
