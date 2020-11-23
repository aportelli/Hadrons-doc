# Installation

Install Grid, the options and dependencies can be a bit involved depending on the targeted architecture, see https://github.com/paboyle/Grid. In a nutshell it should look like

```bash
git clone git@github.com:paboyle/Grid.git
cd Grid
./bootstrap.sh
mkdir build
cd build
../configure --prefix=<prefix> ... # ellipsis: architecture specific options
make -j <nprocs>
make install
```

`<prefix>` is the Unix prefix where the package will be installed and `<nproc>` is the number of parallel build tasks.
In another directory, install Hadrons

```bash
git clone git@github.com:aportelli/Hadrons.git
cd Hadrons
./bootstrap.sh
mkdir build
cd build
../configure --prefix=<prefix> --with-grid=<prefix>
make -j <nprocs>
make install
```

The `<prefix>` option must be the same than Grid.