## options taken  from various stackoverflow and GitHub repos

##    use_nvim_with_R and this .Rprofile is my way to standardise nvimR and R installation and updating in Ubuntu/Debian
##    Copyright (C) 2023 ConYel 
##
##    This file is part of use_nvim_with_R.
##
##    use_nvim_with_R is free software: you can redistribute it and/or modify it under the
##    terms of the GNU Affero General Public License as published by the Free Software Foundation,
##    either version 3 of the License, or (at your option) any later version.
##
##    use_nvim_with_R is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY;
##    without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
##    See the GNU Affero General Public License for more details.
##
##    You should have received a copy of the GNU Affero General Public License along with
##    use_nvim_with_R. If not, see <https://www.gnu.org/licenses/>.

options(max.print=100)
options(languageserver.server_capabilities =
        list(completionProvider = FALSE, completionItemResolve = FALSE))
options(menu.graphics=FALSE)
options(max = 10L)
options(digits = 4L)
options(Ncpus = max(1L, parallel::detectCores() - 1L))
options(max.print = 100L)
options(mc.cores = max(1L, parallel::detectCores() - 1L))
options(show.error.locations = TRUE,
        check.bounds = FALSE,
        setWidthOnResize = TRUE)

utils::rc.settings(ipck=TRUE)

qq <- function (save="no", ...) {
  quit(save=save, ...)
}

.First <- function(){
  if(interactive()){
    library(utils)
    timestamp(,prefix=paste("##------ [",getwd(),"] ",sep=""))
  }
}

.Last <- function(){
  if(interactive()){
    hist_file <- Sys.getenv("R_HISTFILE")
    if(hist_file=="") hist_file <- "~/.RHistory"
    savehistory(hist_file)
  }
}

# I don't remember from where I got this function, I think it might be from 
# jalvesaq:https://github.com/jalvesaq/ but I am not sure.
lsos <- function(order.by = c("PrettySize", "Type", "Size", "Rows", "Columns"),
                 pos = 1) {
  napply <- function(names, fn) sapply(names, function(x) fn(get(x, pos = pos)))
  names <- ls(pos = pos)

  obj.class = napply(names, function(x) as.character(class(x))[1])
  obj.mode = napply(names, mode)
  obj.type = ifelse(is.na(obj.class), obj.mode, obj.class)
  obj.prettysize = napply(names, function(x) {
    utils::capture.output(print(utils::object.size(x), units = "auto"))
  })
  obj.size = napply(names, utils::object.size)
  obj.dim = t(napply(names, function(x) as.numeric(dim(x))[1:2]))

  if (length(names) > 0) {
    vec = is.na(obj.dim)[, 1] & (obj.type != "function")
    obj.dim[vec, 1] = napply(names, length)[vec]
    out = data.frame(obj.type, obj.size, obj.prettysize, obj.dim)
  } else {
    out = tibble::tibble("a", "b", "c", "d", "e")
    out = out[FALSE, ]
  }
  names(out) = c("Type", "Size", "PrettySize", "Rows", "Columns")
  order.by = match.arg(order.by)
  out = out[order(out[[order.by]], decreasing = TRUE), ]
  tibble::tibble(out)
}
if (Sys.getenv("TERM") == "xterm-256color")
    require("colorout")
