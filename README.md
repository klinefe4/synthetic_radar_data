# synthetic_radar_data
Python code for generating radar returns from the MS-ASL dataset with the Google Mediapipe hand landmark detector.  This repo contains the CSV files with detected landmarks for the top 100 classes (ASL100) in the dataset, which you can use directly to generate micro-Doppler responses.  If you would like to generate the responses yourself you can download the dataset and install mediapipe locally.

<h2>Installation (optional)</h2>

1. Download the MS-ASL dataset from  https://www.microsoft.com/en-us/download/details.aspx?id=100121
1. Install Google mediapipe via the instructions [here](https://github.com/google/mediapipe/blob/master/mediapipe/docs/install.md)

<h2>Generating the landmarks (optional)</h2>

1. Generate the CSV file to be read by mediapipe.  This uses [pafy](https://pypi.org/project/pafy/) to get the actual youtube play URL's from the URL's given in the MS-ASL dataset.
```bash
python3 MSASL_to_csv.py
```
2. Add demo_run_graph_main_gpu_out_multi.cc to your mediapipe build then build it.

  ```bash
  bazel build -c opt --copt -DMESA_EGL_NO_X11_HEADERS \
      mediapipe/examples/desktop/multi_hand_tracking:multi_hand_tracking_gpu_out
  ```
3. Run the tracker to generate landmark CSV's.
  ```bash
  GLOG_logtostderr=1 bazel-bin/mediapipe/examples/desktop/multi_hand_tracking/multi_hand_tracking_gpu_out \
      --calculator_graph_config_file=mediapipe/graphs/hand_tracking/multi_hand_tracking_mobile.pbtxt
  ```

Now each video in ASL100 has it's corresponding CSV file containing it's count, class number, the class word, signer index, video frames per second, and each frame with the X,Y,Z values of the detected landmarks.
  
<h2>Generating micro-Doppler responses</h2>
  
  
