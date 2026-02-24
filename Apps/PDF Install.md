
```
sudo apt remove -y wkhtmltopdf
```
```
sudo apt install -y xfonts-75dpi xfonts-base fontconfig
```

## Download wkhtmltopdf 0.12.6 (patched qt) .deb for your OS
## (Pick the correct .deb for your Ubuntu version)
## Example (commonly works on Ubuntu 20/22):
```
wget -O wkhtml.deb https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.focal_amd64.deb
```
```
sudo dpkg -i wkhtml.deb || sudo apt -f install -y
```
```
wkhtmltopdf --version
```
