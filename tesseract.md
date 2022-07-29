# Install on CentOS, SUSE, etc
### Install 4.1.1
```
yum install -y tesseract
```

### Install 4.1.3
https://tesseract-ocr.github.io/tessdoc/InstallationOpenSuse.html
```  
dnf config-manager --add-repo https://download.opensuse.org/repositories/home:/Alexander_Pozdnyakov/CentOS_8/
rpm --import https://build.opensuse.org/projects/home:Alexander_Pozdnyakov/public_key
dnf install tesseract
dnf install tesseract-langpack-eng
```
### Install 5.1
https://tesseract-ocr.github.io/tessdoc/InstallationOpenSuse.html
```  
dnf config-manager --add-repo https://download.opensuse.org/repositories/home:/Alexander_Pozdnyakov:/tesseract5/CentOS_8/
rpm --import https://build.opensuse.org/projects/home:Alexander_Pozdnyakov/public_key
dnf install tesseract
# dnf install tesseract-langpack-eng. # no need for this. eng is already installed
```



# Compile Tesseract and Leptonica in Rocky Linux 8
should also work with AlmaLinux 8 and Redhat 8

## Compile leptonica
```shell
conda install -c conda-forge gnuplot  # install gnuplot.  I am using conda
yum install -y libjpeg-devel
yum install -y libtiff-devel
yum install -y  libwebp-devel
yum install -y libpng-devel
git clone https://github.com/DanBloomberg/leptonica.git
cd leptonica
./autogen.sh
LDFLAGS="-Wl,-rpath -Wl,/usr/lib64" ./configure
make
make install
make check
```


## Compile Tesseract 5 ( not working)


```shell
git clone https://github.com/tesseract-ocr/tesseract.git
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/     # IMPORTANT: this where leptonica package info resides
./autogen.sh
./configure
make
make install
make check   # verify compilation
```

### make error message

```
In file included from src/viewer/svmnode.cpp:35:
src/viewer/svmnode.h:38:16: error: variable 'tesseract::TESS_API tesseract::SVMenuNode' has initializer but incomplete type
 class TESS_API SVMenuNode {
                ^~~~~~~~~~
src/viewer/svmnode.h:39:1: error: expected primary-expression before 'public'
 public:
 ^~~~~~
src/viewer/svmnode.h:39:1: error: expected '}' before 'public'
src/viewer/svmnode.h:38:27: note: to match this '{'
 class TESS_API SVMenuNode {
                           ^
src/viewer/svmnode.h:39:1: error: expected ',' or ';' before 'public'
 public:
 ^~~~~~
src/viewer/svmnode.h:44:14: error: expected class-name before '(' token
   ~SVMenuNode();
              ^
src/viewer/svmnode.h:48:3: error: 'SVMenuNode' does not name a type
   SVMenuNode *AddChild(const char *txt);
   ^~~~~~~~~~
src/viewer/svmnode.h:69:1: error: expected unqualified-id before 'private'
 private:
 ^~~~~~~
src/viewer/svmnode.h:75:17: error: variable or field 'AddChild' declared void
   void AddChild(SVMenuNode *svmn);
                 ^~~~~~~~~~
src/viewer/svmnode.h:75:29: error: 'svmn' was not declared in this scope
   void AddChild(SVMenuNode *svmn);
                             ^~~~
src/viewer/svmnode.h:78:3: error: 'SVMenuNode' does not name a type
   SVMenuNode *parent_;
   ^~~~~~~~~~
src/viewer/svmnode.h:80:3: error: 'SVMenuNode' does not name a type
   SVMenuNode *child_;
   ^~~~~~~~~~
src/viewer/svmnode.h:82:3: error: 'SVMenuNode' does not name a type
   SVMenuNode *next_;
   ^~~~~~~~~~
src/viewer/svmnode.h:98:1: error: expected declaration before '}' token
 } // namespace tesseract
 ^
make[1]: *** [Makefile:6164: src/viewer/libtesseract_la-svmnode.lo] Error 1
make[1]: Leaving directory '/root/tesseract'
make: *** [Makefile:8198: all-recursive] Error 1
```

# Pointers
## Installing and basic use
   https://linuxhint.com/install-tesseract-ocr-linux/

