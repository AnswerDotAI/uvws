# uv workspace

`uvws` is a way to get something like a conda base env or homedir python, but for uv. uv is far from being a full replacement for conda, but if you only need python packages then with uvws you may find it sufficient.

## Setup

First, install uv if you haven't already, using the official script:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

If you've already installed uv, you might want to ensure it's up to date:

```bash
uv self update
```

Now you're ready to install uvws. First, fork the repo. Then clone your fork (replace YOURNAME below of course)--I clone it into my home dir for convenience--and create and activate the uvws environment:

```bash
cd
git clone git@github.com:YOURNAME/uvws.git
cd uvws
uv sync
source .venv/bin/activate
```

You will probably want to have the env auto-activated in all your shells, so run (modifying the location and shell rc file name as needed):

```bash
echo source ~/uvws/.venv/bin/activate >> ~/.bashrc
```

## Usage

To pip install packages, just prepend the using `pip install` with `pip`, for instance:

```bash
uv pip install torch torchvision torchaudio
```

A big benefit of uvws is that you can automatically setup all your machines with the same packages. To do so, instead of using `uv pip install`, use `uv add`; this also does an install, but also adds the package to uvws's `pyproject.toml`, so when you clone your forked repo and `uv sync` on another machine, you'll get the same packages.

```bash
cd ~/uvws
uv add nbclassic # or whatever package you want
```

You also probably want to get an editable install of packages you're working on. To do so, first edit `repos.txt` to list each package you want to clone and editable install; each line should be in the format `org_or_user/repo`. Then to clone them all in parallel (auto skipping any you already have), run:
```bash
./clone.sh 
```

To add these as editable workspace packages, run:

```bash
./addnew.py 
```

At any point you can quickly see the git status for all repos, using:
```bash
./status.sh
```

And, you can do a `git pull` of all in parallel using:
```bash
./update.sh
```

If at any point you want to reset the env entirely, run:
```bash
cd ~/uvws
# uncomment to run this:
# rm -rf .venv uv.lock 
```

You can also delete you `pyproject.toml` if you wish to reset your `git add` and `./addnew.py` changes. Replace it with `pyproject.tmpl` to get the fresh version (i.e `cp pyproject.tmpl pyproject.toml`)

## Conda alternative

If you need conda (e.g because you'd like to be able to switch CUDA environments easily) but also want to use `uv sync`, check out this clever technique: [warner-benjamin/uv-tools](https://github.com/warner-benjamin/uv-tools).
