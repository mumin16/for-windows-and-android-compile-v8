# for-windows-and-android-compile-v8

1-download depot_tools(https://storage.googleapis.com/chrome-infra/depot_tools.zip) and  extract to c:\ and then add c:\depot_tools to "system variables".

2-following two entries add to "user variables" :

DEPOT_TOOLS_WIN_TOOLCHAIN=0

GYP_MSVS_VERSION =2017


3-

open command prompt

cd c:\depot_tools

fetch v8

cd v8

gclient sync

4-

for android:

python tools/dev/v8gen.py arm.release

for windows:

python tools/dev/v8gen.py x64.release

5-

for android:

gn args out.gn/arm.release

add following entries to opened text file:

target_os = "android"      # These lines need to be changed manually

target_cpu = "arm"         # as v8gen.py assumes a simulator build.

v8_target_cpu = "arm"

is_component_build = false


for windows:

gn args out.gn\x64.release

add following entries to opened text file:

is_debug = false

target_cpu = "x64"

is_component_build = false

v8_static_library = true


6-

for android:

ninja -C out.gn/arm.release d8


for windows:

ninja -C out.gn/x64.release

7-

for test:

tools/run-tests.py --gn
