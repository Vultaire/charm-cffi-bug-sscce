How to reproduce issue:

* Install version 2.8.2 of the "charm" snap
* Run: charm build --debug charm-test

Issue encountered: there are issues installing the prerequisites needed
to build the cryptography package, and this results in "charm build"
failing.

This appears to most likely be a GCC build argument issue.  There is an
-I/snap/charm/609/usr/include/python3.6m argument, however I think
there probably should also be a -I/snap/charm/609/usr/include argument,
as the missing file does exist at
/snap/charm/609/usr/include/x86_64-linux-gnu/python3.6m/pyconfig.h.

Here is an excerpt of the build output, which includes the gcc error:

----------------------------------------------------------------------

Collecting cryptography<3.3,>=3.2
  Using cached cryptography-3.2.1.tar.gz (540 kB)
  Installing build dependencies: started
  Installing build dependencies: finished with status 'error'
  ERROR: Command errored out with exit status 1:
   command: /tmp/tmpqba_1gu6/bin/python /tmp/tmpqba_1gu6/lib/python3.6/site-packages/pip install --ignore-installed --no-user --prefix /tmp/pip-build-env-lie7q108/overlay --no-warn-script-location --no-binary :all: --only-binary :none: -i https://pypi.org/simple -- 'setuptools>=40.6.0' wheel 'cffi>=1.8,!=1.11.3; platform_python_implementation != '"'"'PyPy'"'"''
       cwd: None
  Complete output (76 lines):
  Collecting setuptools>=40.6.0
    Using cached setuptools-57.4.0.tar.gz (2.1 MB)
    Installing build dependencies: started
    Installing build dependencies: finished with status 'done'
    Getting requirements to build wheel: started
    Getting requirements to build wheel: finished with status 'done'
      Preparing wheel metadata: started
      Preparing wheel metadata: finished with status 'done'
  Collecting wheel
    Using cached wheel-0.36.2.tar.gz (65 kB)
  Collecting cffi!=1.11.3,>=1.8
    Using cached cffi-1.14.6.tar.gz (475 kB)
  Collecting pycparser
    Using cached pycparser-2.20.tar.gz (161 kB)
  Skipping wheel build for cffi, due to binaries being disabled for it.
  Skipping wheel build for wheel, due to binaries being disabled for it.
  Skipping wheel build for pycparser, due to binaries being disabled for it.
  Building wheels for collected packages: setuptools
    Building wheel for setuptools (PEP 517): started
    Building wheel for setuptools (PEP 517): finished with status 'done'
    Created wheel for setuptools: filename=setuptools-57.4.0-py3-none-any.whl size=819017 sha256=3e9678291e5a76e655365f7dfc8aeab77daf845505af96255d757bb44f970801
    Stored in directory: /home/paul-goins/.cache/pip/wheels/ca/fc/24/0e38bb14b221eb3ccaef4042c4bd3284db8cfbc0c2f4261141
  Successfully built setuptools
  Installing collected packages: pycparser, wheel, setuptools, cffi
      Running setup.py install for pycparser: started
      Running setup.py install for pycparser: finished with status 'done'
      Running setup.py install for wheel: started
      Running setup.py install for wheel: finished with status 'done'
      Running setup.py install for cffi: started
      Running setup.py install for cffi: finished with status 'error'
      ERROR: Command errored out with exit status 1:
       command: /tmp/tmpqba_1gu6/bin/python -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-u5guiicv/cffi_ca3bfaefb8324661b73d1052c3a536ab/setup.py'"'"'; __file__='"'"'/tmp/pip-install-u5guiicv/cffi_ca3bfaefb8324661b73d1052c3a536ab/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record /tmp/pip-record-sdc6d37k/install-record.txt --single-version-externally-managed --prefix /tmp/pip-build-env-lie7q108/overlay --compile --install-headers /tmp/pip-build-env-lie7q108/overlay/include/site/python3.6/cffi
           cwd: /tmp/pip-install-u5guiicv/cffi_ca3bfaefb8324661b73d1052c3a536ab/
      Complete output (38 lines):
      running install
      running build
      running build_py
      creating build
      creating build/lib.linux-x86_64-3.6
      creating build/lib.linux-x86_64-3.6/cffi
      copying cffi/model.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/vengine_gen.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/vengine_cpy.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/__init__.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/verifier.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/setuptools_ext.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/backend_ctypes.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/cparser.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/recompiler.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/cffi_opcode.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/error.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/api.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/pkgconfig.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/commontypes.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/lock.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/ffiplatform.py -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/_cffi_include.h -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/parse_c_type.h -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/_embedding.h -> build/lib.linux-x86_64-3.6/cffi
      copying cffi/_cffi_errors.h -> build/lib.linux-x86_64-3.6/cffi
      running build_ext
      building '_cffi_backend' extension
      creating build/temp.linux-x86_64-3.6
      creating build/temp.linux-x86_64-3.6/c
      x86_64-linux-gnu-gcc -pthread -DNDEBUG -g -fwrapv -O2 -Wall -g -fstack-protector-strong -Wformat -Werror=format-security -Wdate-time -D_FORTIFY_SOURCE=2 -fPIC -DUSE__THREAD -DHAVE_SYNC_SYNCHRONIZE -I/tmp/tmpqba_1gu6/include -I/snap/charm/609/usr/include/python3.6m -c c/_cffi_backend.c -o build/temp.linux-x86_64-3.6/c/_cffi_backend.o
      In file included from /snap/charm/609/usr/include/python3.6m/Python.h:8,
                       from c/_cffi_backend.c:2:
      /snap/charm/609/usr/include/python3.6m/pyconfig.h:3:12: fatal error: x86_64-linux-gnu/python3.6m/pyconfig.h: そのようなファイルやディレクトリはありません
          3 | #  include <x86_64-linux-gnu/python3.6m/pyconfig.h>
            |            ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
      compilation terminated.
      error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
      ----------------------------------------

----------------------------------------------------------------------

Tested on: Ubuntu focal
