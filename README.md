# xrandr-stuff
xrandr-reference

=======
as of the time of writing, the tl;dr is the 3 shell scripts do my trick. Might try without the arandr export one...

good article: https://blog.summercat.com/configuring-mixed-dpi-monitors-with-xrandr.html

 1. save xrandr output into a file called before.status
 2. copy it into a new one before.status.trimmed
 3. look at the resolution details given in System Settings > Devices > Displays (each monitor) and delete the lower resolutions, and any ARs you don't want
 4. visit dpi.lv to find out the ppi (more correct term for dpi) of your monitors (open the site in a browser tab located on the desired monitor)
 5. now, do some math according to the article saved in reference.htm
 6. 3840x2160 is my laptop screen's max res    | 331 ppi
 7. 2560x1440 is my external monitor's max res | 221 ppi
 8. find out the currently set DPI using xrdb -q and save that too (maybe save the whole output in a file)
 9. we'll use the dpi of the higher dpi monitor
10. also we'll use scale 1.5x1.5 because (2560x1440) * 1.5 == 3840x2160
11. now, the external monitor is tilted right, so instead of 3840x2160, we use 2160x3840 so, this means
    => total pixels in the width should be 3840 + 2160 = 6000
    => total pixels needed in the height (to fit in both screens) should be 3840 (due to the vertical / tilt-right external monitor)
    => overall canvas should be 6000x3840 (this factors in the rotation)
12. We will use "--rotate right" in the external monitor arguments alongwith providing mode 2560x1440 (not the flipped)
13

# xrandr --dpi 276 --fb 7040x3960 --output eDP-1 --mode 3200x1800 --output DP-1-2 --scale 2x2 --pos 3200x0 --panning 3840x2160+3200+0

xrandr --dpi 331 --fb 6000x3840 --output eDP-1 --mode 3840x2160 --pos 0x1200 --output DP-3 --scale 1.5x1.5 --pos 3840x0 --panning 2160x1680+3840+0 --rotate right
produced screen 2 a bit more zoomed in than needed => we need more scale
produced screen 2 placed higher than needed => --pos y value of screen 1 needs to reduce

scale 1.8 => 4608x2592
=> total canvas 6432x4608

xrandr --dpi 331 --fb 6432x4608 --output eDP-1 --mode 3840x2160 --pos 0x400 --output DP-3 --scale 1.8x1.8 --pos 3840x0 --panning 2592x2448+3840+0 --rotate right

made external screen even more zoomed => need to _reduce_ the scale factor, not increase it

let's go with 1.2 (as opposed to 1.5 earlier)

=> 3072x1728
=> total canvas size 5568x3072

xrandr --dpi 331 --fb 5568x3072 --output eDP-1 --mode 3840x2160 --pos 0x300 --output DP-3 --scale 1.2x1.2 --pos 3840x0 --panning 1728x912+3840+0 --rotate right

============

all that is junk.

install arandr to _see_ what's the scene of the whole canvas
use system thing to setup alignment etc.

I used 2 commands that got shit working great!

1) scale eDP-1 by 1.2x1.2
2) scale DP-3 by 2x2

  xrandr --output eDP-1 --mode 3840x2160 --scale 1.2x1.2
 2020/04/11 16:12  sankalp  ~ 
 $  xrandr --output eDP-1 --mode 3200x1800 --scale 1.2x1.2
 2020/04/11 16:12  sankalp  ~ 
 $  xrandr --output eDP-1 --mode 3200x1800 --scale 1.2x1.2 --output DP-3 --mode 1440x2560 --pos 3840x0 --scale 1x1
xrandr: cannot find mode 1440x2560
 2020/04/11 16:13  sankalp  ~ 
 $  xrandr --output eDP-1 --mode 3200x1800 --scale 1.2x1.2 --output DP-3 --mode 2560x1440 --pos 3840x0 --scale 1x1
 2020/04/11 16:14  sankalp  ~ 
 $  xrandr --output eDP-1 --mode 3200x1800 --scale 1.2x1.2 --output DP-3 --mode 2560x1440 --pos 3840x0^C
 2020/04/11 16:15  sankalp  ~ 
 $  xrandr --output DP-3 --scale 0.5x0.5
 2020/04/11 16:15  sankalp  ~ 
 $  xrandr --output DP-3 --scale 2x2

can use arandr to drag the main screen down, to setup their relative position, and apply.
it auto-extends canvas size!!!

OMG arandr also exports the current state to a xrandr command (shell script!)
W O W

