# tensorflow-imx8-snap

TensorFlow Lite libraries and examples for i.MX8 MP platforms. This is mostly a proof-of-concept to show that the GPU/NPU acceleration features of the i.MX8 MP are working in the context of a Snap.

## Benchmark

Run the following command to execute the TensorFlow Lite benchmark:

    sudo tensorflow-imx8.benchmark-model --graph=/snap/tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite
    
To enable GPU/NPU acceleration for the same benchmark:

    sudo tensorflow-imx8.benchmark-model --graph=/snap/tensorflow-imx8/current/usr/share/models/mobilenet_v1_1.0_224_quant.tflite --use_nnapi=true
   
