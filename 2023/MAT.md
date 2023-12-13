# Issues in MAT
## 1
***
First I met a problem: 
```
Setting up PyTorch plugin "bias_act_plugin"... D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\torch\utils\cpp_extension.py:287: UserWarning: Error checking compiler version for cl: 
'utf-8' codec can't decode byte 0xd3 in position 0: invalid continuation byte
  warnings.warn('Error checking compiler version for {}: {}'.format(compiler, error))

```
**FIXED**: open "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\torch\utils\cpp_extension.py", find this code:
```
try:
        if sys.platform.startswith('linux'):
            minimum_required_version = MINIMUM_GCC_VERSION
            version = subprocess.check_output([compiler, '-dumpfullversion', '-dumpversion'])
            version = version.decode().strip().split('.')
        else:
            print("----windows operation system")
            minimum_required_version = MINIMUM_MSVC_VERSION
            compiler_info = subprocess.check_output(compiler, stderr=subprocess.STDOUT)
            # the default decode is 'utf8', since cl.exe will return Chinese, so ' gbk'
            #match = re.search(r'(\d+)\.(\d+)\.(\d+)', compiler_info.decode().strip())
            print("----compiler_info: ", compiler_info.decode(' gbk'))
            match = re.search(r'(\d+)\.(\d+)\.(\d+)', compiler_info.decode().strip())
            print("----match: ", match)
            version = (0, 0, 0) if match is None else match.groups()
```
change
```
compiler_info.decode()
```
to
```
compiler_info.decode(' gbk')
```
Note: must go into the Administrator account, otherwise the file CANNOT be saved.


## 2
***
Note: The version of CUDA Toolkit's Visual Studio Integration must match the Visual Studio's version. e.g. Visual Studio 2022 CANNOT fit Visual Studio Integration 11.0.

## 3
***
Then I met another problem:
```
Setting up PyTorch plugin "bias_act_plugin"... Failed!
D:\PythonProject\MAT\MAT\torch_utils\ops\bias_act.py:50: UserWarning: Failed to build CUDA kernels for bias_act. Falling back to slow reference implementation. Details:

Traceback (most recent call last):
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\setuptools\msvc.py", line 175, in _msvc14_get_vc_env
    stderr=subprocess.STDOUT,
  File "D:\PolyU\Anaconda3\envs\MAT\lib\subprocess.py", line 389, in check_output
    **kwargs).stdout
  File "D:\PolyU\Anaconda3\envs\MAT\lib\subprocess.py", line 481, in run
    output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command 'cmd /u /c "C:\Program Files (x86)\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvarsall.bat" x86_amd64 && set' returned non-ze
ro exit status 255.

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "D:\PythonProject\MAT\MAT\torch_utils\ops\bias_act.py", line 48, in _init
    _plugin = custom_ops.get_plugin('bias_act_plugin', sources=sources, extra_cuda_cflags=['--use_fast_math'])
  File "D:\PythonProject\MAT\MAT\torch_utils\custom_ops.py", line 110, in get_plugin
    torch.utils.cpp_extension.load(name=module_name, verbose=verbose_build, sources=sources, **build_kwargs)
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\torch\utils\cpp_extension.py", line 997, in load
    keep_intermediates=keep_intermediates)
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\torch\utils\cpp_extension.py", line 1202, in _jit_compile
    with_cuda=with_cuda)
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\torch\utils\cpp_extension.py", line 1300, in _write_ninja_file_and_build_library
    error_prefix="Error building extension '{}'".format(name))
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\torch\utils\cpp_extension.py", line 1509, in _run_ninja_build
    vc_env = _get_vc_env(plat_spec)
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\setuptools\msvc.py", line 214, in msvc14_get_vc_env
    return _msvc14_get_vc_env(plat_spec)
  File "D:\PolyU\Anaconda3\envs\MAT\lib\site-packages\setuptools\msvc.py", line 180, in _msvc14_get_vc_env
    ) from exc
distutils.errors.DistutilsPlatformError: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-bu
ild-tools/
```
**FIXED**: The reason is I've installed 2 different version of Visual Studio simutaneously: 2019 and 2022. The CUDA Visual Studio Integration 11.0 only fit Visual Studio 2019. However, based on the msvc.py, it will check the latest version of Visual Studio automatically. Thus, only Visual Studio 2022 could be searched and it is not matched to the project, that's why CalledProcessError was invoked. So the solution is to simply uninstall Visual Studio 2022.
