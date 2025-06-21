2025-06-17T10:00:18-07:00



# SC qubits

2025-06-19T21:20:01-07:00
Maybe I'll directly work in google doc.
https://github.com/qiskit-community/qiskit-metal


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




# References

https://github.com/qiskit-community/qiskit-metal

https://qiskit-community.github.io/qiskit-metal/tut/

https://www.youtube.com/watch?v=NCNv3YPvveM&list=PLOFEBzvs-VvqHl5ZqVmhB_FcSqmLufsjb&index=1&ab_channel=Qiskit
- ![[Pasted image 20250620235508.png]]


## Papers

https://arxiv.org/abs/2103.10344

