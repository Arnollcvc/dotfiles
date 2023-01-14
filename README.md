# dotfiles
configuracion lol first time 
qtile conf temporal




import os
import subprocess
from libqtile import hook

from libqtile import bar, layout, widget
from libqtile.config import Click, Drag, Group, Key, Match, Screen
from libqtile.lazy import lazy
from libqtile.utils import guess_terminal

mod = "mod4"
terminal = guess_terminal()

disposivo_red = "wlp1s0"

# Custom

color_barra = "#282a36"
tamano_barra = 19
fuente_predeterminada = "Ubuntu Mono Nerd Font"
tamano_fuente = 14
color_activo = "#77689e"
tamano_iconos = 17
color_fg = "#ffffff"
color_bg = "#282a36"
color_inactivo = "#6272a4"
color_oscuro = "#44475a"
color_claro = "#bd93f9"
color_urgent = "#ff5555"
color_texto1 = "#bd93f9"
color_updates = "#bc0000"
color_grupo1 = "#ff7f00"
color_grupo2 = "#d600f7"
color_grupo3 = "#67ba18"
color_button_off = "#bc0000"


def fc_separador():
    return widget.Sep(
        linewidth = 0,
        padding = 3,
        foreground = color_fg,
        background = color_bg
    )

def fc_rectangulo(vColor,tipo):
    if tipo == 0:
        icono = ""
    else:
        icono = ""

    return widget.TextBox(
        text = icono,
        fontsize = 19,
        foreground = vColor,
        background = color_bg,
        padding = -4
    )



def fc_icono(icono,color_grupo):
    return widget.TextBox(
        text = icono,
        foreground = color_fg,
        background = color_grupo,
        fontsize = 17
    )


def fc_layout_theme():
    return {
        "margin": 1,
        "border_width": 1,
        "border_focus": "#6790EB",
        "border_normal": "#4c566a"
    }

layout_theme = fc_layout_theme()

#END CUSTOM

keys = [
    # A list of available commands that can be bound to keys can be found
    # at https://docs.qtile.org/en/latest/manual/config/lazy.html
    # Switch between windows
    Key([mod], "h", lazy.layout.left(), desc="Move focus to left"),
    Key([mod], "l", lazy.layout.right(), desc="Move focus to right"),
    Key([mod], "j", lazy.layout.down(), desc="Move focus down"),
    Key([mod], "k", lazy.layout.up(), desc="Move focus up"),
    Key([mod], "space", lazy.layout.next(), desc="Move window focus to other window"),
    # Move windows between left/right columns or move up/down in current stack.
    # Moving out of range in Columns layout will create new column.
    Key([mod, "shift"], "h", lazy.layout.shuffle_left(), desc="Move window to the left"),
    Key([mod, "shift"], "l", lazy.layout.shuffle_right(), desc="Move window to the right"),
    Key([mod, "shift"], "j", lazy.layout.shuffle_down(), desc="Move window down"),
    Key([mod, "shift"], "k", lazy.layout.shuffle_up(), desc="Move window up"),
    # Grow windows. If current window is on the edge of screen and direction
    # will be to screen edge - window would shrink.
    Key([mod, "control"], "h", lazy.layout.grow_left(), desc="Grow window to the left"),
    Key([mod, "control"], "l", lazy.layout.grow_right(), desc="Grow window to the right"),
    Key([mod, "control"], "j", lazy.layout.grow_down(), desc="Grow window down"),
    Key([mod, "control"], "k", lazy.layout.grow_up(), desc="Grow window up"),
    Key([mod], "n", lazy.layout.normalize(), desc="Reset all window sizes"),
    # Toggle between split and unsplit sides of stack.
    # Split = all windows displayed
    # Unsplit = 1 window displayed, like Max layout, but still with
    # multiple stack panes
    Key(
        [mod, "shift"],
        "Return",
        lazy.layout.toggle_split(),
        desc="Toggle between split and unsplit sides of stack",
    ),
    Key([mod], "Return", lazy.spawn(terminal), desc="Launch terminal"),

   
    Key([mod], "m", lazy.spawn("rofi -show drun"), desc="Abrir menu"), 
    Key([mod], "f", lazy.spawn("firefox"), desc="abre firefox"),
    Key([mod], "c", lazy.spawn("code"), desc="abre vscode"),
    Key([mod], "d", lazy.spawn("discord"), desc="abre discord"),


    # Toggle between different layouts as defined below
    Key([mod], "Tab", lazy.next_layout(), desc="Toggle between layouts"),
    Key([mod], "w", lazy.window.kill(), desc="Kill focused window"),
    Key([mod, "control"], "r", lazy.reload_config(), desc="Reload the config"),
    Key([mod, "control"], "q", lazy.shutdown(), desc="Shutdown Qtile"),
    Key([mod], "r", lazy.spawncmd(), desc="Spawn a command using a prompt widget"),
    
    #Control de volumen
    Key([], "XF86AudioLowerVolume", lazy.spawn("pactl set-sink-volume @DEFAULT_SINK@ -5%")),
    Key([], "XF86AudioRaiseVolume", lazy.spawn("pactl set-sink-volume @DEFAULT_SINK@ +5%")),
    Key([], "XF86AudioMute", lazy.spawn("pactl set-sink-mute @DEFAULT_SINK@ toggle")),

    #Brillo de pantalla
    Key([], "XF86MonBrightnessUp", lazy.spawn("brightnessctl set 10%-")),
    Key([], "XF86MonBrightnessDown", lazy.spawn("brightnessctl set +10%")),

    #Captura de pantalla
     Key([mod], "s", lazy.spawn("grim"), desc="hace una captura de pantalla"),
    # Key([mod, "shift"], "s", lazy.spawn("scrot -s"), desc="hace una captura de pantalla"),
]

