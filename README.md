Prerequisite
Before using Starship, you need to install a nerd font on your computer. Nerd font is a beautiful blend of font and icons. Go to Nerd Fonts and download your favorite one. If you can’t make up your mind, you can also preview the fonts on programmingfonts.org.

In my case, I like “Fira Code Nerd font”. Download the font and extract the content inside it. You will get a folder named “FiraCode” and all the .otf and .ttf versions of the font.

To install this font, go to your terminal and run the following command.

```sh
# Creating the local font directory in your system
mkdir ~/.local/share/fonts
 
# Moving the extracted fonts to local font directory
mv FiraCode ~/.local/share/fonts/
```

Now FiraCode Nerd font becomes available in your system. You can also install other different types of fonts by following the above guide.

If you like to read an in-depth guide on font installation on a Linux desktop, check out this article on how to install fonts in Ubuntu.

Install Starship
To install starship, go to your home directory and run the following command.

```sh
curl -sS https://starship.rs/install.sh | sh
```
This command will download and install starship binary and add it to your path.

Install Starship on Your Shell
If you are using a bash shell, open your “~/.bashrc” file and paste this line of code at the end.

```sh
eval "$(starship init bash)"
```
If you are using Zsh, open your “~/.zshrc” file instead and paste the above line of code at the end of it.

Now restart your terminal app. You will be greeted by the default starship prompt.

Customize Starship
To customize your shell prompt, create a “starship.toml” file in your config directory.

```sh
# Creating your .config directory if not existed
mkdir -p ~/.config
 
# Creating a new starship.toml file
touch ~/.config/starship.toml
```
You can also change the default location of the starship configuration file. To change the default location of your starship configuration file, add the STARSHIP_CONFIG environment variable at the bottom of your “~/.bashrc” file.

```sh
export STARSHIP_CONFIG=~/example/non/default/path/starship.toml
```
Concept of Module and Variable
A module is a component in the terminal prompt, that gives you contextual information about the underlying operating system. For example, Nodejs is a module in the starship prompt. When we add this module to the config file, then it gives us different data points regarding the current installation of Nodejs in your environment.

The module gives that information in terms of variables. Variables are smaller sub-components that contain information about the module. For example, version is the variable of “Nodejs”. When we call the $version inside the Nodejs module, it gives us the currently installed version of Nodejs in your system. Variables are prefixed with $ symbol. The name of a variable can only contain letters, numbers, and “_”.

Text Groups and Styles
Text groups in starship contain 2 parts. The first part is enclosed under the [ ] symbol. This part is called format string. We can add texts, variables, and even nested text groups here.

The latter part is enclosed under ( ) and this is called style string. You can style the text group using this style string.

Let’s take an example.

```sh
[make_tech_easier](yellow bold)
```
will print the string “make_tech_easier” with bold text and yellow color.

```sh
[👍 All Done](green) 
```
will print “👍 All Done” in green color.

```sh
[a [b](green) c](red)
```
This is an example of nested text groups. It prints a and c in red and b in green.

Some more Style Settings

We can set different foregrounds and backgrounds for the text groups. 

```sh
[ ](fg:red bg:blue) 
```
make text font color red and background blue.
We can also use ANSI colors in the config file. 

```sh
[ ](bold fg:27)
```
represent bold text with ANSI 27 color as foreground color.
If you like to add your hex color, then this is also possible. 

```sh
[ ](underline bg:#bf5700)
```
gives an underlined text with a bright orange background color.


Prompt Customization in Starship
The prompt customization of starship has 4 options. We can change these 4 options to customize the prompt style.

format: It defines how the prompt would look like inside the terminal. You can define any design as a prompt.
scan_timeout: Timeout for a starship to scan files.
command_timeout: Timeout for command executed by starship.
add_newline: It is a boolean. If set true, it adds a blank line between shell prompt.
A demo prompt customization file looks like this.

```sh
# Use custom format
format = """
[┌───────────────────>](bold green)
[│](bold green)$directory$rust$package
[└─>](bold green) """
 
# Wait 10 milliseconds for starship to check files under the current directory.
scan_timeout = 10
 
# Disable the blank line at the start of the prompt
add_newline = false
```
Add Your Desired Module
Now as you know the basics of starship and how to customize it, you can add your desired module very easily. Keep in mind that any module will work if the files and folder related to this module will present at the given path. Simply, the python module will only be visible, if any python file is present inside the current working directory.

