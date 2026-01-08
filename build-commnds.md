# Steps of UAV-Simulation with commands:
- pre-requisites installation
git submodule update --init --recursive --force

./Tools/environment_install/install-prereqs-{os}.sh 
git submodule foreach --recursive git clean -fdx
git submodule foreach --recursive git reset --hard HEAD
git submodule update --init --recursive --force

1. *Docker-based setup/run:*
```
bash docker/run.bash
```

2. *ardupilot build instructions:*
```
cd ardupilot
CC=/usr/bin/clang CXX=/usr/bin/clang++ ./waf configure --board sitl
./waf copter


./waf distclean
./waf configure --board sitl
./waf build
./waf build --target bin/arducopter
build/sitl/bin/arducopter -S --model quad --speedup 1 
or 
./Tools/autotest/sim_vehicle.py -v ArduCopter --console --map
```

3. *hakoniwa-drone-core build instructions:*
```
in docker container shell:
pip3 install hakoniwa-pdu hakopy

cd hakoniwa-drone-core
