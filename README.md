# Instalación de TensorFlow 2.15 con GPU en WSL2
Instrucciones para instalar CUDA v12.2 y cuDNN 8.9 en Ubuntu 22.04 para utilizar GPU con TensorFlow 2.15 en WSL2.

## 1. Instalación de Drivers de GPU NVIDIA
Visita el sitio web de NVIDIA para descargar e instalar los controladores adecuados para tu tarjeta gráfica: [Descargas de Controladores NVIDIA](https://la.nvidia.com/Download/index.aspx?lang=la)

## 2. Instalación de WSL
Abre una terminal de Windows y ejecuta el siguiente comando para instalar Windows Subsystem for Linux (WSL):
```cmd
wsl --install
```
Sigue las instrucciones para configurar un nombre de usuario y una contraseña para tu distribución de WSL.

## 3. Actualización del Sistema y Instalación de Miniconda
Actualiza el sistema operativo Ubuntu dentro de WSL:
```cmd
sudo apt update && sudo apt upgrade -y
```
Reinicia el sistema:
```cmd
sudo reboot
```
Descarga e instala Miniconda:
```cmd
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
```cmd
bash Miniconda3-latest-Linux-x86_64.sh
```
Reinicia tu terminal para cargar las funciones de shell de Conda.

## 4. Preparación del entorno conda
Si prefieres que el entorno base de conda no se active al inicio, establece el parámetro auto_activate_base a false:
```cmd
conda config --set auto_activate_base false
```
Crea un nuevo entorno Conda con Python 3.11 e instala IPykernel:
```cmd
conda create -n entornoGPU anaconda python=3.11
```
```cmd
conda activate entornoGPU
```
```cmd
conda install ipykernel
```
```cmd
python -m ipykernel install --user --name entornoGPU --display-name "entornoGPU"
```
## 4. Instalamos CUDA 12.2
Descarga e instala CUDA 12.2:
```cmd
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
```
```cmd
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
```
```cmd
wget https://developer.download.nvidia.com/compute/cuda/12.2.0/local_installers/cuda-repo-wsl-ubuntu-12-2-local_12.2.0-1_amd64.deb
```
```cmd
sudo dpkg -i cuda-repo-wsl-ubuntu-12-2-local_12.2.0-1_amd64.deb
```
```cmd
sudo cp /var/cuda-repo-wsl-ubuntu-12-2-local/cuda-*-keyring.gpg /usr/share/keyrings/
```
```cmd
sudo apt-get update
```
```cmd
sudo apt-get -y install cuda
```
Configura las variables de entorno:
```cmd
echo 'export PATH=/usr/local/cuda-12.2/bin:$PATH' >> ~/.bashrc
```
```cmd
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-12.2/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
```
```cmd
source ~/.bashrc
```
## 4. Instalamos cuDNN 8.9.4
Primero, regístrate en el [sitio web de NVIDIA ](https://developer.nvidia.com/developer-program/signup) y descarga el paquete cuDNN 8.9.4.

https://developer.nvidia.com/downloads/compute/cudnn/secure/8.9.4/local_installers/12.x/cudnn-local-repo-ubuntu2204-8.9.4.25_1.0-1_amd64.deb/ ,

Transfiere el archivo .deb de cuDNN a tu instancia de WSL2. Puedes hacer esto a través de la carpeta compartida de WSL2, preferiblemente colocándolo en /home/usuario/.
Instala cuDNN y sus dependencias:
```cmd
sudo dpkg -i cudnn-local-repo-ubuntu2204-8.9.4.25_1.0-1_amd64.deb
```
```cmd
sudo apt update
```
```cmd
sudo apt install libcudnn8 libcudnn8-dev libcudnn8-samples
```
Reinicia el sistema:
```cmd
sudo reboot
```

## 5. Verificación de la Instalación
```cmd
nvidia-smi
```
```cmd
nvcc -V
```

## 5. Instalación de TensorFlow 2.15
Vuelve al entorno de Conda:
```cmd
conda activate entornoGPU
```
Instala TensorFlow 2.15:
```cmd
pip install tensorflow==2.15
```

## 6. Verificación de TensorFlow con GPU
Verifica la versión de TensorFlow instalada:
```cmd
python -c "import tensorflow as tf; print(tf.__version__)"
```
Comprueba si TensorFlow detecta la GPU:
```cmd
python -c "import tensorflow as tf; print('Num GPUs Available:', len(tf.config.list_physical_devices('GPU')))"
```

Verifica la versión de TensorFlow instalada y si se detecta la GPU correctamente. Si la salida del siguiente comando muestra `Num GPUs Available: 0`, significa que TensorFlow no está detectando la GPU correctamente.

Si la GPU no se detecta correctamente, asegúrate de haber seguido correctamente los pasos anteriores. Aquí hay algunos puntos que puedes revisar:

- Verifica que los controladores de la GPU estén instalados correctamente.

- Confirma que CUDA y cuDNN se hayan instalado correctamente y las variables de entorno estén configuradas adecuadamente.

- Asegúrate de estar utilizando el entorno de Conda `entornoGPU` antes de ejecutar los comandos de TensorFlow.

## 6. Uso de TensorFlow con GPU
Puedes utilizar TensorFlow con GPU en entornos como Visual Studio Code o Jupyter Notebook. Para ello:

Abre Visual Studio Code: 
```cmd
code .
```
Inicia Jupyter Notebook:
```cmd
jupyter notebook
```

Selecciona el entorno de ejecución entornoGPU dentro de Jupyter Notebook para utilizar TensorFlow con GPU.

¡Listo! Ahora estás configurado para utilizar TensorFlow con GPU en tu entorno de desarrollo.
