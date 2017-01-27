#!/bin/bash
#
# Easy GRBL flashing script
#
# Rob Street Jan. 27th, 2011
#
# Execute from the folder containing the .hex file that you want to flash.
# User is prompted for paths to their avrdude, avrdude.conf and Arduino device.
# These values are stored in grbl.conf for future use. If nothing is entered it uses and stores default values.
#

clear;

echo '__/\\\\\\\\\\\\_____/\\\\\\\\\_______/\\\\\\\\\\\\\_____/\\\____________________'
echo '_/\\\//////////____/\\\///////\\\____\/\\\/////////\\\__\/\\\___________________'
echo '_/\\\______________\/\\\_____\/\\\____\/\\\_______\/\\\__\/\\\__________________'
echo '_\/\\\____/\\\\\\\__\/\\\\\\\\\\\/_____\/\\\\\\\\\\\\\\___\/\\\_________________'
echo '__\/\\\___\/////\\\__\/\\\//////\\\_____\/\\\/////////\\\__\/\\\________________'
echo '___\/\\\_______\/\\\__\/\\\____\//\\\____\/\\\_______\/\\\__\/\\\_______________'
echo '____\/\\\_______\/\\\__\/\\\_____\//\\\___\/\\\_______\/\\\__\/\\\______________'
echo '_____\//\\\\\\\\\\\\/___\/\\\______\//\\\__\/\\\\\\\\\\\\\/___\/\\\\\\\\\\\\\___'
echo '______\////////////______\///________\///___\/////////////_____\/////////////___'
echo '                                  Flash script'
echo '                          V1.0 Rob Street 29/01/2011'
echo #/n
echo #/n

# Default values
def1=/home/$USER/arduino-1.0/hardware/tools/avrdude
def2=/home/$USER/arduino-1.0/hardware/tools/avrdude.conf
def3=/dev/ttyACM0

# Is this the right folder?

for file in ./*.hex
do
    if [ -f "${file}" ]; then
    break
       else
	echo 'No .Hex files found!'
	echo #/n
        echo 'Run this script from the folder that has the .hex file that you want to flash.'
	echo #/n
        exit
    fi
done

# does grbl.conf exist?
if [ -e grbl.conf ]
   then
      echo 'grbl.conf found, using previous settings.'
      echo #/n

      #Read from grbl.conf

      old_IFS=$IFS
      IFS=$'\n'
      line=($(cat grbl.conf)) # array accessed by ${line[0]} this equals value of line 0
      IFS=$old_IFS

      #Write file contents into variables

      avrpath=${line[0]}
      avrconfpath=${line[1]}
      device=${line[2]}
   else
      #Ask user for input

      echo "Hi $USER, grbl.conf was not found, let's create a new one."
      echo #/n
      echo 'I need to know where your AVRDUDE binary is located, '
      echo 'if your arduino IDE is installed to your home directory then the default value '
      echo 'should be fine, just press enter, if not, it can usually be found in the '
      echo 'Arduino IDE folder under hardware/tools.'
      echo #/n
      echo "Enter your AVRDUDE binary path and press enter"
      echo "(default=$def1)"
      read avrpath

      echo #/n
      echo ' * * * '
      echo #/n
      echo "Ok, your AVRDUDE binary is located at:"
      echo " ${avrpath:-$def1}"
      echo #/n
      echo "Next we need the path for the AVRDUDE.conf file, if you accepted the default "
      echo "value above, you can do so here as well, otherwise enter the path for your "
      echo "AVRDUDE.conf and press enter"
      echo "(default=$def2)"
      read avrconfpath

      echo #/n
      echo ' * * * '
      echo #/n
      echo "Ok, your AVRDUDE.conf is located at:"
      echo "${avrconfpath:-$def2}"
      echo #/n
      echo "Now we need the name of your Arduino serial device, "
      echo "the easiest way to find this is by plugging in your Arduino, "
      echo "opening a terminal and entering dmesg."
      echo #/n
      echo "Look for ttyACM*, that's your Arduino. "
      echo "The default value of $def3 is usually fine." 
      echo #/n
      echo 'Press enter to accept the default or enter your device name and press enter.'
      read $device
      echo #/n
      echo ' * * * '
      echo #/n
      echo "Ok, your Arduino is located at:"
      echo "${device:-$def3}"

      #Write grbl.conf
      echo 'Saving settings to grbl.conf'
      echo #/n

      echo ${avrpath:-$def1} >> grbl.conf
      echo ${avrconfpath:-$def2} >> grbl.conf
      echo ${device:-$def3} >> grbl.conf

      #Read from grbl.conf

      old_IFS=$IFS
      IFS=$'\n'
      line=($(cat grbl.conf)) # array accessed by ${line[0]} this equals value of line 0
      IFS=$old_IFS

      #Write file contents into variables

      avrpath=${line[0]}
      avrconfpath=${line[1]}
      device=${line[2]}

fi


echo 'Enter the filename and extension of the hex file you want to flash:'
read hexfile
echo #/n
echo ""$avrpath" -C"$avrconfpath" -pm328p -carduino -P"$device" -D -Uflash:w:"$hexfile""
echo #/n
echo 'Okay, so we will be executing the command above, ok y/[n]?'

read yesno
echo #/n
case $yesno in

	Y|y) echo 'Continuing to flash'
	echo #/n
	$avrpath -C$avrconfpath -pm328p -carduino -P$device -D -Uflash:w:$hexfile
	;;

	N|n) echo 'User chose not to flash, exiting.'
	echo #/n
	exit
	;;

	*) echo 'Invalid choice, exiting without flashing'
	echo #/n
	exit
	;;

esac
