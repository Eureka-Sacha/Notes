今天在docker image中安装sparkmagic时遇到报错

> 1402.1 Collecting gssapi>=1.6.0 (from pyspnego[kerberos]->requests_kerberos>=0.8.0->sparkmagic)
> 1402.2   Downloading gssapi-1.8.3.tar.gz (94 kB)
> 1407.1      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 94.2/94.2 kB 19.1 kB/s eta 0:00:00
> 1407.1   Installing build dependencies: started
> 1570.7   Installing build dependencies: still running...
> 1574.1   Installing build dependencies: finished with status 'done'
> 1574.1   Getting requirements to build wheel: started
> 1574.2   Getting requirements to build wheel: finished with status 'error'
> 1574.2   error: subprocess-exited-with-error
> 1574.2
> 1574.2   × Getting requirements to build wheel did not run successfully.
> 1574.2   │ exit code: 1
> 1574.2   ╰─> [21 lines of output]
> 1574.2       /bin/sh: 1: krb5-config: not found
> 1574.2       Traceback (most recent call last):
> 1574.2         File "/usr/local/lib/python3.10/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
> 1574.2           main()
> 1574.2         File "/usr/local/lib/python3.10/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
> 1574.2           json_out['return_val'] = hook(**hook_input['kwargs'])
> 1574.2         File "/usr/local/lib/python3.10/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
> 1574.2           return hook(config_settings)
> 1574.2         File "/tmp/pip-build-env-_rm33gbc/overlay/local/lib/python3.10/dist-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
> 1574.2           return self._get_build_requires(config_settings, requirements=['wheel'])
> 1574.2         File "/tmp/pip-build-env-_rm33gbc/overlay/local/lib/python3.10/dist-packages/setuptools/build_meta.py", line 295, in _get_build_requires
> 1574.2           self.run_setup()
> 1574.2         File "/tmp/pip-build-env-_rm33gbc/overlay/local/lib/python3.10/dist-packages/setuptools/build_meta.py", line 311, in run_setup
> 1574.2           exec(code, locals())
> 1574.2         File "<string>", line 109, in <module>
> 1574.2         File "<string>", line 22, in get_output
> 1574.2         File "/usr/lib/python3.10/subprocess.py", line 421, in check_output
> 1574.2           return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
> 1574.2         File "/usr/lib/python3.10/subprocess.py", line 526, in run
> 1574.2           raise CalledProcessError(retcode, process.args,
> 1574.2       subprocess.CalledProcessError: Command 'krb5-config --libs gssapi' returned non-zero exit status 127.
> 1574.2       [end of output]
> 1574.2
> 1574.2   note: This error originates from a subprocess, and is likely not a problem with pip.
> 1574.2 error: subprocess-exited-with-error
> 1574.2
> 1574.2 × Getting requirements to build wheel did not run successfully.
> 1574.2 │ exit code: 1
> 1574.2 ╰─> See above for output.
> 1574.2
> 1574.2 note: This error originates from a subprocess, and is likely not a problem with pip.
> 1575.3

单从日志上看是krb5-config命令找不到

尝试直接安装 所谓的 krb5-config

```shell
# Debian
apt-get install libkrb5-dev

# CentOS
yum install krb5-devel
```

安装后报错改变

> 1228.8   × Building wheel for krb5 (pyproject.toml) did not run successfully.
> 1228.8   │ exit code: 1
> 1228.8   ╰─> [82 lines of output]
> 1228.8       Using krb5-config at 'krb5-config'
> 1228.8       Using /usr/lib/x86_64-linux-gnu/mit-krb5/libkrb5.so as Kerberos module for platform checks
> 1228.8       Compiling src/krb5/_ccache.pyx
> 1228.8       Compiling src/krb5/_ccache_mit.pyx
> 1228.8       Compiling src/krb5/_ccache_match.pyx
> 1228.8       Compiling src/krb5/_ccache_support_switch.pyx
> 1228.8       Compiling src/krb5/_cccol.pyx
> 1228.8       Compiling src/krb5/_context.pyx
> 1228.8       Compiling src/krb5/_context_mit.pyx
> 1228.8       Compiling src/krb5/_creds.pyx
> 1228.8       Compiling src/krb5/_creds_opt.pyx
> 1228.8       Skipping src/krb5/_creds_opt_heimdal.pyx as it is not supported by the selected Kerberos implementation.
> 1228.8       Compiling src/krb5/_creds_opt_mit.pyx
> 1228.8       Compiling src/krb5/_creds_opt_set_in_ccache.pyx
> 1228.8       Compiling src/krb5/_creds_opt_set_pac_request.pyx
> 1228.8       Compiling src/krb5/_exceptions.pyx
> 1228.8       Compiling src/krb5/_keyblock.pyx
> 1228.8       Compiling src/krb5/_kt.pyx
> 1228.8       Compiling src/krb5/_kt_mit.pyx
> 1228.8       Skipping src/krb5/_kt_heimdal.pyx as it is not supported by the selected Kerberos implementation.
> 1228.8       Compiling src/krb5/_kt_have_content.pyx
> 1228.8       Compiling src/krb5/_principal.pyx
> 1228.8       Skipping src/krb5/_principal_heimdal.pyx as it is not supported by the selected Kerberos implementation.
> 1228.8       Compiling src/krb5/_string.pyx
> 1228.8       Compiling src/krb5/_string_mit.pyx
> 1228.8       running bdist_wheel
> 1228.8       running build
> 1228.8       running build_py
> 1228.8       creating build
> 1228.8       creating build/lib.linux-x86_64-cpython-310
> 1228.8       creating build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/__init__.py -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       running egg_info
> 1228.8       writing src/krb5.egg-info/PKG-INFO
> 1228.8       writing dependency_links to src/krb5.egg-info/dependency_links.txt
> 1228.8       writing top-level names to src/krb5.egg-info/top_level.txt
> 1228.8       reading manifest file 'src/krb5.egg-info/SOURCES.txt'
> 1228.8       reading manifest template 'MANIFEST.in'
> 1228.8       warning: no previously-included files found matching '.coverage'
> 1228.8       warning: no previously-included files found matching '.gitignore'
> 1228.8       warning: no previously-included files found matching '.pre-commit-config.yaml'
> 1228.8       warning: no previously-included files matching '*.pyc' found under directory 'tests'
> 1228.8       adding license file 'LICENSE'
> 1228.8       writing manifest file 'src/krb5.egg-info/SOURCES.txt'
> 1228.8       copying src/krb5/_ccache.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_ccache_match.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_ccache_mit.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_ccache_support_switch.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_cccol.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_context.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_context_mit.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_creds.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_creds_opt.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_creds_opt_heimdal.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_creds_opt_mit.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_creds_opt_set_in_ccache.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_creds_opt_set_pac_request.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_exceptions.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_keyblock.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_kt.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_kt_have_content.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_kt_heimdal.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_kt_mit.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_principal.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_principal_heimdal.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_string.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/_string_mit.pyi -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/py.typed -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       copying src/krb5/python_krb5.h -> build/lib.linux-x86_64-cpython-310/krb5
> 1228.8       warning: build_py: byte-compiling is disabled, skipping.
> 1228.8
> 1228.8       running build_ext
> 1228.8       building 'krb5._ccache' extension
> 1228.8       creating build/temp.linux-x86_64-cpython-310
> 1228.8       creating build/temp.linux-x86_64-cpython-310/src
> 1228.8       creating build/temp.linux-x86_64-cpython-310/src/krb5
> 1228.8       x86_64-linux-gnu-gcc -Wno-unused-result -Wsign-compare -DNDEBUG -g -fwrapv -O2 -Wall -g -fstack-protector-strong -Wformat -Werror=format-security -g -fwrapv -O2 -fPIC -Isrc/krb5 -I/usr/include/python3.10 -c src/krb5/_ccache.c -o build/temp.linux-x86_64-cpython-310/src/krb5/_ccache.o -isystem /usr/include/mit-krb5
> 1228.8       src/krb5/_ccache.c:53:10: fatal error: Python.h: No such file or directory
> 1228.8          53 | #include "Python.h"
> 1228.8             |          ^~~~~~~~~~
> 1228.8       compilation terminated.
> 1228.8       error: command '/usr/bin/x86_64-linux-gnu-gcc' failed with exit code 1

搜索了一下需要安装其他的库

```shell
apt-get install -y python3-dev
```

重新运行，继续报错， 这次的exit code = 2

> 623.5   × Getting requirements to build wheel did not run successfully.
> 623.5   │ exit code: 1
> 623.5   ╰─> [22 lines of output]
> 623.5       /usr/bin/krb5-config: 1: cc: not found
> 623.5       Failed to find installation architecture
> 623.5       Traceback (most recent call last):
> 623.5         File "/usr/local/lib/python3.10/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
> 623.5           main()
> 623.5         File "/usr/local/lib/python3.10/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
> 623.5           json_out['return_val'] = hook(**hook_input['kwargs'])
> 623.5         File "/usr/local/lib/python3.10/dist-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
> 623.5           return hook(config_settings)
> 623.5         File "/tmp/pip-build-env-12oqjyus/overlay/local/lib/python3.10/dist-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
> 623.5           return self._get_build_requires(config_settings, requirements=['wheel'])
> 623.5         File "/tmp/pip-build-env-12oqjyus/overlay/local/lib/python3.10/dist-packages/setuptools/build_meta.py", line 295, in _get_build_requires
> 623.5           self.run_setup()
> 623.5         File "/tmp/pip-build-env-12oqjyus/overlay/local/lib/python3.10/dist-packages/setuptools/build_meta.py", line 311, in run_setup
> 623.5           exec(code, locals())
> 623.5         File "<string>", line 109, in <module>
> 623.5         File "<string>", line 22, in get_output
> 623.5         File "/usr/lib/python3.10/subprocess.py", line 421, in check_output
> 623.5           return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
> 623.5         File "/usr/lib/python3.10/subprocess.py", line 526, in run
> 623.5           raise CalledProcessError(retcode, process.args,
> 623.5       subprocess.CalledProcessError: Command 'krb5-config --libs gssapi' returned non-zero exit status 2.

尝试安装gcc

`apt-get install -y`