groups = [Group(i) for i in [
    "  ", "  ", "  ", " ﭮ ", "  ", "  ",
]]

for i, group in enumerate(groups):
    numeroDesck = str(i+1)
    keys.extend(
        [
            # mod1 + letter of group = switch to group
            Key(
                [mod],
                numeroDesck,
                lazy.group[group.name].toscreen(),
                desc="Switch to group {}".format(group.name),
            ),
            # mod1 + shift + letter of group = switch to & move focused window to group
            Key(
                [mod, "shift"],
                numeroDesck,
                lazy.window.togroup(group.name, switch_group=True),
                desc="Switch to & move focused window to group {}".format(group.name),
            ),
            # Or, use below if you prefer not to switch to that group.
            # # mod1 + shift + letter of group = move focused window to group
            # Key([mod, "shift"], i.name, lazy.window.togroup(i.name),
            #     desc="move focused window to group {}".format(i.name)),
        ]
    )

layouts = [
    # layout.Columns(border_focus_stack=["#d11a9f", "#8f3d3d"], border_width=4),
    layout.Max(**layout_theme),
    # Try more layouts by unleashing below layouts.
    # layout.Stack(num_stacks=2),
    layout.Bsp(**layout_theme),
    layout.Matrix(**layout_theme),
    layout.Floating(**layout_theme),
    layout.MonadTall(**layout_theme),
    layout.MonadWide(**layout_theme),
    # layout.RatioTile(),
    # layout.Tile(),
    # layout.TreeTab(),
    # layout.VerticalTile(),
    # layout.Zoomy(),
]

widget_defaults = dict(
    font=fuente_predeterminada,
    fontsize=tamano_fuente,
    padding=2,
)
extension_defaults = widget_defaults.copy()

