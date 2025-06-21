2025-06-17T10:00:18-07:00



# SC qubits

2025-06-19T21:20:01-07:00
Maybe I'll directly work in google doc.
https://github.com/qiskit-community/qiskit-metal


## Installing Qiskit
2025-06-20T23:39:44-07:00
Installing:
![[Pasted image 20250620233936.png]]

The fuck is this error:
- `ERROR: Cannot install qiskit-metal==0.0.4, qiskit-metal==0.1.0, qiskit-metal==0.1.1, qiskit-metal==0.1.2 and qiskit-metal==0.1.5 because these package versions have conflicting dependencies.`


IBM likes specific stuff I guess:
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
qiskit-metal 0.1.5 requires addict==2.4.0, which is not installed.
qiskit-metal 0.1.5 requires descartes==1.1.0, which is not installed.
qiskit-metal 0.1.5 requires gdspy==1.6.12, which is not installed.
qiskit-metal 0.1.5 requires geopandas==0.12.2, which is not installed.
qiskit-metal 0.1.5 requires gmsh==4.11.1, which is not installed.
qiskit-metal 0.1.5 requires ipython==8.10.0, which is not installed.
qiskit-metal 0.1.5 requires pint==0.20.1, which is not installed.
qiskit-metal 0.1.5 requires pyaedt==0.6.46, which is not installed.
qiskit-metal 0.1.5 requires pyEPR-quantum==0.8.5.7, which is not installed.
qiskit-metal 0.1.5 requires pygments==2.14.0, which is not installed.
qiskit-metal 0.1.5 requires pyside2==5.15.2.1, which is not installed.
qiskit-metal 0.1.5 requires pyyaml==6.0, which is not installed.
qiskit-metal 0.1.5 requires qdarkstyle==3.1, which is not installed.
qiskit-metal 0.1.5 requires qutip==4.7.1, which is not installed.
qiskit-metal 0.1.5 requires scipy==1.10.0, which is not installed.
qiskit-metal 0.1.5 requires scqubits==3.1.0, which is not installed.
qiskit-metal 0.1.5 requires matplotlib==3.7.0, but you have matplotlib 3.9.4 which is incompatible.
qiskit-metal 0.1.5 requires numpy==1.24.2, but you have numpy 2.0.2 which is incompatible.
qiskit-metal 0.1.5 requires pandas==1.5.3, but you have pandas 2.3.0 which is incompatible.
qiskit-metal 0.1.5 requires shapely==2.0.1, but you have shapely 2.0.7 which is incompatible.
```

- `source venv/bin/activate && pip install numpy==1.24.2 pandas==1.5.3 matplotlib==3.7.0 shapely==2.0.1`
- `source venv/bin/activate && pip install scipy==1.10.0 addict==2.4.0 pint==0.20.1 pyyaml==6.0 pygments==2.14.0 ipython==8.10.0`
- `source venv/bin/activate && pip install scqubits==3.1.0`
- `source venv/bin/activate && pip install qutip==4.7.1 --force-reinstall`

2025-06-20T23:57:43-07:00
Still struggling to install pyside2. Asking it to get from source:
- https://download.qt.io/official_releases/QtForPython/pyside2/PySide2-5.15.8-src/
- `brew install cmake qt@5`
- qmake not found

2025-06-21T00:04:27-07:00
look at this fucking installation cmd:
```shell
cd /tmp/pyside-setup-opensource-src-5.15.8 && export PATH="/opt/homebrew/opt/qt@5/bin:$PATH" && export LDFLAGS="-L/opt/homebrew/opt/qt@5/lib" && export CPPFLAGS="-I/opt/homebrew/opt/qt@5/include" && export PKG_CONFIG_PATH="/opt/homebrew/opt/qt@5/lib/pkgconfig" && source /Users/wentaojiang/Documents/GitHub/PlayGround/20250619_SCQubits/venv/bin/activate && python setup.py build --qmake=/opt/homebrew/opt/qt@5/bin/qmake
```

Need cmake < 3.5
- have to install from source `wget https://github.com/Kitware/CMake/releases/download/v3.19.8/cmake-3.19.8-macos-universal.tar.gz`

2025-06-21T00:08:49-07:00
We are going deeper
- `brew install llvm`
- actually
- `brew install llvm@12`

