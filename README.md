# Try a Java Demo on Dragonboard410c


Assume you are working on a dragonboard410c with Debian, you’ve attached sensors mezzanine to the baseboard, powered it up, attached it to an HDMI monitor with a keyboard and mouse, and you have connected to WiFi, or installed a USB-Ethernet dongle and have that hooked up. In any event, you must be connected to the Internet.

## Install packages

---

    $ sudo apt-get install git build-essential autoconf automake libtool swig3.0 python3-dev pkg-config default-jdk

This will run for a bit, depending on your connection. It will download a bunch of software and install it. If prompted for Y/N select Y. Once it’s finished run:

    $ sudo apt-get clean


## Install libsoc

---

    $ git clone https://github.com/jackmitch/libsoc.git<Enter>
    $ cd libsoc<Enter>
    $ autoreconf -i<Enter>
    $ ./configure --disable-debug --enable-python=3 --enable-board=dragonboard410c ----with-board-configs <Enter>
    $ make && sudo make install<Enter>
    $ sudo ldconfig /usr/local/lib<Enter>

#### Add PYTHONPATH 

Add following line to ~/.bashrc or /etc/bash.bashrc: 

    export PYTHONPATH=$PYTHONPATH:/usr/local/lib/python3.4/site-packages/

Add following line to /etc/sudoers: (If encounter error "no module named libsoc" when running sudo ./xx.py, this is a solution.)

    Defaults env_keep += "PYTHONPATH"
    (remove   Defaults !env_reset   from sudoers file if present)

## Run the java demo and check the result

---

#### Download led_touch.jar

    $ git clone https://github.com/LiveWang/libsoc_jna_demo.git<Enter>

#### Setup the Hardware

    Attach the touch sensor to G2 (reading from GPIO-E)
    Attach the led to G3 (driving GPIO-G)

#### Run the Demo

    $ cd libsoc_jna_demo
    $ sudo java -jar led_touch.jar

Tap the touch sensor with your finger. Tap the touch sensor several times in different speed (you can try tap very fast, or keep your finger on the touch sensor for several seconds and move away or any other speed).

Check the result on your screen, number after highs should equal to number after lows every time you move your finger away from the touch sensor. If not, some interrupts were missed by the board.



