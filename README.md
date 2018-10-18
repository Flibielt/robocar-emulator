# Introduction #
Robocar World Championship (OOCWC = rObOCar World Championship) 
is intended to offer a common research platform for developing urban traffic control algorithms and for investigating the relationship between smart cities and robot cars with particular attention to spread of robot cars of the near future. At the heart of this initiative is the Robocar City Emulator. It will enable researchers to test and validate their theories and models.

The following figure shows a high-level "Tetris" plan of the project
![http://m.cdn.blog.hu/pr/progpater/image/tetris_plan2.400x.png](http://m.cdn.blog.hu/pr/progpater/image/tetris_plan2.400x.png)

  * **Map**: Competitions are played on an OSM ([OpenStreetMap](https://www.openstreetmap.org/about)) map.
  * **City**: Each competition is assigned to a city.
  * **ASA**: (Automated Sensor Annotations) This subsystem will automatically collect traffic data using video cameras. The collected data will be used in the [smartcity](https://code.google.com/p/robocar-emulator/source/browse/justine/rcemu/src/smartcity.hpp) program.
  * **HSA**: (Human controlled Sensor Annotations)
  * **Robocar City Emulator**:
  * **The competition**:
  * **Results**:
  * **Monitors**:

We already have a working prototype called [Justine](https://github.com/nbatfai/robocar-emulator/tree/master/justine). In the spirit of RERO (_"release early, release often"_) it has been released in _"version 0.1 Police edition"_ for the competition called _"Debrecen 1"_. This initial rapid prototype has very limited features but we think it can be used to start building a community of people who share an interest in this subject.
  * Each competition is assigned to a city. Competitions are played on a rectangular area of the OSM ([OpenStreetMap](https://www.openstreetmap.org)) map of the given city that is defined by GPS coords minlat, minlon, maxlat and maxlon. This area is referred to as the "City Operating Area".
  * [Routine cars](https://github.com/nbatfai/robocar-emulator/blob/master/justine/rcemu/src/car.hpp), [smart cars and guided cars](https://github.com/nbatfai/robocar-emulator/blob/master/justine/rcemu/src/car.hpp) may be placed onto the City Operating Area in which cop multiagents pursue gangster agents for ten minutes. Cops are guided cars and these are controlled by the competing teams.

## SamuSmartCity - Everybody has been travelling in a car since nobody got a car.

This is a new edition of Police edition of OOCWC which has been called "There is no own car"
edition, for detailed information see https://github.com/nbatfai/SamuSmartCity 

## Hiba javítások és használat:

### Előkészületek:
sudo apt-get install git

sudo apt-get install autoconf

sudo apt-get install libtool

sudo apt-get install protobuf-compiler libprotobuf-dev

sudo apt-get install libboost-all-dev

sudo apt-get install flex

sudo apt-get install libexpat1-dev

sudo apt-get install zlib1g-dev

sudo apt-get install libbz2-dev

sudo apt-get install libsparsehash-dev

sudo apt-get install libgdal-dev

sudo apt-get install libgeos++-dev

sudo apt-get install libproj-dev

sudo apt-get install doxygen graphviz xmlstarlet

sudo apt-get install cmake

sudo apt-get install maven

sudo apt-get install automake

#### Home könyvtárban:
mkdir libosmium

cd libosmium

Ide le kell tölteni a libosmiumot, mivel az új verzóval már nem működik a robotautó, ezért egy régbbi verziót kell használni. Ez a verzió valószínűleg jó lesz: https://github.com/osmcode/libosmium/tree/v2.10.3

Tömörítsük ki a zip-et.

cd libosmium-2.10.3

cmake .

make

sudo cp -R ../libosmium-2.10.3/include/* /usr/local/include

Ez sok időt és tárhelyet (~3GB) vesz igénybe.

#### Home könyvtárból:
mkdir OSM-binary

cd OSM-binary

git clone https://github.com/scrosby/OSM-binary.git

cd OSM-binary/

make -C src

sudo make -C src install

#### A robotautó előkészítése:
git clone https://github.com/Flibielt/robocar-emulator.git

cd robocar-emulator-master/justine/rcemu/

autoreconf --install

./configure

Az src mappában lévő Makefile 385. sorát egészítsük -lrt kiegészítéssel. Így kell kinéznie:

<code>SHM_OPEN_LIBS = -lrt</code>

#### Futtatás:
##### rcemu mappából:

src/smartcity --osm=../debrecen.osm --city=Debrecen --shm=DebrecenSharedMemory --node2gps=../debrecen-lmap.txt

src/traffic --port=10007 --shm=DebrecenSharedMemory

##### rcwin mappából:

mvn clean compile package site assembly:assembly

java -jar target/site/justine-rcwin-0.0.16-jar-with-dependencies.jar ../debrecen-lmap.txt
