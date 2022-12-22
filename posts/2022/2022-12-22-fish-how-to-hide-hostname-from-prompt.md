---
layout: post
title: fish shell - How to Hide hostname from Prompt
date: 2022-12-22
---

By default, fish shell displays hostname in the prompt. For WSL2 usage, you might not like to see the hostname displayed in the fish prompt. Below is an simple note for fish shell to how to hide hostname from Prompt.

## After 3.3.0

1. Enter fish function editing through

```sh
$ funced prompt_login
```

2. on the last `echo` line, remove `@ (set_color $color_host) (prompt_hostname) (set_color normal)`. Save and exit your editor.

3. You may be prompted to save the function, do so. If you don't get prompted, do following:

```sh
$ funcsave fish_prompt
```

## Prior to 3.3.0

1. Enter fish function editing through

```sh
$ funced fish_prompt
fish_prompt> # Defined in /usr/share/fish/functions/fish_prompt.fish @ line 4
             function fish_prompt --description 'Write out the prompt'
                 set -l last_pipestatus $pipestatus
                 set -l normal (set_color normal)

                 # Color the prompt differently when we're root
                 set -l color_cwd $fish_color_cwd
                 set -l prefix
                 set -l suffix '>'
                 if contains -- $USER root toor
                     if set -q fish_color_cwd_root
                         set color_cwd $fish_color_cwd_root
                     end
                     set suffix '#'
                 end

                 # If we're running via SSH, change the host color.
                 set -l color_host $fish_color_host
                 if set -q SSH_TTY
                     set color_host $fish_color_host_remote
                 end

                 # Write pipestatus
                 set -l prompt_status (__fish_print_pipestatus " [" "]" "|" (set_color $fish_color_status) (set_color --bold $fish_color_status) $last_pipestatus)

                 echo -n -s (set_color $fish_color_user) "$USER" $normal @ (set_color $color_host) (prompt_hostname) $normal ' ' (set_color $color_cwd) (prompt_pwd) $normal (fish_vcs_prompt) $normal $prompt_status $suffix " "
             end
```

2. on the last `echo` line, remove ` @ (set_color $color_host) (prompt_hostname) $normal`. Save and exit your editor.

3. You may be prompted to save the function, do so. If you don't get prompted, do following:

```sh
$ funcsave fish_prompt
```
