# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  # allow X-Forwarding
  config.ssh.forward_x11 = true

  config.vm.provision :shell, :inline => $BOOTSTRAP_SCRIPT # see below
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, '--audio', 'coreaudio', '--audiocontroller', 'hda'] # choices: hda sb16 ac97
  end
end

$BOOTSTRAP_SCRIPT = <<EOF
  set -e # Stop on any error

  # --------------- SETTINGS ----------------
  # Other settings
  export DEBIAN_FRONTEND=noninteractive

  sudo apt-get update

  # ---- OSS AUDIO
  sudo usermod -a -G audio vagrant
  sudo apt-get install -y oss4-base oss4-dkms oss4-source oss4-gtk linux-headers-3.2.0-23 debconf-utils
  sudo ln -s /usr/src/linux-headers-$(uname -r)/ /lib/modules/$(uname -r)/source || echo ALREADY SYMLINKED
  sudo module-assistant prepare
  sudo module-assistant auto-install -i oss4 # this can take 2 minutes
  sudo debconf-set-selections <<< "linux-sound-base linux-sound-base/sound_system select  OSS"
  echo READY.

  sudo apt-get install -y build-essential python-dev ipython-notebook \
  python-setuptools python-numpy python-scipy libatlas-dev \
  libfreetype6-dev libatlas3gf-base python-pip python-pandas python-nose \
  libatlas3gf-base python-pip python-pandas python-nose python-audioread \
  pandoc python-pygame cython wget unzip pkg-config libyaml-dev git openssh-client \
  libavcodec-dev libavformat-dev libtag1-dev libfftw3-dev libfftw3-doc libsamplerate0-dev libav-tools

  pip install --upgrade distribute
  pip install --upgrade scikit-learn
  pip install pylint==1.2.0
  pip install --upgrade matplotlib
  pip install --upgrade ipython
  pip install bokeh
  pip install jsonschema

  # ---- GIT
  sudo apt-get install -y git

  # ---- sms-tools
  git clone https://github.com/MTG/sms-tools.git

  ## essentia (http://essentia.upf.edu/documentation/installing.html)
  git clone https://github.com/soundcloud/essentia.git
  (cd essentia && git reset --hard 3326000)
  (cd essentia && ./waf configure --mode=release --with-python --with-cpptests --with-examples --with-v && ./waf && ./waf install)
  easy_install scikits.samplerate

  # have to reboot for drivers to kick in, but only the first time of course
  if [ ! -f ~/runonce ]
  then
    sudo reboot
    touch ~/runonce
  fi
EOF
