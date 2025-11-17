# StevanFreeBorn's Nvim Config CAA 17 Nov 2025

> This is my neovim configuration. It is - as with all of us - a work in progress. If you are curious about getting started with Neovim I couldn't recommend this course from Typecraft enough: [Neovim for Newbs](https://typecraft.dev/neovim-for-newbs)

## Refresher

You can store your neovim configurations independently from neovim, so long as ultimately that folder has the `init.lua` script.

Neovim figures out what config folder you want to use by referencing the $NVIM_APPNAME variable.

With this, you can change the config folder that neovim looks for by specifing a folder. 

Store your config folders under XDG_CONFIG_HOME, which for windows is `~/AppData/Local`, whre `~` is your home directory.

For profile switching with powershell in windows, use the script below, and save it at "C:\Users\<YOUR_USERNAME>\OneDrive\Documents\PowerShell\Microsoft.PowerShell_profile.ps1" or wherever you want your powershell to look at before it loads.

Do:

`nvims <enter_key>` from your powershell window and pick which config folder you want nvim to use.

Alternatively, set up functions (the equivalent of aliases in linux and macOS) to swap out keywords and set the config folder you wanna use (see lines 1 - 6 of the script below). ie:

`nvim-lazy` automatically will pull up lazyvim.

```pwsh
# Aliases for different Neovim configurations
function nvim-lazy { $env:NVIM_APPNAME = "LazyVim"; nvim @args }
function nvim-stevan { $env:NVIM__APPNAME = "StevanFreeBorn"; nvim @args }
#function nvim-kick { $env:NVIM_APPNAME = "kickstart"; nvim @args }
#function nvim-chad { $env:NVIM_APPNAME = "NvChad"; nvim @args }
#function nvim-astro { $env:NVIM_APPNAME = "AstroNvim"; nvim @args }

# Function to select Neovim configuration
function nvims {
    $items = @("default", "LazyVim" ,"StevanFreeBorn")
    #$items = @("default", "kickstart", "LazyVim", "NvChad", "AstroNvim")
    
    # Using fzf to select configuration
    $config = $items | fzf --prompt=" Neovim Config  " --height=~50% --layout=reverse --border --exit-0
    
    if (-not $config) {
        Write-Host "Nothing selected"
        return
    }
    elseif ($config -eq "default") {
        $config = ""
    }
    
    $env:NVIM_APPNAME = $config
    nvim @args
}

# Set up key binding (Ctrl+A) to launch nvims
Set-PSReadLineKeyHandler -Key Ctrl+A -ScriptBlock {
    [Microsoft.PowerShell.PSConsoleReadLine]::RevertLine()
    [Microsoft.PowerShell.PSConsoleReadLine]::Insert("nvims")
    [Microsoft.PowerShell.PSConsoleReadLine]::AcceptLine()
}
```

Inspired by [Elijah Manor's Neovim Config Switcher](https://youtu.be/LkHjJlSgKZY?si=k7eFnbRSu8Q7C6yf) and some guy who translated the linux stuff into powershell for me. 
(I'll always remember you whoever you are (╯˘ -˘ )╯ )
