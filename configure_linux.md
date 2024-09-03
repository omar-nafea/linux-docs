To add Arabic support to both your system locale and keyboard on Arch Linux, follow these steps:
1. Add Arabic Locale

    Edit the Locale Configuration:
        Open the locale configuration file with your preferred text editor:

        bash

sudo nano /etc/locale.gen

Uncomment the Arabic locale by finding the line # ar_EG.UTF-8 UTF-8 and removing the # at the beginning. You can also uncomment other Arabic locales if needed.

    ar_EG.UTF-8 UTF-8

Generate the Locale:

    After saving the changes to /etc/locale.gen, generate the locales:

    bash

    sudo locale-gen

Set the Default Locale:

    To set the default locale to Arabic or to ensure it's available, add it to the /etc/locale.conf file:

    bash

sudo nano /etc/locale.conf

Add or modify the following line to include the Arabic locale:

makefile

LANG=ar_EG.UTF-8

If you want the default to remain in English but allow Arabic, you can add both:

makefile

        LANG=en_US.UTF-8
        LANGUAGE=en_US:ar_EG

    Apply the Locale Settings:
        Apply the locale settings by restarting your system or logging out and back in.

2. Add Arabic Keyboard Layout

    Configure X11 for Arabic Keyboard Layout:
        Create or edit the X11 keyboard configuration file:

        bash

sudo nano /etc/X11/xorg.conf.d/00-keyboard.conf

Add or update the configuration to include both English and Arabic layouts:

bash

        Section "InputClass"
            Identifier "system-keyboard"
            MatchIsKeyboard "on"
            Option "XkbLayout" "us,ara"
            Option "XkbVariant" ","
            Option "XkbOptions" "grp:win_space_toggle"
        EndSection

        This configuration sets the keyboard layouts to English (US) and Arabic (ara) and uses the Win + Space key combination to toggle between them.

    Restart X11 or Reboot:
        Restart your X11 session or reboot your system to apply the changes.

3. Verify and Test

    Switch Layouts:
        Use the Win + Space key combination to switch between English and Arabic keyboard layouts.
    Test Input:
        Open a text editor or terminal and try typing in Arabic to ensure the layout switch works correctly.

4. Configure st Terminal for Arabic Font Support

    Set Font in st Terminal:
        If you're using the st terminal and want Arabic font support, you'll need to ensure you have a compatible font installed that supports Arabic script. You can set this in the st configuration (config.h) before compiling st.
        For example, if using a font like Noto Sans Arabic:

        c

    static char *font = "Noto Sans Arabic:size=12";

    Recompile and install st after setting the font.

Ensure Font Coverage:

    Verify that your selected font has coverage for Arabic characters. You can install ttf-arabeyes or noto-fonts for better Arabic font support:

    bash

        sudo pacman -S ttf-arabeyes noto-fonts

    Check Scrolling in st:
        If scrolling in st isn't working, make sure you have a scrollback patch applied to st and are using a compatible terminal multiplexer like tmux if necessary.

These steps should enable Arabic support for both your system locale and keyboard layout, and ensure that you can toggle between English and Arabic layouts with ease.

Touchpad and Keyboard Configuration

    Touchpad:
        Use xinput to configure touchpad settings. You can list devices and configure them with:

        bash

xinput list
 Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ Lenovo Lenovo Gaming Mouse              	id=14	[slave  pointer  (2)]
⎜   ↳ BY Tech Gaming Keyboard Mouse           	id=18	[slave  pointer  (2)]
⎜   ↳ MSFT0001:00 04F3:3140 Mouse             	id=11	[slave  pointer  (2)]
⎜   ↳ BY Tech Gaming Keyboard Consumer Control	id=15	[slave  pointer  (2)]
⎜   ↳ MSFT0001:00 04F3:3140 Touchpad          	id=12	[slave  pointer  (2)]
⎣ Virtual core keyboard                   	id=3	[master keyboard (2)]
    ↳ Virtual core XTEST keyboard             	id=5	[slave  keyboard (3)]
    ↳ Power Button                            	id=6	[slave  keyboard (3)]
    ↳ Video Bus                               	id=8	[slave  keyboard (3)]
    ↳ Video Bus                               	id=7	[slave  keyboard (3)]
    ↳ Power Button                            	id=9	[slave  keyboard (3)]
    ↳ BY Tech Gaming Keyboard System Control  	id=19	[slave  keyboard (3)]
    ↳ AT Translated Set 2 keyboard            	id=13	[slave  keyboard (3)]
    ↳ BY Tech Gaming Keyboard                 	id=20	[slave  keyboard (3)]
    ↳ BY Tech Gaming Keyboard                 	id=17	[slave  keyboard (3)]
    ↳ BY Tech Gaming Keyboard Consumer Control	id=16	[slave  keyboard (3)]
    ↳ Ideapad extra buttons                   	id=10	[slave  keyboard (3)]
[omar@nafea ~]$ xinput set-prop 12 "libinput Tapping Enabled" 1
