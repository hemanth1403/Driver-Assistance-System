# MAJOR PROJECT - 2024

<h1>Driver Assistance System</h1>

Road safety has always been an important concern for development and public safety since it is one of the main causes of injury and death across the world.
Active engagement with the development of modern transportation systems, road safety measures, and other collaborations could assist in reducing the number of traffic accidents.
The proposed research seeks to make a contribution significantly to the various initiatives to broaden Advanced Driver Assistance Systems (ADASs) and Autonomous Vehicles (AVs).
It comprises a set up of six components: drowsiness recognition and alerting system, lane detection, lane departure warning system, lane maintaining assistance system,
object detection and recognition, and collision warning system. The drowsiness detection and alert system is engaged throughout the ride to monitor the driver's focus levels.
Lane detection is then carried out using an ultrafast lane detector, that delivers spatial awareness by recognizing road lane markings.
The lane departure system thus activates whenever the vehicle deviates from the lane without signaling, alerting the driver immediately.
As the vehicle continues to deviate from the lane, the lane keeping assistance system detects and adjusts the vehicle's steering to keep it in the lane.
Concurrently, recognition and detection of objects applying YOLO recognize a wide range of items on the road, that serve as crucial in collision warning systems.
The collision warning system employs OpenCV and distance measurement to monitor the vehicle's surroundings and alert the driver about possible collision risks.
The integration aims to enhance driver safety through providing timely warnings and offering assistance throughout the driving process.

<h2>Proposed system</h2>

<img src="https://github.com/hemanth1403/Driver-Assistance-System/blob/main/Architecture%20and%20Flow%20charts/Proposed_system.png">

<br>

The proposed system has 2 point of views

1. POV_1 -> Monitoring the external environment
2. POV_2 -> Monitoring the internal environment

<h2>POV_1 : Monitoring the external environment </h2>

<h3>class UML diagram</h3>
<img src="https://github.com/hemanth1403/Driver-Assistance-System/blob/main/UML%20Diagrams/POV_1/Class_UML.png">

<h3>Collaboration UML diagram</h3>
<img src="https://github.com/hemanth1403/Driver-Assistance-System/blob/main/UML%20Diagrams/POV_1/Collaboration_UML.png">

<h3>Sequence UML diagram</h3>
<img src="https://github.com/hemanth1403/Driver-Assistance-System/blob/main/UML%20Diagrams/POV_1/Sequence_UML.png">

<h3>Usecase UML Diagram</h3>
<img src="https://github.com/hemanth1403/Driver-Assistance-System/blob/main/UML%20Diagrams/POV_1/UseCase_UML.png">

<h2>POV_2 : Monitoring the internal environment </h2>
<h3>UML diagram</h3>
<img src="https://github.com/hemanth1403/Driver-Assistance-System/blob/main/UML%20Diagrams/POV_2/Drowsiness.png">

<h3>Psudo code</h3>
<img src="https://github.com/hemanth1403/Driver-Assistance-System/blob/main/Architecture%20and%20Flow%20charts/Psudo_Drowsiness.png">

<h2>Code setup : </h2>
<h3>Requirements : </h3>

- **Python 3.7+**

- **OpenCV**, **Scikit-learn**, **onnxruntime**, **pycuda** and **pytorch**.

- **Install :**

  The `requirements.txt` file should list all Python libraries that your notebooks
  depend on, and they will be installed using:

  ```
  pip install -r requirements.txt
  ```

<h3>Examples :</h3>

- **_Download YOLO Series Onnx model_** :

  Use the Google Colab notebook to convert

  | Model       | release version | Link                                                                                                                                                                |
  | :---------- | :-------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
  | YOLOv5      | `v6.2`          | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1mwoA3_-f3QIcHtLSuGN5WVszKeZ_i366?usp=sharing) |
  | YOLOv6/Lite | `0.4.0`         | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1FhyQvDUzUVgPwYB1DSADfCm_CG09D9Ab?usp=sharing) |
  | YOLOv7      | `v0.1`          | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1arGcVT32Sm3zxhql2jgAa5xIEZdsDq9D?usp=sharing) |
  | YOLOv8      | `8.1.27`        | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1mrhgTaZFQWWwhf0jcMwD_tOjmXfMh3pS?usp=sharing) |
  | YOLOv9      | `v0.1`          | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/12oFXgco3CARzhU8CiLCpf_6oBA3sAvPT?usp=sharing) |

