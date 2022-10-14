# tensorflow-imx8-snap

TensorFlow Lite libraries and examples for i.MX8 MP platforms. This is mostly a proof-of-concept to show that the GPU/NPU acceleration features of the i.MX8 MP are working in the context of a Snap.

Refer to the [i.MX Machine Learning User's Guide](https://community.nxp.com/pwmxy87654/attachments/pwmxy87654/imx-processors/168233/1/i.MX_Machine_Learning_User's_Guide.pdf) from NXP for more information.

## Tests

### benchmark-model

Run the following command to execute the TensorFlow Lite benchmark:

    itrue-tensorflow-imx8.benchmark-model \
        --graph=/snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite

Example result:

    STARTING!
    Log parameter values verbosely: [0]
    Graph: [/snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite]
    Use NNAPI: [0]
    Loaded model /snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite
    The input model file size (MB): 4.27635
    Initialized session in 2.517ms.
    Running benchmark for at least 1 iterations and at least 0.5 seconds but terminate if exceeding 150 seconds.
    count=3 first=217409 curr=210455 min=210455 max=217409 avg=213058 std=3096

    Running benchmark for at least 50 iterations and at least 1 seconds but terminate if exceeding 150 seconds.
    count=50 first=209891 curr=209739 min=209739 max=210599 avg=210028 std=221

    Inference timings in us: Init: 2517, First inference: 217409, Warmup (avg): 213058, Inference (avg): 210028
    Note: as the benchmark tool itself affects memory footprint, the following is only APPROXIMATE to the actual memory footprint of the model at runtime. Take the information at your discretion.
    Peak memory footprint (MB): init=0 overall=0

To enable GPU/NPU acceleration for the same benchmark (requires elevated permissions to access the device node):

    sudo itrue-tensorflow-imx8.benchmark-model \
        --graph=/snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite \
        --use_nnapi=true

Example result:

    STARTING!
    Log parameter values verbosely: [0]
    Graph: [/snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite]
    Use NNAPI: [1]
    NNAPI accelerators available: [vsi-npu]
    Loaded model /snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite
    INFO: Created TensorFlow Lite delegate for NNAPI.
    Explicitly applied NNAPI delegate, and the model graph will be completely executed by the delegate.
    The input model file size (MB): 4.27635
    Initialized session in 13.386ms.
    Running benchmark for at least 1 iterations and at least 0.5 seconds but terminate if exceeding 150 seconds.
    count=1 curr=11403859

    Running benchmark for at least 50 iterations and at least 1 seconds but terminate if exceeding 150 seconds.
    count=260 first=3918 curr=3711 min=3700 max=4026 avg=3756.87 std=35

    Inference timings in us: Init: 13386, First inference: 11403859, Warmup (avg): 1.14039e+07, Inference (avg): 3756.87
    Note: as the benchmark tool itself affects memory footprint, the following is only APPROXIMATE to the actual memory footprint of the model at runtime. Take the information at your discretion.
    Peak memory footprint (MB): init=0 overall=15.3438

### label-image

Run the following command to run the label-image example without acceleration:

    itrue-tensorflow-imx8.label-image \
        -m /snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite \
        -i /snap/itrue-tensorflow-imx8/current/usr/share/tflite-examples/grace_hopper.bmp \
        -l /snap/itrue-tensorflow-imx8/current/usr/share/tflite-examples/labels.txt

Example result:

    INFO: Loaded model /snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite
    INFO: resolved reporter
    INFO: invoked
    INFO: average time: 57.346 ms
    INFO: 0.752941: 653 653:military uniform
    INFO: 0.141176: 907 907:Windsor tie
    INFO: 0.0156863: 458 458:bow tie, bow-tie, bowtie
    INFO: 0.00784314: 835 835:suit, suit of clothes
    INFO: 0.00784314: 700 700:panpipe, pandean pipe, syrinx

Run the following command to run the label-image example with GPU/NPU acceleration (requires elevated permissions to access the device node):

    sudo itrue-tensorflow-imx8.label-image \
        -m /snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite \
        -i /snap/itrue-tensorflow-imx8/current/usr/share/tflite-examples/grace_hopper.bmp \
        -l /snap/itrue-tensorflow-imx8/current/usr/share/tflite-examples/labels.txt
        -a 1

Example result:

    INFO: Loaded model /snap/itrue-tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite
    INFO: resolved reporter
    INFO: Created TensorFlow Lite delegate for NNAPI.
    INFO: Applied NNAPI delegate.
    INFO: invoked
    INFO: average time: 3.901 ms
    INFO: 0.745098: 653 653:military uniform
    INFO: 0.141176: 907 907:Windsor tie
    INFO: 0.0156863: 458 458:bow tie, bow-tie, bowtie
    INFO: 0.00784314: 835 835:suit, suit of clothes
    INFO: 0.00784314: 700 700:panpipe, pandean pipe, syrinx
