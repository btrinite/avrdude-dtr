Using avrdude with the Raspberry Pi
===================================

Since the Raspberry Pi lacks a DTR pin that makes it oh-so-easy to upload your hex files into
the avr, we need this hack to make it just as easy.  When you wire up your atmega chip, be sure
to connect one of the digital gpio pins to the reset pin, and then you'll be able to use avrdude
as if your serial cable actually had a dtr pin.

Instructions:
-------------

sudo apt-get install strace
sudo apt-get install curl

* Install arduino-cli

curl -fsSL https://raw.githubusercontent.com/arduino/arduino-cli/master/install.sh | sh

* Add 'bin' created directory to your PATH,

nano ~/.bashrc
export PATH=$PATH:/home/pi/bin

* Initialize arduino-cli 

arduino-cli config init
arduino-cli core update-index

* install board support

arduino-cli core install arduino:avr

* install avrdude trick 

cp autoreset /usr/bin
cp avrdude-autoreset /usr/bin
sudo mv ~/.arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/bin/avrdude ~/.arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/bin/avrdude-original
sudo ln -s ~/.arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/bin/avrdude-original /usr/bin/avrdude-original
sudo ln -s /usr/bin/avrdude-autoreset ~/.arduino15/packages/arduino/tools/avrdude/6.3.0-arduino17/bin/avrdude

* install Hat source code and dep

cd project
git clone https://github.com/btrinite/robocars_hat.git
mv robocars_hat RobocarsHat
arduino-cli lib install PololuLedStrip

cd ~/Arduino/libraries/
git clone https://github.com/PaulStoffregen/PWMServo
git clone https://github.com/ElectricRCAircraftGuy/eRCaGuy_TimerCounter.git

* compile


arduino-cli compile -b arduino:avr:mini /home/btrinite/projects/RobocarsHat/

* upload sketch


arduino-cli upload -p /dev/ttyTHS1 --fqbn  arduino:avr:mini /home/btrinite/projects/RobocarsHat/
