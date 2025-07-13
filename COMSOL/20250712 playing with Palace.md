2025-07-12T11:34:48-07:00

```
~/GitHub/palace$ source ~/spack/share/spack/setup-env.sh && spack install palace +magma +libxsmm +superlu-dist +slepc +openmp +gslib ^magma cuda_arch=90
[+] /usr (external glibc-2.35-k3ew4cxm5njpjlxrtczjmuw7mnayyvei)
[+] /usr (external gcc-11.4.0-ho6xntcwvsuhewn4kf5g5un5hfofrt7w)
==> No binary for compiler-wrapper-1.0-vrtwitoifuvcwm7r47x3hxhjqin2hov5 found: installing from source
==> Installing compiler-wrapper-1.0-vrtwitoifuvcwm7r47x3hxhjqin2hov5 [3/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/c6/c65a9d2b2d4eef67ab5cb0684d706bb9f005bb2be94f53d82683d7055bdb837c
    [100%]   30.23 KB @   40.1 MB/s
==> No patches needed for compiler-wrapper
==> compiler-wrapper: Executing phase: 'install'
==> compiler-wrapper: Successfully installed compiler-wrapper-1.0-vrtwitoifuvcwm7r47x3hxhjqin2hov5
  Stage: 0.17s.  Install: 0.00s.  Post-install: 0.01s.  Total: 0.23s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/compiler-wrapper-1.0-vrtwitoifuvcwm7r47x3hxhjqin2hov5
==> No binary for ca-certificates-mozilla-2025-02-25-cgyslpaz7bv53h6hbptcpb25xnkqr7xw found: installing from source
==> Installing ca-certificates-mozilla-2025-02-25-cgyslpaz7bv53h6hbptcpb25xnkqr7xw [4/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/50/50a6277ec69113f00c5fd45f09e8b97a4b3e32daa35d3a95ab30137a55386cef
    [100%]  233.26 KB @   12.1 MB/s
==> No patches needed for ca-certificates-mozilla
==> ca-certificates-mozilla: Executing phase: 'install'
==> ca-certificates-mozilla: Successfully installed ca-certificates-mozilla-2025-02-25-cgyslpaz7bv53h6hbptcpb25xnkqr7xw
  Stage: 0.19s.  Install: 0.00s.  Post-install: 0.00s.  Total: 0.23s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/ca-certificates-mozilla-2025-02-25-cgyslpaz7bv53h6hbptcpb25xnkqr7xw
==> No binary for gcc-runtime-11.4.0-qgwasudzyqhoah4luei64jb3fhbg4mzq found: installing from source
==> Installing gcc-runtime-11.4.0-qgwasudzyqhoah4luei64jb3fhbg4mzq [5/67]
==> No patches needed for gcc-runtime
==> gcc-runtime: Executing phase: 'install'
==> gcc-runtime: Successfully installed gcc-runtime-11.4.0-qgwasudzyqhoah4luei64jb3fhbg4mzq
  Stage: 0.00s.  Install: 0.08s.  Post-install: 0.03s.  Total: 0.15s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/gcc-runtime-11.4.0-qgwasudzyqhoah4luei64jb3fhbg4mzq
==> No binary for gmake-4.4.1-74t4nqlqhen6azcxigk6xx5574crbzxg found: installing from source
==> Installing gmake-4.4.1-74t4nqlqhen6azcxigk6xx5574crbzxg [6/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/dd/dd16fb1d67bfab79a72f5e8390735c49e3e8e70b4945a15ab1f81ddb78658fb3.tar.gz
    [100%]    2.35 MB @   57.4 MB/s
==> No patches needed for gmake
==> gmake: Executing phase: 'install'
==> gmake: Successfully installed gmake-4.4.1-74t4nqlqhen6azcxigk6xx5574crbzxg
  Stage: 0.23s.  Install: 9.00s.  Post-install: 0.00s.  Total: 9.29s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/gmake-4.4.1-74t4nqlqhen6azcxigk6xx5574crbzxg
==> No binary for pkgconf-2.3.0-mktfvgyb66tsmf32mt63yzyho7oelyeh found: installing from source
==> Installing pkgconf-2.3.0-mktfvgyb66tsmf32mt63yzyho7oelyeh [7/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/3a/3a9080ac51d03615e7c1910a0a2a8df08424892b5f13b0628a204d3fcce0ea8b.tar.xz
    [100%]  316.16 KB @   14.5 MB/s
==> No patches needed for pkgconf
==> pkgconf: Executing phase: 'autoreconf'
==> pkgconf: Executing phase: 'configure'
==> pkgconf: Executing phase: 'build'
==> pkgconf: Executing phase: 'install'
==> pkgconf: Successfully installed pkgconf-2.3.0-mktfvgyb66tsmf32mt63yzyho7oelyeh
  Stage: 0.20s.  Autoreconf: 0.00s.  Configure: 1.99s.  Build: 0.73s.  Install: 0.15s.  Post-install: 0.01s.  Total: 3.26s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/pkgconf-2.3.0-mktfvgyb66tsmf32mt63yzyho7oelyeh
==> No binary for libiconv-1.18-qcaeztrhheldqpqu2ja7stibggrd634e found: installing from source
==> Installing libiconv-1.18-qcaeztrhheldqpqu2ja7stibggrd634e [8/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/3b/3b08f5f4f9b4eb82f151a7040bfd6fe6c6fb922efe4b1659c66ea933276965e8.tar.gz
    [100%]    5.82 MB @  113.9 MB/s
==> No patches needed for libiconv
==> libiconv: Executing phase: 'autoreconf'
==> libiconv: Executing phase: 'configure'
==> libiconv: Executing phase: 'build'
==> libiconv: Executing phase: 'install'
==> libiconv: Successfully installed libiconv-1.18-qcaeztrhheldqpqu2ja7stibggrd634e
  Stage: 0.32s.  Autoreconf: 0.00s.  Configure: 9.91s.  Build: 6.79s.  Install: 0.52s.  Post-install: 0.03s.  Total: 17.75s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/libiconv-1.18-qcaeztrhheldqpqu2ja7stibggrd634e
==> No binary for libffi-3.4.8-dsketbq42egxdh3pnqyhbibcb6tgyqi6 found: installing from source
==> Installing libffi-3.4.8-dsketbq42egxdh3pnqyhbibcb6tgyqi6 [9/67]
==> Fetching https://github.com/libffi/libffi/releases/download/v3.4.8/libffi-3.4.8.tar.gz
    [100%]    1.40 MB @   37.3 MB/s
==> No patches needed for libffi
==> libffi: Executing phase: 'autoreconf'
==> libffi: Executing phase: 'configure'
==> libffi: Executing phase: 'build'
==> libffi: Executing phase: 'install'
==> libffi: Successfully installed libffi-3.4.8-dsketbq42egxdh3pnqyhbibcb6tgyqi6
  Stage: 0.62s.  Autoreconf: 0.00s.  Configure: 3.35s.  Build: 0.84s.  Install: 0.17s.  Post-install: 0.01s.  Total: 5.19s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/libffi-3.4.8-dsketbq42egxdh3pnqyhbibcb6tgyqi6
==> No binary for zlib-ng-2.2.4-umq2hsqsdrtbmb343whnlmsedftzwibj found: installing from source
==> Installing zlib-ng-2.2.4-umq2hsqsdrtbmb343whnlmsedftzwibj [10/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/a7/a73343c3093e5cdc50d9377997c3815b878fd110bf6511c2c7759f2afb90f5a3.tar.gz
    [100%]    2.42 MB @   57.3 MB/s
==> No patches needed for zlib-ng
==> zlib-ng: Executing phase: 'autoreconf'
==> zlib-ng: Executing phase: 'configure'
==> zlib-ng: Executing phase: 'build'
==> zlib-ng: Executing phase: 'install'
==> zlib-ng: Successfully installed zlib-ng-2.2.4-umq2hsqsdrtbmb343whnlmsedftzwibj
  Stage: 0.41s.  Autoreconf: 0.00s.  Configure: 2.14s.  Build: 0.72s.  Install: 0.22s.  Post-install: 0.01s.  Total: 3.69s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/zlib-ng-2.2.4-umq2hsqsdrtbmb343whnlmsedftzwibj
==> No binary for libmd-1.1.0-j6rtu2gqq3fezvbhyamrqkgylgff7f7f found: installing from source
==> Installing libmd-1.1.0-j6rtu2gqq3fezvbhyamrqkgylgff7f7f [11/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/1b/1bd6aa42275313af3141c7cf2e5b964e8b1fd488025caf2f971f43b00776b332.tar.xz
    [100%]  271.23 KB @   13.6 MB/s
==> No patches needed for libmd
==> libmd: Executing phase: 'autoreconf'
==> libmd: Executing phase: 'configure'
==> libmd: Executing phase: 'build'
==> libmd: Executing phase: 'install'
==> libmd: Successfully installed libmd-1.1.0-j6rtu2gqq3fezvbhyamrqkgylgff7f7f
  Stage: 0.13s.  Autoreconf: 0.00s.  Configure: 2.56s.  Build: 0.64s.  Install: 0.14s.  Post-install: 0.01s.  Total: 3.66s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/libmd-1.1.0-j6rtu2gqq3fezvbhyamrqkgylgff7f7f
==> No binary for berkeley-db-18.1.40-q6drzzgsfdle7frtpwzhzn37ezo4vfx7 found: installing from source
==> Installing berkeley-db-18.1.40-q6drzzgsfdle7frtpwzhzn37ezo4vfx7 [12/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/0c/0cecb2ef0c67b166de93732769abdeba0555086d51de1090df325e18ee8da9c8.tar.gz
    [100%]   30.76 MB @  166.5 MB/s
==> Applied patch /home/ubuntu/.spack/package_repos/fncqgg4/repos/spack_repo/builtin/packages/berkeley_db/drop-docs.patch
==> Applied patch /home/ubuntu/.spack/package_repos/fncqgg4/repos/spack_repo/builtin/packages/berkeley_db/tls.patch
==> Ran patch() for berkeley-db
==> berkeley-db: Executing phase: 'autoreconf'
==> berkeley-db: Executing phase: 'configure'
==> berkeley-db: Executing phase: 'build'
==> berkeley-db: Executing phase: 'install'
==> berkeley-db: Successfully installed berkeley-db-18.1.40-q6drzzgsfdle7frtpwzhzn37ezo4vfx7
  Stage: 0.88s.  Autoreconf: 0.00s.  Configure: 9.00s.  Build: 4.43s.  Install: 0.30s.  Post-install: 0.02s.  Total: 14.84s
[+] /home/ubuntu/spack/opt/spack/linux-icelake/berkeley-db-18.1.40-q6drzzgsfdle7frtpwzhzn37ezo4vfx7
==> No binary for xz-5.6.3-4odu6xmsgqnnguiee7lkkm36sbhvp7o2 found: installing from source
==> Installing xz-5.6.3-4odu6xmsgqnnguiee7lkkm36sbhvp7o2 [13/67]
==> Fetching https://mirror.spack.io/_source-cache/archive/a9/a95a49147b2dbb5487517acc0adcd77f9c2032cf00664eeae352405357d14a6c.tar.bz2
    [100%]    1.73 MB @   43.5 MB/s
==> No patches needed for xz
==> xz: Executing phase: 'autoreconf'
==> xz: Executing phase: 'configure'
==> xz: Executing phase: 'build'
==> xz: Executing phase: 
```