- **_Convert Onnx to TenserRT model_** :

  Need to modify `onnx_model_path` and `trt_model_path` before converting.

  ```
  python convertOnnxToTensorRT.py -i <path-of-your-onnx-model>  -o <path-of-your-trt-model>
  ```

- **_Quantize ONNX models_** :

  Converting a model to use float16 instead of float32 can decrease the model size.

  ```
  python onnxQuantization.py -i <path-of-your-onnx-model>
  ```

- **_Video Inference_** :

  - Setting Config :
    > Note : can support onnx/tensorRT format model. But it needs to match the same model type.

  ```python
  lane_config = {
   "model_path": "./TrafficLaneDetector/models/culane_res18.trt",
   "model_type" : LaneModelType.UFLDV2_CULANE
  }

  object_config = {
   "model_path": './ObjectDetector/models/yolov8l-coco.trt',
   "model_type" : ObjectModelType.YOLOV8,
   "classes_path" : './ObjectDetector/models/coco_label.txt',
   "box_score" : 0.4,
   "box_nms_iou" : 0.45
  }
  ```

  | Target | Model Type                      | Describe                                         |
  | :----: | :------------------------------ | :----------------------------------------------- |
  | Lanes  | `LaneModelType.UFLD_TUSIMPLE`   | Support Tusimple data with ResNet18 backbone.    |
  | Lanes  | `LaneModelType.UFLD_CULANE`     | Support CULane data with ResNet18 backbone.      |
  | Lanes  | `LaneModelType.UFLDV2_TUSIMPLE` | Support Tusimple data with ResNet18/34 backbone. |
  | Lanes  | `LaneModelType.UFLDV2_CULANE`   | Support CULane data with ResNet18/34 backbone.   |
  | Object | `ObjectModelType.YOLOV5`        | Support yolov5n/s/m/l/x model.                   |
  | Object | `ObjectModelType.YOLOV5_LITE`   | Support yolov5lite-e/s/c/g model.                |
  | Object | `ObjectModelType.YOLOV6`        | Support yolov6n/s/m/l, yolov6lite-s/m/l model.   |
  | Object | `ObjectModelType.YOLOV7`        | Support yolov7 tiny/x/w/e/d model.               |
  | Object | `ObjectModelType.YOLOV8`        | Support yolov8n/s/m/l/x model.                   |
  | Object | `ObjectModelType.YOLOV9`        | Support yolov9s/m/c/e model.                     |
  | Object | `ObjectModelType.EfficientDet`  | Support efficientDet b0/b1/b2/b3 model.          |

  - Run for POV_1:

  ```
  python demo.py
  ```

  - Run for POV_2:
    (change the directory to Drowsiness detector)

  ```
  python detect.py
  ```

<h2>Hardware setup : </h2>
<h3>Hardware requirements : </h3>
&nbsp  &nbsp &nbsp 1. Jetson orin nano <br>
&nbsp  &nbsp &nbsp 2. Servo motor <br>
&nbsp  &nbsp &nbsp 3. Pi camera module 3 <br>
&nbsp  &nbsp &nbsp 4. Power bank (10000 to 20000 mAh) <br>
&nbsp  &nbsp &nbsp 5. Arduino Nano <br>
&nbsp  &nbsp &nbsp 6. ADXL - 345 <br>
&nbsp  &nbsp &nbsp 7. GPS Neo - 6m <br>
&nbsp  &nbsp &nbsp 8. GSM SIM800I <br>
&nbsp  &nbsp &nbsp 9. LM2596 step converter <br>
&nbsp  &nbsp &nbsp 10. Zero PCB <br>
&nbsp  &nbsp &nbsp 11. 12v 2A Power supply <br>
&nbsp  &nbsp &nbsp 12. 15 pin to 22 pin cable <br>
<h3>Hardware Integration : </h3>
<img src="">
