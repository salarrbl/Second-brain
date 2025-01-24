# Conky
- ## Clock
- ![[conky-clock.png]]
```conky.conf

use_xft yes
xftfont 123:size=8
xftalpha 0.1
update_interval 1
total_run_times 0

own_window yes
own_window_type normal
#own_window_transparent yes
own_window_hints undecorated,below,sticky,skip_taskbar,skip_pager
own_window_colour 000000
#own_window_argb_visual no
#own_window_argb_value 0

own_window_transparent no
own_window_argb_visual yes
own_window_argb_value 0

double_buffer yes
#minimum_size 250 5
#maximum_width 500
draw_shades yes
draw_outline no
draw_borders no
draw_graph_borders no
default_color white
default_shade_color black
default_outline_color black
alignment top_left
gap_x 80
gap_y 80
no_buffers yes
uppercase no
cpu_avg_samples 2
net_avg_samples 1
override_utf8_locale yes
use_spacer yes


minimum_size 0 0
TEXT
${alignc}${voffset 5}${color #c3bebb}${font Sqeron, Regular:pixelsize=100}${time %H:%M}${font}
${alignc}${voffset 5}${color #c3bebb}${font Nasalization, Regular:pixelsize=36}${time %A}${font}
${alignc}${voffset 5}${offset 15}${color #c3bebb}${font Sqeron, Regular:pixelsize=80}${time %d} 
${alignc}${voffset -35}${color #c3bebb}${font Nasalization, Regular:pixelsize=18}${time  %B} ${time %Y}${font}
```
# the best vim config
wim -> create by woland [GitHub](https://github.com/wolandark/wim)