2025-06-21T00:10:57-07:00
We are finally building:
```bash
export PATH="/opt/homebrew/opt/qt@5/bin:/opt/homebrew/opt/llvm@12/bin:$PATH" && export LDFLAGS="-L/opt/homebrew/opt/qt@5/lib -L/opt/homebrew/opt/llvm@12/lib" && export CPPFLAGS="-I/opt/homebrew/opt/qt@5/include -I/opt/homebrew/opt/llvm@12/include" && export PKG_CONFIG_PATH="/opt/homebrew/opt/qt@5/lib/pkgconfig" && export LLVM_INSTALL_DIR="/opt/homebrew/opt/llvm@12" && source /Users/wentaojiang/Documents/GitHub/PlayGround/20250619_SCQubits/venv/bin/activate && python setup.py build --qmake=/opt/homebrew/opt/qt@5/bin/qmake
```
- error about numpy version too new lol god damn it
- `source /Users/wentaojiang/Documents/GitHub/PlayGround/20250619_SCQubits/venv/bin/activate && pip install numpy==1.20.3 --force-reinstall`

```bash
      CCompilerOpt._cache_write[796] : write cache to path -> /private/var/folders/kz/b5tghg8n23b2p362dn3b0knw0000gn/T/pip-install-zunb0wgw/numpy_4ca2e88f3cb0492895cd83ea1a971906/build/temp.macosx-10.9-universal2-3.9/ccompiler_opt_cache_clib.py
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for numpy
Failed to build numpy
ERROR: Failed to build installable wheels for some pyproject.toml based projects (numpy)

```

```bash
source /Users/wentaojiang/Documents/GitHub/PlayGround/20250619_SCQubits/venv/bin/activate && export MACOSX_DEPLOYMENT_TARGET=11.0 && pip install numpy==1.20.3 --force-reinstall
```

2025-06-21T00:19:55-07:00
old numpy installed using old Cython:
```bash
source /Users/wentaojiang/Documents/GitHub/PlayGround/20250619_SCQubits/venv/bin/activate && export NPY_DISABLE_SVML=1 && export CFLAGS="-Wno-error" && export NPY_BLAS_ORDER="" && export NPY_LAPACK_ORDER="" && pip install numpy==1.19.5 --no-use-pep517
```

Building PySide2 again:
```bash
export PATH="/opt/homebrew/opt/qt@5/bin:/opt/homebrew/opt/llvm@12/bin:$PATH" && export LDFLAGS="-L/opt/homebrew/opt/qt@5/lib -L/opt/homebrew/opt/llvm@12/lib" && export CPPFLAGS="-I/opt/homebrew/opt/qt@5/include -I/opt/homebrew/opt/llvm@12/include" && export PKG_CONFIG_PATH="/opt/homebrew/opt/qt@5/lib/pkgconfig" && export LLVM_INSTALL_DIR="/opt/homebrew/opt/llvm@12" && source /Users/wentaojiang/Documents/GitHub/PlayGround/20250619_SCQubits/venv/bin/activate && python setup.py build --qmake=/opt/homebrew/opt/qt@5/bin/qmake
```

Too many errors emitted
```
/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/v1/__config:901:81: note: expanded from macro '_LIBCPP_BEGIN_NAMESPACE_STD'

  :0:0: fatal: too many errors emitted, stopping now

Keeping temporary file: /private/var/folders/kz/b5tghg8n23b2p362dn3b0knw0000gn/T/QtCore_global_NHglYB.hpp
shiboken: Error running ApiExtractor.
```


## Docker container & tunnel the GUI to macos
2025-06-21T00:27:44-07:00

Going to try this. The above installation was too brutal.

2025-06-21T00:40:45-07:00
Damn it built:
```shell

(venv) Mac:20250619_SCQubits wentaojiang$ docker build -t qiskit-metal-container .

[+] Building 295.8s (17/17) FINISHED                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 2.64kB                                     0.0s
 => [internal] load metadata for docker.io/library/ubuntu:20.04            5.0s
 => [internal] load .dockerignore                                          0.1s
 => => transferring context: 2B                                            0.1s
 => [ 1/12] FROM docker.io/library/ubuntu:20.04@sha256:8feb4d8ca5354def3d  1.9s
 => => resolve docker.io/library/ubuntu:20.04@sha256:8feb4d8ca5354def3d8f  0.0s
 => => sha256:ecd83b6c354452b6a9979c7666bba16927f1e60e2 25.98MB / 25.98MB  1.4s
 => => extracting sha256:ecd83b6c354452b6a9979c7666bba16927f1e60e2afbfe64  0.5s
 => [internal] load build context                                          0.0s
 => => transferring context: 2.23kB                                        0.0s
 => [ 2/12] RUN apt-get update && apt-get install -y     build-essentia  224.1s
 => [ 3/12] RUN update-alternatives --install /usr/bin/python3 python3 /u  0.4s 
 => [ 4/12] RUN update-alternatives --install /usr/bin/python python /usr  0.1s 
 => [ 5/12] RUN curl https://bootstrap.pypa.io/get-pip.py | python3.9      3.0s 
 => [ 6/12] WORKDIR /workspace                                             0.0s 
 => [ 7/12] RUN python3.9 -m venv /workspace/venv                          0.8s 
 => [ 8/12] RUN /workspace/venv/bin/pip install --upgrade pip setuptools   3.0s 
 => [ 9/12] RUN /workspace/venv/bin/pip install     "numpy==1.19.5"     "  5.0s 
 => [10/12] COPY requirements-docker.txt /workspace/                       0.0s 
 => [11/12] COPY install-qiskit-metal.sh /workspace/                       0.0s 
 => [12/12] RUN chmod +x /workspace/install-qiskit-metal.sh                0.1s 
 => exporting to image                                                    52.1s 
 => => exporting layers                                                   40.2s 
 => => exporting manifest sha256:d7369072d0081517f9d85870fd1cf94a7ee70d82  0.0s 
 => => exporting config sha256:dd8345d2de5342c3763516eecac324df3cce97f54e  0.0s
 => => exporting attestation manifest sha256:d3673bd5de8f5c0d5a390f721b80  0.0s
 => => exporting manifest list sha256:b5f9cef95da076bdd650ff9feca23541be1  0.0s
 => => naming to docker.io/library/qiskit-metal-container:latest           0.0s
 => => unpacking to docker.io/library/qiskit-metal-container:latest       11.8s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/t60s3rc1e5fuaeu9mqgwsmwqh
(venv) Mac:20250619_SCQubits wentaojiang$ 
```

