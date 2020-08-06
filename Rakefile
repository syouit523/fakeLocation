require 'fileutils'

WORKDIR = File.dirname(__FILE__)

class String
  def red
    "\e[32m#{self}\e[0m"
  end

  def green
    "\e[32m#{self}\e[0m"
  end

  def blue
    "\e[34m#{self}\e[0m"
  end

  def bold
    "\e[1m#{self}\e[22m"
  end
end

desc 'Initialize for development'
task :init do

  info 'Init'
  Rake::Task["brew"].execute()
  Rake::Task["libimobiledevice"].execute()
  Rake::Task["cloneIdevicelocation"].execute()
  Rake::Task["setVariables"].execute()
  Rake::Task["make"].execute()
  info 'Complete all process!'
end

desc 'brew install'
task :brew do
  info 'start brew install'
  if `which brew`.empty? then
    sh '/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"'
  end
end

desc 'install libimobiledevice'
task :libimobiledevice do
  info 'start install libimobiledevice'

  sh 'brew install libimobiledevice'

  brewInstallPackages = ["libtool", "automake", "libzip"]
  for package in brewInstallPackages do
    if isBrewInstalled(package) == false then
      sh 'brew install #{package}'
    end
  end
  if File.exist?('/usr/local/lib/pkgconfig/libplist.pc') == false then
    # if installed libplist-2.0 create to libplist.pc for ideviceLocation
    sh 'cp /usr/local/lib/pkgconfig/libplist-2.0.pc /usr/local/lib/pkgconfig/libplist.pc'
  end
end

def isBrewInstalled(package)
  sh "brew list | grep #{package}" do |ok, status|
    return ok
  end
end

desc 'git clone idevicelocation'
task :cloneIdevicelocation do
  info 'start git clone idevicelocation'
  if File.directory?('./idevicelocation') == false then
    sh 'git clone https://github.com/JonGabilondoAngulo/idevicelocation.git'
  end
end

desc 'set variables'
task :setVariables do
  info 'start set variables'
  sh 'export PATH=/usr/local/opt/openssl/bin:$PATH'
  sh 'export LD_LIBRARY_PATH=/usr/local/opt/openssl/lib:$LD_LIBRARY_PATH'
  sh 'export CPATH=/usr/local/opt/openssl/include:$CPATH'
  sh 'export LIBRARY_PATH=/usr/local/opt/openssl/lib:$LIBRARY_PATH'
  sh 'export PKG_CONFIG_PATH=/usr/local/opt/openssl/lib/pkgconfig'
end

desc 'libimobiledevice make'
task :make do
  info 'start libimobiledevice make'
  cd 'idevicelocation/' do
    sh './autogen.sh'
    sh 'make'
    sh 'cp ./src/idevicelocation /usr/local/bin/idevicelocation'
  end
end

desc 'uninstall'
task :uninstall do
  info 'start uninstall'
  if FileTest.exists?("./idevicelocation/") then
    sh 'rm -rf idevicelocation'
  end
  if FileTest.exists?("./LocationSimulator/") then
    sh 'rm -rf LocationSimulator'
  end
  sh 'brew uninstall --ignore-dependencies libimobiledevice'
end

def verbose(s)
  puts s
end

def info(s)
  puts s.bold.red
end

def success(s)
  puts s.bold.green
end

def fail(s)
  abort s.bold.red
end