Let’s add the python module to our starship configuration as a demo. open your starship.toml file. There add [python] in a new line to activate the python environment.

Python module shows the information about python installation in your OS or if any virtual environment is activated. If you want to change the python icon and want to display the virtualenv name, then the configuration file looks like this.

```sh
[python]
symbol = "🐍 "
pyenv_version_name = true
```
If you have installed both python2 and python3, then you can specify the default python binary using starship configuration file.

```sh
[python]
# Only use the `python3` binary to get the version.
python_binary = "python3"
```
Like this example, you can add any module you want and customize them as per your imagination.

How to Add Starship Presets
If you don’t want to do all those customization yourself, but want a good-looking terminal prompt out of the box, you can use presets. Presets are simply starship configuration files shared by other users. You can import those presets to your config file and you have a nice-looking terminal prompt in just a moment.


You can explore the official presets on this page. The above terminal prompt is uploaded by a user and available on the presets page. Now to make your terminal prompt look like this, open your starship config file and add this configuration to your file.

```sh
format = """
[←](#9A348E)\
$username\
[→](bg:#DA627D fg:#9A348E)\
$directory\
[→](fg:#DA627D bg:#FCA17D)\
$git_branch\
$git_status\
[→](fg:#FCA17D bg:#86BBD8)\
$c\
$elixir\
$elm\
$golang\
$haskell\
$java\
$julia\
$nodejs\
$nim\
$rust\
[→](fg:#86BBD8 bg:#06969A)\
$docker_context\
[→](fg:#06969A bg:#33658A)\
$time\
[→ ](fg:#33658A)\
"""
 
# Disable the blank line at the start of the prompt
# add_newline = false
 
# You can also replace your username with a neat symbol like  to save some space
[username]
show_always = true
style_user = "bg:#9A348E"
style_root = "bg:#9A348E"
format = '[$user ]($style)'
 
[directory]
style = "bg:#DA627D"
format = "[ $path ]($style)"
truncation_length = 3
truncation_symbol = "…/"
 
# Here is how you can shorten some long paths by text replacement
# similar to mapped_locations in Oh My Posh:
[directory.substitutions]
"Documents" = "📄 "
"Downloads" = "📥 "
"Music" = "🎜 "
"Pictures" = "📷 "
# Keep in mind that the order matters. For example:
# "Important Documents" = "  "
# will not be replaced, because "Documents" was already substituted before.
# So either put "Important Documents" before "Documents" or use the substituted version:
# "Important  " = "  "
 
[c]
symbol = "© "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[docker_context]
symbol = "🐳 "
style = "bg:#06969A"
format = '[[ $symbol $context ](bg:#06969A)]($style) $path'
 
[elixir]
symbol = "💧 "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[elm]
symbol = "🌳 "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[git_branch]
symbol = "☊"
style = "bg:#FCA17D"
format = '[[ $symbol $branch ](bg:#FCA17D)]($style)'
 
[git_status]
style = "bg:#FCA17D"
format = '[[($all_status$ahead_behind )](bg:#FCA17D)]($style)'
 
[golang]
symbol = "🐹 "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[haskell]
symbol = "λ "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[java]
symbol = "☕ "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[julia]
symbol = "ஃ "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[nodejs]
symbol = "🔷 "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[nim]
symbol = "👑 "
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[rust]
symbol = "🦀"
style = "bg:#86BBD8"
format = '[[ $symbol ($version) ](bg:#86BBD8)]($style)'
 
[time]
disabled = false
time_format = "%R" # Hour:Minute Format
style = "bg:#33658A"
format = '[[ 🤍 $time ](bg:#33658A)]($style)'
```
Restart your terminal and your terminal prompt look exactly like the prompt in the image. You can search for more terminal prompts presets and try which suits you the best. In the meantime, if you want to try out some bash tips and tricks, this article should be best suited for you.

How to uninstall starship?
To uninstall starship, first, delete your starship config file.

```sh
rm ~/.config/starship.toml
```
Then remove the line you have pasted during installation inside the “~/.bashrc” file.

At last, uninstall the starship binary from your device.

```sh
sh -c 'rm "$(command -v 'starship')"'
```
Now relaunch your terminal and your prompt restored to the default style.

Frequently Asked Questions
How do I apply my current terminal prompt style to another device?
It is very easy. Just copy your starship configuration file located in your 
```sh
~/.config/starship.toml
```
to the other device. Install starship, restart the terminal, and you are done.

