# Hands-On: Implement a native example service



## Limitations:

 - The example service ignores all permission handling.
 - No auto start over the init deamon.
 - The service needs manual deployment, it is not added to a device/product.
 - No integration of the service's git repo into the build tree with `repo`.


## Hints

 - To format the source code run `clang-format -i *.cpp *.h`.


# Step 1: Service setup and initial build

1. Clone or copy the `hands-on_native-service` folder to `system/demo` in the AOSP source tree.

2. Make sure the service builds without errors.


Verify:

If the service is build correctly you can find it's binary under:
`out/target/product/trout_x86_64/system/bin/demo`


Hints:

 - The service is not added to a device/product.
 - Remember `hmm` to build the service.


# Step 2: Run the service on a (virtual) device

As the service is not integrated into an product it is not automatically
installed.

To install it by hand the system partition needs to be writable.

```
adb root
# Only needed once (`adb reboot` required)
adb disable-verity
adb remount system
adb sync system
```

And then run the service manually `demo`.

Call the service with the `service call ...` util.


Verify:

 - The running service is shown in `service list`.
    (If the service list command hangs stop it with CTRL-C)
 - Running `demo call-service` works.
 - `service call ...` works.



# Step 3: Log to the system log buffers (logcat)

Currently the service only logs to the standard output.
But if the service is running under the `init` daemon these logs would not show
up in the logcat.

Add logging to the system logcat buffer to the service calls.


## Verify

The added log messages appearer in `adb logcat`.


## Hints

 - Check how other services do the logging.
 - Logcat supports log filters see `logcat` on how to reduce the log output.


# Step 4: Implement a new AIDL method

Add the function `int calculate(int a, int b)`
to the AIDL interface and implement it.

This function should add the two numbers (a+b) and return the result.

Extend the client part of the service to call the new function.


## Verify

With the `service` command you can call any service provided over the service manager.
Call the new service implementation. (The `service` command has a help / usage message.)


## Hints

 - Check other services or the AIDL documentation how to add the function.


# Step X: (Optional) Add the service to product and init system


See:
 - Other services.
 - https://android.googlesource.com/platform/system/core/+/master/init/README.md


Verify:
 - Service is automatically build with `m`.
    (You can delete the service binary manually)
 - Service is started by the init daemon. Check with `demo call-service`
