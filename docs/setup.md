# Setup and installation

The following steps will prepare you to run complete analyses of SARS-CoV-2 data by installing required software and running a simple example workflow.

## 1. Make a copy of this tutorial

There are two ways to do this:

  * [Recommended] If you're familiar with git, clone this repository either via the web interface, a GUI such as [GitKraken](https://www.gitkraken.com/), or the command line:

```bash
git clone https://github.com/nextstrain/ncov.git
```

  * [Alternative] If you're not familiar with git, you can also download a copy of these files via the buttons on the left.

## 2. Setup your Nextstrain environment

Create a Nextstrain conda environment with augur and auspice installed.
If you do not have conda installed already, [see our full installation instructions for more details](https://nextstrain.org/docs/getting-started/local-installation).
If you are running Windows, [see our documentation about setting up the Windows Subsystem for Linux (WSL)](https://nextstrain.org/docs/getting-started/windows-help).

```bash
curl http://data.nextstrain.org/nextstrain.yml --compressed -o nextstrain.yml
conda env create -f nextstrain.yml
conda activate nextstrain
npm install --global auspice
```

## 3. Run a basic analysis with example data

Run a basic workflow with example data, to confirm that your Nextstrain environment is properly configured.
First, change into the `ncov` repository's directory.

```bash
cd ncov
```

Then, uncompress the example sequence data we include in the repository.

```bash
gzip -d -c data/example_sequences.fasta.gz > data/example_sequences.fasta
```

Finally, run the basic workflow with these example data.

```bash
snakemake --cores 4 --profile ./my_profiles/getting_started
```

The `getting_started` profile produces a minimal global phylogeny for visualization in auspice.
This workflow should complete in about 5 minutes on a MacBook Pro (2.7 GHz Intel Core i5) with four cores.

## 4. Visualize the phylogeny for example data

Go to [http://auspice.us](http://auspice.us) in your browser.
Drag and drop the JSON file `auspice/ncov_global.json` anywhere on the [http://auspice.us](http://auspice.us) landing page, to visualize the resulting phylogeny.

## Advanced reading: considerations for keeping a 'Location Build' up-to-date

_Note: we'll walk through what each of the referenced files does shortly_

### Keeping data updated
If you are aiming to create a public health build for a state, division, or area of interest, you likely want to keep your analysis up-to-date easily.
If your run contains contextual subsampling (sequences from outside of your focal area), you should first ensure that you [regularly download the latest sequences as input](data-prep.md), then re-run the build.
This way, you always have a build that reflects the most recent SARS-CoV-2 information.

### Keeping your workflow updated
You should also aim to keep this `ncov` repository updated.
If you've clone the repository from Github, this is done by running `git pull`.
This downloads any changes that we have made to the repository to your own computer.
In particular, we add [new colors and latitute & longitude information](customizing-analysis.md) regularly - these should match the new sequences you download, so that you don't need to add this information yourself.

If you don't need to share the contents of [`my_profiles`](orientation-files.md) (the files that parameterize your specific analysis) with anyone, then you can leave this in the `./my_profiles/` folder.
It won't be changed when you `git pull` for the latest information.

However, if you want to share your profile, you'll need to adopt one of the following solutions.
First, you can 'fork' the entire `ncov` repository, which means you have your own copy of the repository.
You can then add your profile files to the repository and anyone else can download them as part of your 'fork' of the repository.
Note that if you do this, you should ensure you `pull` regularly from the original `ncov` repository to keep it up-to-date.

Alternatively, you can create a new, separate repository to hold your `my_profiles` files, outside of the `ncov` repository.
You can then share this repository with others, and it's straightforward to keep `ncov` up to date, as you don't change it at all.
If doing this, it can be easiest to create a `my_profiles` folder and imitate the structure found in the `./my_profiles` folder , but this isn't required.
Note that to run the build you'll need still run the `snakemake` command from within the `ncov` repository, but specify that the build you want is outside that folder.

For the [`south-usa-sarscov2`](https://github.com/emmahodcroft/south-usa-sarscov2/) example, you can see the `south-central` build set up in a `profiles` folder.
To run this, one would call the following from within `ncov`:

```bash
snakemake --cores 1 --profile ../south-usa-sarscov2/profiles/south-central/
```

## [Next Section: Preparing your data](data-prep.md)
