# soedit-deb
APT packaging for the Sanos Text Editor

The Sanos Text Editor is a simple text-mode application that uses the same key bindings as most GUI text editors. It was written my Michael Ringguard. More details can be found [here](http://www.jbox.dk/sanos/editor.htm).

It has been packaged using autools in an upstream project called [soedit](https://github.com/rajch/soedit).

This project creates an APT package using soedit as the base, also called soedit. 

## Apt Repo
This GitHub repository also hosts an APT repo via GitHub Pages, signed and maintained by Raj Chaudhuri. The repo containes soedit packaged for the 64-bit versions of:

* Debian: buster, stretch and jessie
* Ubuntu: disco, bionic and xenial


## To Install soedit
1. Add the GPG key which was used to sign the repo:
   ```bash
   $ wget -O - http://rajch.github.com/soedit-deb/gpg | sudo apt-key add -
   ```
   Verify that you now have the key with the fingerprint `9B61312E44364B1B1B4E06CE36ECFDFD204EF325`.
2. Use to following command to set up the repository:
   ```bash
   $ sudo add-apt-repository \
      "deb [arch=amd64] https://rajch.github.io/soedit-deb \
      $(lsb_release -cs) \
      main"
   ```
3. Update the `apt` package index:
   ```bash
   $ sudo apt update
   ```
4. Install soedit:
   ```bash
   $ sudo apt install soedit
   ```
5. Verify soedit is installed by running:
   ```bash
   $ soedit
   ```
   You can exit by pressing `Ctrl+q`.


