    use_nvim_with_R is my way to standardise nvimR and R installation and updating in Ubuntu/Debian
    Copyright (C) 2023 ConYel 

    This file is part of use_nvim_with_R.

    use_nvim_with_R is free software: you can redistribute it and/or modify it under the
    terms of the GNU Affero General Public License as published by the Free Software Foundation,
    either version 3 of the License, or (at your option) any later version.

    use_nvim_with_R is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
    without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    See the GNU Affero General Public License for more details.

    You should have received a copy of the GNU Affero General Public License along with
    use_nvim_with_R. If not, see <https://www.gnu.org/licenses/>.


# How to use nvim with R 
This is a way to standardise how I should install/update R and bioconductor   
while using nvim with jalvesaq-[Nvim-R](https://github.com/jalvesaq/Nvim-R)  

## UBUNTU/DEBIAN ENVIROMENT
Current folders:  

In main `R` directory:  
```
R
└── x86_64-pc-linux-gnu-library
    ├── 4.2
    ├── 4.3.1-Bioc-3.17
    └── 4.3.1-Bioc-3.18
```

In `.bashrc` I have this:
```
# alias for R if it exists
if [ -f /usr/bin/R ] ; then
    export R_LIBS_USER='~/R/x86_64-pc-linux-gnu-library/4.3.1-Bioc-3.17'

    alias R='R_LIBS_USER=~/R/x86_64-pc-linux-gnu-library/4.3.1-Bioc-3.17/ R'
fi
```
Of course it is not the best options as I have to update every time the version...  

Sparce configuration files for nvim:  

`.config/nvim/lua/core`
```my.remaps.lua
-- R plugin
vim.keymap.set("v", "<C-e>", "<Plug>RDSendSelection", { desc = '[C]trl [e] send selected' })
vim.keymap.set("n", "<C-e>", "<Plug>RDSendLine", { desc = '[C]trl [e] send line' })
```

`.config/nvim/lua/core`
```options.lua
vim.api.nvim_set_var('R_auto_start' , 1)
vim.api.nvim_set_var('R_objbr_auto_start' , 1)
```

`.config/nvim/lua/core`
```packer.lua
  use ( 'jalvesaq/Nvim-R' )
  use ( 'jalvesaq/cmp-nvim-r' )
  use ( 'jalvesaq/cmp-zotcite' )
```
-------------------------------------------------------------------------------

The first time I installed R I followed the guide from [here](https://www.digitalocean.com/community/tutorials/how-to-install-r-on-debian-10)
I put the first steps here in case something happen to the link, and I also saved it on the wayback [machine](https://web.archive.org/web/20231028180349/https://www.digitalocean.com/community/tutorials/how-to-install-r-on-debian-10):
- By Lisa Tagliaferri
```
sudo apt install dirmngr --install-recommends
sudo apt install software-properties-common
sudo apt install apt-transport-https
sudo apt-key adv --keyserver keys.gnupg.net --recv-key 'E19F5F87128899B192B1A2C2AD5F960A256A04AF'
sudo add-apt-repository 'deb http://cloud.r-project.org/bin/linux/debian buster-cran35/'
sudo apt update
sudo apt install r-base
```

Then I install the Bioconductor version in my main user on the specific folder 
that has been set in `.bashrc`


```
R
install.packages("Bioconductor", )
q()
mkdir ~/R/x86_64-pc-linux-gnu-library/4.3.1-Bioc-3.18
R
BiocManager::install(version= "3.18", lib = "/home/user6/R/x86_64-pc-linux-gnu-library/4.3.1-Bioc-3.18/")
q()
```
After the installation of all the packages from the previous version it should be checked if
`colorout` works, as some times it doesn't and it needs to be reinstalled
