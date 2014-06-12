# encoding: UTF-8

require 'rake'

task :brew do
  if !File.exists?("/usr/local/Cellar")
    `ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"`
  end
   # TODO: refactor into own listing
  `brew install ack git libtool imagemagick mysql postgres phantomjs pngcrush rbenv rbenv-gemset ruby-build tmux vim tree ssh-copy-id the_silver_searcher`
end

task :install do
  linkables = Dir['home/*']
  colors = Dir['colors/*']
  hostname = `hostname`.strip

  skip_all = false
  overwrite_all = false
  backup_all = false

  if !File.exists?("/Users/#{hostname}/.vim/janus")
    puts "✱ Installing Janus"
    `curl -Lo- https://bit.ly/janus-bootstrap | bash`
  end

  puts "✱ Linking submodules"
  FileUtils.rm_rf("$HOME/.janus")
  `ln -s "$PWD/janus" "$HOME/.janus"`

  puts "✱ Linking Colorschemes"
  colors.each do |color|
    color = color.sub('colors/','')
    target = "#{ENV["HOME"]}/.vim/janus/vim/colors/#{color}"
    FileUtils.rm_rf(target)
    puts "- linked #{color}"
    `ln -s "$PWD/colors/#{color}" "#{target}"`
  end

  if !File.exists?("/bin/zsh")
    puts "✱ Installing zsh"
    `sudo apt-get install zsh`
  end

  if !File.exists?("/Users/#{hostname}/.oh-my-zsh")
    puts "✱ Installing oh-my-zsh"
    `curl -L http://install.ohmyz.sh | sh`
  end

  puts "✱ Syncing gitmodules (janus plugins)"
  `git submodule update --init`

  puts "\n✱ Symlinking dotfiles"
  linkables.each do |linkable|
    overwrite = false
    backup = false
    linkable = linkable.sub('home/','')
    target = "#{ENV["HOME"]}/.#{linkable}"

    if File.exists?(target) || File.symlink?(target)
      unless skip_all || overwrite_all || backup_all
        puts "!!! File already exists: #{target}"
        puts "!!! What do you want to do? [s]kip, [S]kip all, [o]verwrite, [O]verwrite all, [b]ackup, [B]ackup all"
        case STDIN.gets.chomp
        when 'O' then overwrite_all = true
        when 'o' then overwrite = true
        when 'B' then backup_all = true
        when 'b' then backup = true
        when 'S' then skip_all = true
        when 's' then next
        end
      end
      FileUtils.rm_rf(target) if overwrite || overwrite_al
      `mv "$HOME/.#{linkable}" "$HOME/backups/.#{linkable}.backup"` if backup || backup_all
    end
    puts "✱ Linked #{target}"
    `ln -s "$PWD/home/#{linkable}" "#{target}"`
  end
end

task :uninstall do
  linkables = Dir.glob('home/*')

  linkables.each do |linkable|
    linkable = linkable.sub('home/','')
    target = "#{ENV["HOME"]}/.#{linkable}"

    if File.symlink?(target)
      puts "✱ Removed #{target}"
      FileUtils.rm(target)
    end

    if File.exists?("#{ENV["HOME"]}/.#{linkable}.backup")
      puts "✱ Restored #{linkable}"
      `mv "$HOME/backups/.#{linkable}.backup" "$HOME/.#{linkable}"`
    end
  end
 end

# thanks mislav
task :update_submodules do
  system <<-EOS
    git submodule foreach '
      rev=$(git rev-parse HEAD)
      git pull --quiet --ff-only --no-rebase origin master &&
      git --no-pager log --no-merges --pretty=format:"%s %Cgreen(%ar)%Creset" --date=relative ${rev}..
      echo
    '
  EOS
end

task :add_submodule, [:a0, :a1] do |t, args|
  install_path = "janus/#{args[:a1]}"
  puts "*installing #{args[:a0]} to #{install_path}"
  `git submodule add #{args[:a0]} #{install_path}`
end

task :default => 'install'