2025-07-12T22:52:10-07:00
Trying out some examples, get config error:
```
~/GitHub/palace/examples/spheres$ cd /home/ubuntu/GitHub/palace/examples/cpw && palace cpw_wave_uniform.json
>> /home/ubuntu/spack/opt/spack/linux-icelake/openmpi-5.0.8-eck3x6xr3gz374vqoehf7dkjv4esq3k2/bin/mpirun -n 1 /home/ubuntu/spack/opt/spack/linux-icelake/palace-0.12.0-2l2r2vugxfbfossyzwenc7sfpmksqnog/bin/palace-x86_64.bin cpw_wave_uniform.json

_____________     _______
_____   __   \____ __   /____ ____________
____   /_/  /  __ ` /  /  __ ` /  ___/  _ \
___   _____/  /_/  /  /  /_/  /  /__/  ___/
  /__/     \___,__/__/\___,__/\_____\_____/



Verification failed: (it->find("X") != it->end() && it->find("Y") != it->end() && it->find("Z") != it->end()) is false:
 --> Missing "Probe" point "X", "Y", or "Z" in configuration file!
 ... in function: void palace::config::ProbePostData::SetUp(palace::config::json&)
 ... in file: /tmp/ubuntu/spack-stage/spack-stage-palace-0.12.0-2l2r2vugxfbfossyzwenc7sfpmksqnog/spack-src/palace/utils/configfile.cpp:575

--------------------------------------------------------------------------
MPI_ABORT was invoked on rank 0 in communicator MPI_COMM_WORLD
  Proc: [[45441,1],0]
  Errorcode: 1

NOTE: invoking MPI_ABORT causes Open MPI to kill all MPI processes.
You may or may not see output from other processes, depending on
exactly when Open MPI kills them.
--------------------------------------------------------------------------
--------------------------------------------------------------------------
prterun has exited due to process rank 0 with PID 0 on node 192-222-54-152 calling
"abort". This may have caused other processes in the application to be
terminated by signals sent by prterun (as reported here).
--------------------------------------------------------------------------
```


