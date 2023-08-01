# PyPICache
## A Raspberry Pi-powered PyPI cache
![Raspberry PI](images/raspberry-pi-950490_640.webp)


PyPICache is a relatively easy-to-make server that serves content from the Python Packaging Index and makes it available offline using a Raspberry Pi, allowing you to use `pip` without Internet access. 

To set up PyPICache initially, you need an Internet connection (to download the required software and clone PyPI), and a Raspberry Pi(or similar computer) with a wireless chip. The PyPICache device creates a WIFI network that laptops or any WIFI-enabled devices can connect to and install Python packages from. It clones the PyPI repository to local storage, organises the packages, and serves them using [`pypiserver`](https://pypi.org/project/pypiserver/) and `nginx` as a caching and reverse proxy.
# Table of Contents

- [Tutorial](tutorial.md#tutorial)
- [How TOs](howto.md#how-to-guides)