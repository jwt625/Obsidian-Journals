
Reference:
- https://github.com/srush/GPU-Puzzles/tree/main


2025-05-11T22:08:34-07:00
Ran into error at `problem.check()`
Found out I likely need to change the hardware accelerator type:
![[Pasted image 20250511220844.png]]

2025-05-11T22:11:29-07:00
ok now the error is different:
```
  
LinkerError: [222] Call to cuLinkAddData results in CUDA_ERROR_UNSUPPORTED_PTX_VERSION ptxas application ptx input, line 9; fatal : Unsupported .version 8.5; current version is '8.4'
```

Debugging:
![[Pasted image 20250511221200.png]]
2025-05-11T22:21:05-07:00
Going to try the TPU lol.
ok same error of no CUPDA:
```
  
CudaSupportError: Error at driver init: 

CUDA driver library cannot be found.
If you are sure that a CUDA driver is installed,
try setting environment variable NUMBA_CUDA_DRIVER
with the file path of the CUDA driver shared library.
:

---------------------------------------------------------------------------
NOTE: If your import is failing due to a missing package, you can
manually install dependencies using either !pip or !apt.

To view examples of installing some common dependencies, click the
"Open Examples" button below.
---------------------------------------------------------------------------
```

GPT4o suggested `pip install numba==0.57`, and `pip install llvmlite==0.40`.

```
from numba import cuda
print(cuda.detect())
```

```
Found 1 CUDA devices id 0 b'Tesla T4' [SUPPORTED] Compute Capability: 7.5 PCI Device ID: 4 PCI Bus ID: 0 UUID: GPU-7123bc20-e754-70dd-e114-d6d55b9fcde2 Watchdog: Disabled FP32/FP64 Performance Ratio: 32 Summary: 1/1 devices are supported True
```

2025-05-11T22:30:23-07:00
Chat also suggested adding the following before numba import
```
import os
os.environ['NUMBA_CUDA_PTX_VERSION'] = '80'
```

Now `from lib import CudaProblem, Coord` is not working, seems like it cannot find the lib folder... weird!
```
      3 import warnings
----> 4 from lib import CudaProblem, Coord

ImportError: cannot import name 'CudaProblem' from 'lib' (unknown location)

---------------------------------------------------------------------------
NOTE: If your import is failing due to a missing package, you can
manually install dependencies using either !pip or !apt.

To view examples of installing some common dependencies, click the
"Open Examples" button below.
---------------------------------------------------------------------------
```

It is from the wget above, why is it not found??

What is this shit:
![[Pasted image 20250511223411.png]]

`from lib import *` works...

WTF:
![[Pasted image 20250511223629.png]]

hmm is it because of the `os.environ['NUMBA_CUDA_PTX_VERSION'] = '80'`, the CudaProblem is after `@dataclass`. What the hell does this do?
2025-05-11T22:39:33-07:00
ok it is just something to simplify a class definition:
![[Pasted image 20250511223943.png]]

2025-05-12T00:24:12-07:00
Now it runs fine. Jesus fucking christ.
However, some error at `problem.check()`. I gave up for now:
![[Pasted image 20250512002507.png]]





