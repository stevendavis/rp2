# RP2 v0.5.0

![Main branch](https://github.com/eprbell/rp2/actions/workflows/build.yml/badge.svg)

## Table of Contents
* **[Introduction](#introduction)**
* **[License](#license)**
* **[Download](#download)**
* **[Installation](#installation)**
  * [Ubuntu Linux](#installation-on-ubuntu-linux)
  * [macOS](#installation-on-macos)
  * [Windows 10](#installation-on-windows-10)
  * [Other Unix-like Systems](#installation-on-other-unix-like-systems)
* **[Running](#running)**
  * [Linux, macOS and Other Unix-like Systems](#running-on-linux-macos-and-other-unix-like-systems)
  * [Windows 10](#running-on-windows-10)
* **[Reporting Bugs](#reporting-bugs)**
* **[Contributing](#contributing)**
* **[Documentation](#documentation)**
* **[Frequently Asked Questions](#frequently-asked-questions)**

## Introduction
[RP2](https://github.com/eprbell/rp2) is a privacy-focused, free, open-source cryptocurrency tax calculator. Preparing crypto taxes can be a daunting and error-prone task, especially if multiple transactions, coins, exchanges and wallets are involved. This problem could be delegated to a crypto tax preparation service, but many crypto users value their privacy and prefer not to send their transaction information to third parties unnecessarily. Additionally, many of these services cost money. RP2 solves all of these problems:
- it manages the complexity related to coin flows and tax calculation and it generates forms tax that accountants can understand, even if they are not cryptocurrency experts (e.g. form 8949);
- it prioritizes user privacy by storing crypto transactions and tax results on the user's computer and not sending them anywhere else;
- it's free and open-source.

RP2 reads in a user-prepared spreadsheet containing crypto transactions. It then uses high-precision math to calculate long/short capital gains, cost bases, balances, average price, in/out lot relationships and fractions, and finally it generates output spreadsheets. It supports the FIFO accounting method.

It has a programmable plugin architecture for [output generators](plugin/output): currently only US-specific plugins are available (one for form 8949 and another for a full tax report), but the architecture makes it possible to contribute additional output generators for different countries or for different US-based cases.

RP2 has extensive [unit test](test/) coverage to reduce the risk of regression.

The author of RP2 is not a tax professional, but has used RP2 personally for a few years.

**IMPORTANT DISCLAIMER**: RP2 offers no guarantee of correctness (read the [license](#license)): always verify results with the help of a tax professional.

## License
RP2 is released under the terms of Apache License Version 2.0. For more information see [LICENSE](LICENSE) or http://www.apache.org/licenses/LICENSE-2.0.

## Download
The latest version of RP2 can be downloaded at: https://github.com/eprbell/rp2

## Installation
RP2 has been tested on Ubuntu Linux, macOS and Windows 10 but it should work on all systems that have Python version 3.7.0 or greater.

### Installation on Ubuntu Linux
First make sure Python, pip and virtualenv are installed. If not, open a terminal window and enter the following commands:
```
sudo apt-get update
sudo apt-get install make python3 python3-pip virtualenv
```

Then install RP2 Python package requirements:
```
cd <rp2_directory>
make
```
### Installation on macOS
First make sure [Homebrew](https://brew.sh) is installed, then open a terminal window and enter the following commands:
```
brew update
brew install python3 virtualenv
```

Finally install RP2 Python package requirements:
```
cd <rp2_directory>
make
```
### Installation on Windows 10
First make sure [Python](https://python.org) 3.7 or greater is installed (in the Python installer window be sure to click on "Add Python to PATH"), then open a PowerShell window and enter the following commands:
```
python -m pip install virtualenv
cd <rp2_directory>
virtualenv -p python .venv
```

Finally install RP2 Python package requirements:
```
cd <rp2_directory>
python -m pip install -r requirements.txt
```

### Installation on Other Unix-like Systems
* install GNU make
* install python 3.7 or greater
* install pip3
* install virtualenv
* update PATH variable, if needed
* cd _<rp2_directory>_
* make

## Running
RP2 reads in two user-prepared files:
- an ODS-format spreadsheet, containing crypto transactions (ODS-format files can be opened and edited with [LibreOffice](https://www.libreoffice.org/), Microsoft Excel and many other spreadsheet applications);
- a JSON config file, describing the format of the spreadsheet file.

The formats of these files are described in detail in the [Input Files](doc/input_files.md) section of the documentation.

Examples of an input spreadsheet and its respective config file:
* [input/crypto_example.ods](input/crypto_example.ods)
* [config/crypto_example.config](config/crypto_example.config) (if desired, this config file can be used as boilerplate).

RP2 generates output files based on the received input. The output files contain information on long/short capital gains, cost bases, balances, average price, in/out lot relationships and fractions. They are described in detail in the [Output Files](doc/output_files.md) section of the documentation.

The next sections contain platform-specific information on how to run RP2 on different systems.

### Running on Linux, macOS and Other Unix-like Systems

To generate output for the above example open a terminal window and enter the following commands:
  ```
  cd <rp2_directory>
  . .venv/bin/activate
  bin/rp2.py -o output -p crypto_example_ config/crypto_example.config input/crypto_example.ods
  ```
Results are generated in the `output` directory. Logs are stored in the `log` directory.

To print command usage information for the `rp2.py` command:
  ```
  bin/rp2.py --help
  ```

### Running on Windows 10

To generate output for the above example open a PowerShell window and enter the following commands:
  ```
  cd <rp2_directory>
  .venv\Scripts\activate.ps1
  python bin\rp2.py -o output -p crypto_example_ config\crypto_example.config input\crypto_example.ods
  ```

Results are generated in the `output` directory. Logs are stored in the `log` directory.

To print command usage information for the `rp2.py` command:
  ```
  python bin\rp2.py --help
  ```

## Reporting Bugs
Feel free to submit bugs via Issue Tracker.

**IMPORTANT**: RP2 stores logs and outputs locally on the user's machine and doesn't send this data elsewhere. Logs, inputs and outputs can be useful to reproduce a bug, so a user can decide (or not) to share them to help fix a problem. If you decide to share this information, be mindful of what you post or send out: stack traces are typically free of personal data, but RP2 logs, inputs and outputs, while very useful to reproduce an issue, may contain information that can identify you and your transactions. Before posting such data publicly or even sending it privately to the author of RP2, make sure that:
- the data is sanitized of personal information (although this may make it harder to reproduce the problem), or
- you're comfortable sharing your personal data.

Logs are stored in the `log/` directory and each file name is appended with a timestamp. Outputs are stored in the `output/` directory or where specified by the user with the `-o` option.

## Contributing
Feel free to submit pull requests. [Unit tests](test/) for new code are highly appreciated.

Here is a list of make targets, that are relevant for development:
* `make`: installs package requirements
* `make archive`: creates a .zip file with the contents of the RP2 directory
* `make check`: runs all RP2 unit tests
* `make clean`: removes the virtual environment, logs, outputs, caches, etc.
* `make lint`: analyzes all Python sources with Pylint
* `make reformat`: formats all Python sources using Black
* `make typecheck`: analyzes all Python sources with Mypy static type checker

RP2 follows strictly the PEP 8 coding standard, so, before pushing code, use `make reformat`, which enforces the style guide.

The following make targets perform static and runtime (unit test) checks: they are ran automatically in continuous integration on push and pull requests, but they are also useful while coding (use them liberally on your machine):
* `make check lint typecheck`

The development and test workflows are supported only on Linux and macOS (not on Windows).

Logs are stored in the `log` directory. To generate debug logs, prepend the command line with `LOG_LEVEL=DEBUG`, e.g.:
```
LOG_LEVEL=DEBUG bin/rp2.py -o output -p crypto_example_ config/crypto_example.config input/crypto_example.ods
```

## Documentation
[Documentation](doc/README.md) is available in the [doc](doc/) directory

## Frequently Asked Questions
The [FAQ list](doc/faq.md) is available in the [doc](doc/) directory