screens = [
    Screen(
        top=bar.Bar(
            [
                widget.GroupBox(
                    active=color_activo,
                    borderwidth= 1,
                    disable_drag= True,
                    fontsize = tamano_iconos,
                    foreground=color_fg,
                    highlight_method="text",
                    inactive=color_inactivo,
                    margin_x = 0,
                    margin_y = 2,
                    other_current_screen_border=color_oscuro,
                    other_screen_border = color_oscuro,
                    padding_x = 0,
                    padding_y = 10,
                    this_current_screen_border = color_claro,
                    urgent_alert_method = "block",
                    urgent_border = color_urgent
                ),
                widget.Prompt(),
                fc_separador(),
                widget.WindowName(
                    foreground = color_texto1,
                    background = color_bg
                ),
                widget.Systray(
                    icon_size = tamano_iconos,
                    background = color_bg
                ),

                #Notificaciones


                widget.Notify(
                    default_timeout=7,
                    background=color_bg
                ),

                fc_separador(),


                #Notificaciones END

                #grupo 1
                fc_rectangulo(color_grupo1, 0),

                fc_icono("", color_grupo1),
                widget.ThermalSensor(
                    foreground = color_fg,
                    background = color_grupo1,
                    threshold = 50,
                    tag_sensor = "Core 0",
                    fnt = "T1:{}"
                ),
                widget.Memory(
                    foreground = color_fg,
                    background = color_grupo1,
                    measure_mem='G',
                    format="  {MemUsed:.0f}{mm}/{MemTotal:.0f}{mm}"
                ),
                # fc_icono("龍 ", color_grupo1),
                # widget.CPU(
                #     background=color_grupo1,
                #     foreground=color_fg
                # ),
                widget.Wlan(
                    background=color_grupo1,
                    foreground=color_fg,
                    disconnected_message=" 睊 ",
                    interface="wlp1s0",
                    format="   {quality}/100"
                ),
                fc_rectangulo(color_grupo1, 1),
                fc_separador(),
                #grupo 1 END

                #grupo 2

                fc_rectangulo(color_grupo2, 0),

                # fc_icono(" ", color_grupo2),
                # widget.CheckUpdates(
                #     background = color_grupo2,
                #     colour_have_updates = color_updates,
                #     colour_no_updates = color_fg,
                #     no_update_string = "0",
                #     display_format = "{updates}",
                #     update_interval = 1800,
                #     distro = "Arch_checkupdates"
                # ),
                widget.Net(
                    foreground = color_fg,
                    background = color_grupo2,
                    format = "{down} {up}",
                    interface = disposivo_red,
                    use_bits = "true"
                ),

                widget.Battery(
                    charge_char="",
                    discharge_char="",
                    empty_char="",
                    full_char="",
                    unknown_char="",     
                    background=color_grupo2,
                    foreground=color_fg,
                    format=" {char} {percent:2.0%}",
                    update_interval=2
                ),

                fc_rectangulo(color_grupo2, 1),

                #grupo 2 END
                fc_separador(),

                #grupo 3

                fc_rectangulo(color_grupo3, 0),
                widget.Clock(
                    format=" %I:%M %p%a %d-%m",
                    background=color_grupo3,
                    foreground=color_fg,
                ),

                fc_rectangulo(color_grupo3, 1),

                # fc_separador(),
                #grupo 3 END
                fc_separador(),
                # widget.CurrentLayout(),
                fc_rectangulo(color_button_off, 0),
                widget.QuickExit(
                    background=color_button_off,
                    default_text="襤",
                    countdown_format=" {}s",
                    padding=7
                ),
                fc_rectangulo(color_button_off, 1),
                # fc_separador(),
            ],
            tamano_barra,
            # border_width=[2, 1, 2, 0],  # Draw top and bottom borders
            # border_color=["ff00ff", "000000", "ff00ff", "000000"],  # Borders are magenta
            background=color_barra,
            
        ),
    ),
]

# Drag floating layouts.
mouse = [
    Drag([mod], "Button1", lazy.window.set_position_floating(), start=lazy.window.get_position()),
    Drag([mod], "Button3", lazy.window.set_size_floating(), start=lazy.window.get_size()),
    Click([mod], "Button2", lazy.window.bring_to_front()),
]

dgroups_key_binder = None
dgroups_app_rules = []  # type: list
follow_mouse_focus = True
bring_front_click = False
cursor_warp = False
floating_layout = layout.Floating(
    float_rules=[
        # Run the utility of `xprop` to see the wm class and name of an X client.
        *layout.Floating.default_float_rules,
        Match(wm_class="confirmreset"),  # gitk
        Match(wm_class="makebranch"),  # gitk
        Match(wm_class="maketag"),  # gitk
        Match(wm_class="ssh-askpass"),  # ssh-askpass
        Match(title="branchdialog"),  # gitk
        Match(title="pinentry"),  # GPG key password entry
    ]
)
auto_fullscreen = True
focus_on_window_activation = "smart"
reconfigure_screens = True

# If things like steam games want to auto-minimize themselves when losing
# focus, should we respect this or not?
auto_minimize = True

# When using the Wayland backend, this can be used to configure input devices.
wl_input_rules = None

# XXX: Gasp! We're lying here. In fact, nobody really uses or cares about this
# string besides java UI toolkits; you can see several discussions on the
# mailing lists, GitHub issues, and other WM documentation that suggest setting
# this string if your java app doesn't work correctly. We may as well just lie
# and say that we're a working one by default.
#
# We choose LG3D to maximize irony: it is a 3D non-reparenting WM written in
# java that happens to be on java's whitelist.
wmname = "LG3D"

@hook.subscribe.startup_once
def autostart():
    home = os.path.expanduser('~')
    subprocess.Popen([home + '/.config/qtile/autostart.sh'])
