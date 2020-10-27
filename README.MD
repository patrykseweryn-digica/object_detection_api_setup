# Ustalenie ścieżki, gdzie znajdą się wszystkie niezbędne pliki

OBJ_API_PATH="$(pwd)"  
mkdir $OBJ_API_PATH  
cd $OBJ_API_PATH

# instalacja Pythona

sudo apt-get install python3-pip  
sudo pip3 install virtualenv
 
# tworzenie środowiska wirtualnego
virtualenv -p python3 venv  
source venv/bin/activate

# pobranie repozytorium Tensorflowa z gotowymi modelami do użycia 
mkdir TensorFlow  
cd TensorFlow  
git clone https://github.com/tensorflow/models.git  
cd ..  

# install protobuf
 
sudo apt install protobuf-compiler  
cd TensorFlow/models/research  
protoc object_detection/protos/*.proto --python_out=.  

# instalacja cythona - biblioteki istotnej do zbudowania cocoapi 
pip install cython


 
# install cocoapi
cd $OBJ_API_PATH
pip install numpy
git clone https://github.com/cocodataset/cocoapi.git  
cd cocoapi/PythonAPI  
make  
cp -r pycocotools $OBJ_API_PATH/TensorFlow/models/research/  
cd $OBJ_API_PATH  
 
# instalacja niezbędnych bibliotek
cd $OBJ_API_PATH/TensorFlow/models/research  
cp object_detection/packages/tf2/setup.py .  
python -m pip install .  
 
# uruchomienie testów, by mieć pewność, że wszystko działa
python object_detection/builders/model_builder_tf2_test.py  

# Repozytorium z wytrenowanymi modelami: 
https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md

