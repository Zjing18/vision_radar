# vision_radar
This enhanced YOLOv5 system adds a downsampling layer for better small-object detection. Using a dual-network approach, it first detects robots, then identifies armor plates within ROIs. OpenCV-based affine transformation with a pre-calibrated matrix converts bird's-eye views to field coordinates for accurate monocular localization.
