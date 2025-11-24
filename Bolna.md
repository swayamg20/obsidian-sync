Installing and managing multiple Python versions on macOS is effectively achieved using `pyenv`.

1. Install Homebrew (if not already installed):

Code

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. Install `pyenv` and build dependencies using Homebrew:

Code

```
brew install pyenv openssl readline sqlite3 xz zlib tcl-tk@8 libb2 zstd
```

3. Configure your shell environment for `pyenv`:

Add the following lines to your shell's configuration file (e.g., `~/.zshrc` for Zsh or `~/.bash_profile` for Bash):

Code

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrcecho 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrcecho 'eval "$(pyenv init --no-rehash)"' >> ~/.zshrc
```

(Replace `~/.zshrc` with `~/.bash_profile` if you are using Bash).

Then, reload your shell configuration:

Code

```
source ~/.zshrc
```

(Or `source ~/.bash_profile` for Bash).

4. Install desired Python versions:

First, list available versions:

Code

```
pyenv install -l
```

Then, install a specific version (e.g., Python 3.9.10):

Code

```
pyenv install 3.9.10
```

Repeat for any other versions you require.

5. Manage Python versions:

List installed versions.

Code

```
    pyenv versions
```

- **Set global Python version (default for all new terminals):**

Code

```
    pyenv global 3.9.10
```

- **Set local Python version (for a specific project directory):**

Navigate to your project directory and run:

Code

```
    pyenv local 3.10.0
```

This creates a `.python-version` file in that directory. Create and activate a virtual environment.

Code

```
    pyenv virtualenv 3.9.10 my_project_env    pyenv activate my_project_env
```




twilio recovery: 61D3ET4E52F1KQDPC8QND9M3

assemblyAI: 0d17b1f23b0a4734ae8ed229fce55e31