2025-06-21T00:43:17-07:00
Running install in docker:
```bash

root@8eb374669111:/workspace# ls -la
total 20
drwxr-xr-x  1 root root 4096 Jun 21 07:42 .
drwxr-xr-x  1 root root 4096 Jun 21 07:42 ..
drwxr-xr-x 13 root root  416 Jun 21 07:25 host
-rwxr-xr-x  1 root root 1651 Jun 21 07:25 install-qiskit-metal.sh
-rw-r--r--  1 root root  402 Jun 21 07:25 requirements-docker.txt
drwxr-xr-x  1 root root 4096 Jun 21 07:38 venv
root@8eb374669111:/workspace# ./install-qiskit-metal.sh
```


2025-06-21T01:11:44-07:00
Switching to x86_64. God damn it.
```bash
# Rebuild the container for x86_64 platform
docker build --platform linux/x86_64 -t qiskit-metal-container .
```


2025-06-21T01:18:46-07:00
Built and running
```bash
docker run --platform linux/x86_64 -it --rm -e DISPLAY=host.docker.internal:0 -v /tmp/.X11-unix:/tmp/.X11-unix -v $(pwd):/workspace/host qiskit-metal-container bash
```

![[Pasted image 20250621011932.png]]


```bash
# Activate the virtual environment
source venv/bin/activate

# Check the platform to confirm we're on x86_64
python -c "import platform; print('Platform:', platform.platform()); print('Machine:', platform.machine())"

# Install PySide2 from the wheel
pip install https://files.pythonhosted.org/packages/c2/9a/78ca8bada6cf4d2798e0c823c025c590517d74445837f4eb50bfddce8737/PySide2-5.15.2.1-5.15.2-cp35.cp36.cp37.cp38.cp39.cp310-abi3-manylinux1_x86_64.whl

# Install the rest of the requirements
pip install -r /workspace/requirements-docker.txt

# Install qiskit-metal
pip install qiskit-metal

# Test the installation
python -c "
import qiskit_metal
import scqubits
import PySide2
print('✅ All packages imported successfully!')
print(f'qiskit-metal version: {qiskit_metal.__version__}')
print(f'scqubits version: {scqubits.__version__}')
print(f'PySide2 version: {PySide2.__version__}')
"
```

2025-06-21T01:19:58-07:00
God damn it wow:
```
✅ All packages imported successfully!
qiskit-metal version: 0.1.5
scqubits version: 3.1.0
PySide2 version: 5.15.2.1
```

2025-06-21T01:28:11-07:00
holy shit
![[Pasted image 20250621012816.png]]

2025-06-21T01:35:00-07:00
Trying to build the container with qiskit installed.

2025-06-21T01:44:53-07:00

![[Pasted image 20250621014455.png]]

2025-06-21T01:57:33-07:00
Ok time to sleep
![[Pasted image 20250621015737.png]]
- https://qiskit-community.github.io/qiskit-metal/circuit-examples/F.Small-quantum-chips/51-Four_qubit_chip.html



# References

https://github.com/qiskit-community/qiskit-metal

https://qiskit-community.github.io/qiskit-metal/tut/

https://www.youtube.com/watch?v=NCNv3YPvveM&list=PLOFEBzvs-VvqHl5ZqVmhB_FcSqmLufsjb&index=1&ab_channel=Qiskit
- ![[Pasted image 20250620235508.png]]


## Papers

https://arxiv.org/abs/2103.10344

