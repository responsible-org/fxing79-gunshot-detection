# gunshot detection
The code (two jupyter notebooks) replicates un-ensembled method for the [Lim's paper](http://www.cs.tut.fi/sgn/arg/dcase2017/documents/challenge_technical_reports/DCASE2017_Lim_204.pdf) of the DCASE-2017 Task 2 http://www.cs.tut.fi/sgn/arg/dcase2017/challenge/task-rare-sound-event-detection. The achieved results seem to attribute partially to the CRNN network architecture and partially to the training data augmentation. 

## Installing the data and making this work
The strategy here is to use a docker container that can run itorch. This is
controlled by a Makefile

- make data. This copies the data from an S3 bucket that has the data expanded.
  See below for how to do this, but you have to do a manual extract into s3. The
script then will push this data into a ../data directory that lives above this.
This is a standard layout for our systems. Have S3 syncrhonize
- make run. This will run a docker container, connect it to the data source in
  the appropriate mountpoint. See below with the -v connection

## Running this with iTorch
Unfortunately, this uses pytorch, so you can't use colab.research.google.com just use File/Open
and select Github. So you need to get itorch running. This isn't easy given how
long it's been since iTorch has been in development. The easiest way to do this
is to run the docker container https://github.com/davidhunteruk/itorch-notebook

## File layout and testing
There are two notebooks:
- basecase.pynb This just tests the files are correctly formed
- drnn.pynb. Once the above works, then you can run training with this.

## Installing the data
This base repo only contains the source code, there are bunch of dependencies on
the data itself. The code assumes that this lives in the repo itself.

You get the data from https://zenodo.org/record/401395 which is the dataset for
the competition. Most of the dataset is not relevant so you need to just get the
gunshot source and the gunshot mixture. The dataset is created by taking some
samples and then overlaying it with background noise which is called the
`mixture` set.

It is confusing which to download, but if you use Git LFS, then you can leave
the data in the repo and use `git lfs track .wav` to make sure that you do not
actually put them into the repo. 

The relevant files to download are noted in the `PATH` variable in the base
notebook, but they should live in the directory. This has a strange name because
the data is synthetically generated from background sounds. And the actual test
include baby crying and glass breaking in addition to gunshots, but there should
be 500 gunshots. 

There is both a devtest and a devtrqin set and you just point the path

You need to extract the files
```
https://zenodo.org/record/401395/files/TUT-rare-sound-events-2017-development.mixture_data.9.zip?download=1


Ij the development file as of January 2020, the GUID used has changed (probably
because there were some errors in the original file, so you need to modify the
path from
```
applications/data/TUT-rare-sound-events-2017-development/generated_data/mixtures_devtrain_0367e094f3f5c81ef017d128ebff4a3c/
```

to the following. This will change if the dataset is ever updated

```
applications/data/TUT-rare-sound-events-2017-development/generated_data/mixtures/devtrain/023402938402834/
```

There is another path that you can use for testing which are the source gunshots
themselves in 

```
'applications/data/TUT-rare-sound-events-2017-development/data/source_data/events/gunshot/'
```

