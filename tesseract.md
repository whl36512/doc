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
yum install tesseract-script-latn.noarch
conda install -c conda-forge pytesseract
```

# Use
```
tesseract capture.png output -l eng+fra
```
##### Output to searchable PDF
```
tesseract capture.png output -l eng pdf
```
##### Extracting Text from a Multi-page PDF File
```
pdftoppm -png file.pdf output
```
##### Combine PDF files
```
pdfunite *.pdf joined.pdf
```
##### Bounding box

  https://stackoverflow.com/questions/20831612/getting-the-bounding-box-of-the-recognized-words-using-python-tesseract
```
import pytesseract
from pytesseract import Output
import cv2
img = cv2.imread('image.jpg')

d = pytesseract.image_to_data(img, output_type=Output.DICT)
#ocr_result = pytesseract.image_to_string(Image.open('image.tif'), lang='eng')

n_boxes = len(d['level'])
for i in range(n_boxes):
    (x, y, w, h) = (d['left'][i], d['top'][i], d['width'][i], d['height'][i])
    cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)

cv2.imshow('img', img)
cv2.waitKey(0)
```
##### Alphanumeric codes ( still not satisfactory)
https://groups.google.com/g/tesseract-ocr/c/Hr79AmtApeA

```
tesseract = new Tesseract();                      
tesseract.setOcrEngineMode(TessAPI.TessOcrEngineMode.OEM_TESSERACT_ONLY);
tesseract.setPageSegMode(7);
tesseract.setTessVariable("load_system_dawg", "0");
tesseract.setTessVariable("load_freq_dawg", "0");
tesseract.setTessVariable("load_punc_dawg", "0");
tesseract.setTessVariable("load_number_dawg", "0");
tesseract.setTessVariable("load_unambig_dawg", "0");
tesseract.setTessVariable("load_bigram_dawg", "0");
tesseract.setTessVariable("load_fixed_length_dawgs", "0");

tesseract.setTessVariable("classify_enable_learning", "0");
tesseract.setTessVariable("classify_enable_adaptive_matcher", "0");

tesseract.setTessVariable("segment_penalty_garbage", "0");
tesseract.setTessVariable("segment_penalty_dict_nonword", "0");
tesseract.setTessVariable("segment_penalty_dict_frequent_word", "0");
tesseract.setTessVariable("segment_penalty_dict_case_ok", "0");
tesseract.setTessVariable("segment_penalty_dict_case_bad", "0");
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
   
   https://linuxhint.com/install_tesseract_ocr_linux/
   
   https://developer.apple.com/documentation/vision/recognizing_text_in_images
   
   https://stackoverflow.com/questions/37745519/use-pytesseract-ocr-to-recognize-text-from-an-image. ( 2nd solution to recognize codes with --psm option?)
   
   https://pyimagesearch.com/2021/11/15/tesseract-page-segmentation-modes-psms-explained-how-to-improve-your-ocr-accuracy/
   
   Boxes around charactors
   
   https://nanonets.com/blog/ocr-with-tesseract/
   
   Set installed tesseract path for pytesseract and use --psm 6
   
   https://www.analyticsvidhya.com/blog/2021/12/optical-character-recognition-using-pytesseract/
   
   pytesseract.pytesseract.tesseract_cmd = r'C:Program FilesTesseract-OCRtesseract.exe'
   
   https://stackoverflow.com/questions/64286102/how-to-set-pytesseract-to-solve-captcha-alphanumeric-and-5-length
   
   Solves basic alpha numeric captchas using pytesseract and PIL

   https://gist.github.com/gauravssnl/f0425100713fd424592f16e61dafd74b
   
   Levinshtein distance, generating test images
   
   https://francescopochetti.com/easyocr-vs-tesseract-vs-amazon-textract-an-ocr-engine-comparison/
   
   https://betterprogramming.pub/beginners-guide-to-tesseract-ocr-using-python-10ecbb426c3d
   
   https://tesseract-ocr.github.io/tessdoc/FAQ.html
   
   
# Ideas

  Use italian language for english text
  
  Use -l script\Latin 
  
  -lsm 6
  
  I have combined LSTM and legacy together. Using combine_tessdata command.
  
  --oem 0 # engine (or 1 or 2 or 3).   
  
  tesseract multiLanguageText.png output hocr.  # hOCR output file
  
  tesseract --print-parameters
  
  From version 8.4.0 on, qpdf has the options --overlay/--underlay for easy merging of image-only and text PDFs. E.g.,

```
$ qpdf image.pdf --underlay text.pdf -- image_txt.pdf
```  
  
> You can call hocrtransform from the command line and give it the image you wish to use. ocrmypdf would normally use the filename 000001.image as the image.
>```
> python3 -m ocrmypdf.hocrtransform --image 000001.image 000001.hocr manual_000001.pdf
> ```
>  You can use qpdf to combine the single page PDFs into a PDF.
> ```
> qpdf original.pdf --pages manual_*.pdf -- outputfile.pdf
> ```
> original.pdf should be the original PDF, IIRC, from which it will gather overall document information. If you need outputfile to be a PDF/A you could write a script to invoke ocrmypdf.generate_pdfa for you.


   