2025-07-12T23:55:41-07:00
The coaxial example ran. Got the files locally.
Setting up paraview
- `brew install --cask paraview`
![[Pasted image 20250713002931.png]]
Could see some shit but a lot of other examples are not running properly.

2025-07-13T09:01:08-07:00
Ran all examples before sleep yesterday:
```
cd /home/ubuntu/GitHub/palace && chmod +x run_all_examples.sh && ./run_all_examples.sh
Palace Examples Run - Sun Jul 13 07:31:55 UTC 2025
Palace Version: Usage: palace [OPTIONS] CONFIG_FILE
========================================

Running 15 Palace examples...

[1/15] Running: spheres/spheres.json
----------------------------------------
ubuntu@192-222-54-152:~/GitHub/palace$ ^C
ubuntu@192-222-54-152:~/GitHub/palace$ 
ubuntu@192-222-54-152:~/GitHub/palace$ 
ubuntu@192-222-54-152:~/GitHub/palace$ 
ubuntu@192-222-54-152:~/GitHub/palace$ ./run_all_examples.sh
Palace Examples Run - Sun Jul 13 07:36:29 UTC 2025
Palace Version: Usage: palace [OPTIONS] CONFIG_FILE
========================================

Running 15 Palace examples...

[1/15] Running: spheres/spheres.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 1s)

[2/15] Running: rings/rings.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 0s)

[3/15] Running: cylinder/cavity_pec.json
----------------------------------------
Status: SUCCESS (Exit Code: 0, Runtime: 392s)

[4/15] Running: cylinder/cavity_impedance.json
----------------------------------------
Status: SUCCESS (Exit Code: 0, Runtime: 223s)

[5/15] Running: cylinder/waveguide.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 0s)

[6/15] Running: cylinder/floquet.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 0s)

[7/15] Running: coaxial/coaxial_open.json
----------------------------------------
Status: SUCCESS (Exit Code: 0, Runtime: 74s)

[8/15] Running: coaxial/coaxial_matched.json
----------------------------------------
Status: SUCCESS (Exit Code: 0, Runtime: 79s)

[9/15] Running: coaxial/coaxial_short.json
----------------------------------------
Status: SUCCESS (Exit Code: 0, Runtime: 75s)

[10/15] Running: cpw/cpw_lumped_uniform.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 1s)

[11/15] Running: cpw/cpw_wave_uniform.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 0s)

[12/15] Running: cpw/cpw_lumped_adaptive.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 0s)

[13/15] Running: cpw/cpw_wave_adaptive.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 1s)

[14/15] Running: cpw/cpw_coax_uniform.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 0s)

[15/15] Running: cpw/cpw_coax_adaptive.json
----------------------------------------
Status: FAILED (Exit Code: 1, Runtime: 1s)

========================================
FINAL SUMMARY
========================================
Total Examples: 15
Successful: 5
Failed: 10
Success Rate: 33%
Log file saved to: /home/ubuntu/GitHub/palace/all_examples_log.txt
Results summary will be saved to: /home/ubuntu/GitHub/palace/example_results_summary.md
Summary generated: /home/ubuntu/GitHub/palace/example_results_summary.md
```

