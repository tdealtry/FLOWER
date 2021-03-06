# FLOWER: soFtware for LOW-Energy Reconstruction
An energy reconstruction script for low-energy events that works with (hk-)BONSAI and other low-energy reconstruction tools - it just requires a positional vertex.

## Compiling
Make sure the following are sourced
* WCSim (`$WCSIMDIR` set)

Then
```bash
export FLOWERDIR=/path/to/FLOWER
make
```

## Running the WCSim/BONSAI script bundled here
Make sure the following are sourced
* WCSim (`$WCSIMDIR` set)
* BONSAI (`$BONSAIDIR` set)
* FLOWER (`$FLOWERDIR` set)

Then
```bash
export PATH=$FLOWERDIR/rootflower:$PATH
export FLOWERDATADIR=/path/to/writable/directory/ #e.g. $FLOWERDIR/data/
rootflower -b -q flower_with_bonsai.C+'("/path/to/wcsim/file.root",1,"SuperK")'
```

Note that `$FLOWERDATADIR` is used to store information that takes a while to calculate, and that only needs to be done once per geometry (e.g. the nearest neighbours of each PMT). If `$FLOWERDATADIR` is not set, or set to a directory that doesn't exist, `$FLOWERDIR/data/` will be used instead

## Running in other code
* Construct a class member, giving it a detectorname (it sets default values and determines the exact energy correction factors) and detector geometry
```
WCSimFlower(const char * detectorname, WCSimRootGeom * geom, int verbose);
```
* Override default values of parameters with these methods
```
void SetDarkRate(double darkrate);
void SetNPMTs   (int npmts);
void SetNWorkingPMTs(int nworkingpmts);
void SetNeighbourDistance(double neighbourdistance);
void SetShortDuration(double shortduration);
void SetLongDuration(double longduration);
void SetTopBottomDistance(double hi, double lo);
```
* Every event/trigger, send it a list of hit times, hit tube IDs, and reconstructed vertex position (x,y,z)
```
double GetEnergy(std::vector<float> times, std::vector<int> tubeIds, float * vertex);
```
