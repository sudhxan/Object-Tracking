# Python Object Tracking with OpenCV

## Introduction
This README outlines the procedure for setting up and running an object tracking application using Python and OpenCV. The application uses OpenCV's built-in tracker algorithms to track objects in video streams.

## Prerequisites
- Python 3.x
- OpenCV library with contrib modules

To install OpenCV with contrib modules, run the following command:
```sh
pip install opencv-python opencv-contrib-python
```

## Files
- `ObjectDetection.py`: The main script that initializes the tracker and processes the video stream.

## Usage
1. **Initialize the Tracker:**
   The tracker dictionary `TrDict` contains various tracker types. To initialize a tracker, select one from the dictionary.
   ```python
   import cv2
   TrDict = {'csrt': cv2.TrackerCSRT_create,
             'kcf': cv2.TrackerKCF_create,
             'boosting': cv2.legacy_TrackerBoosting_create,
             'mil': cv2.TrackerMIL_create,
             'tld': cv2.legacy_TrackerTLD_create,
             'medianflow': cv2.legacy_TrackerMedianFlow_create,
             'mosse': cv2.legacy_TrackerMOSSE_create}
   tracker = TrDict['csrt']()
   ```

2. **Video Capture:**
   Capture the video stream from a file. Replace `'input_video.mp4'` with your video file's name.
   ```python
   v = cv2.VideoCapture('input_video.mp4')
   ret, frame = v.read()
   ```

3. **Select ROI:**
   Select the Region of Interest (ROI) manually by drawing a box around the object to track.
   ```python
   cv2.imshow('Frame',frame)
   bb = cv2.selectROI('Frame', frame, fromCenter=False, showCrosshair=True)
   tracker.init(frame, bb)
   ```

4. **Tracking Loop:**
   The loop reads frames from the video and updates the tracker for each frame. It draws a rectangle around the tracked object.
   ```python
   while True:
       ret, frame = v.read()
       if not ret:
           break
       success, box = tracker.update(frame)
       if success:
           (x, y, w, h) = [int(a) for a in box]
           cv2.rectangle(frame, (x, y), (x+w, y+h), (255, 255, 0), 2)
       cv2.imshow('Frame', frame)
       if cv2.waitKey(5) & 0xFF == ord('q'):
           break
   v.release()
   cv2.destroyAllWindows()
   ```

5. **Run the Application:**
   Execute the script in your terminal.
   ```sh
   python ObjectDetection.py
   ```

## Key Controls
- Press `q` to quit the application.

## Notes
- The `cv2.legacy` module is used for backward compatibility with older tracker algorithms.
- Make sure to select the object accurately when drawing the ROI for better tracking results.
