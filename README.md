# mydeb (prepare the deb file in the github repo)
```bash
sudo apt install gnupg && gpg --full-gen-key
```
```bash
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? 1

RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096

Please specify how long the key should be valid.
0 = key does not expire
<n> = key expires in n days
<n>w = key expires in n weeks
<n>m = key expires in n months
<n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

Real name: My Name
Email address: ${EMAIL}
Comment:
You selected this USER-ID:
"My Name <my.name@email.com>"
Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
```
```bash
gpg --export-secret-keys "hetongapp@gmail.com" > my-private-key.asc
```
```bash
gpg --import my-private-key.asc
```
# IN YOUR REPO FOLDER
```bash
gpg --armor --export "hetongapp@gmail.com" > /path/to/my_ppa/KEY.gpg 
```
```bash
dpkg-scanpackages --multiversion . > Packages
gzip -k -f Packages
```
```bash
apt-ftparchive release . > Release
gpg --default-key "hetongapp@gmail.com" -abs -o - Release > Release.gpg
gpg --default-key "hetongapp@gmail.com" --clearsign -o - Release > InRelease
```
```bash
echo "deb [signed-by=/etc/apt/trusted.gpg.d/mydeb.gpg] https://raw.githubusercontent.com/HETONGAPP/mydeb/master/ /" > my_list_file.list
```
# PUSH THE REPO

# TEST
```bash
curl -s --compressed "https://raw.githubusercontent.com/HETONGAPP/mydeb/master/KEY.gpg" | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/mydeb.gpg >/dev/null
```
```bash
sudo curl -s --compressed -o /etc/apt/sources.list.d/my_list_file.list "https://raw.githubusercontent.com/HETONGAPP/mydeb/master/my_list_file.list"
```
```bash
sudo apt update && sudo apt list --upgradable
sudo apt install webrtc
```


# HOW TO ADD NEW DEB FILES
```bash
# Packages & Packages.gz
dpkg-scanpackages --multiversion . > Packages
gzip -k -f Packages

# Release, Release.gpg & InRelease
apt-ftparchive release . > Release
gpg --default-key "hetongapp@gmail.com" -abs -o - Release > Release.gpg
gpg --default-key "hetongapp@gmail.com" --clearsign -o - Release > InRelease
```

# THEN PUSH
