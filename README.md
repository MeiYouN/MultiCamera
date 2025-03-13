# 多摄像头管理系统使用指南


## 1. 项目概述

本系统提供多摄像头统一管理解决方案，主要功能包括：
- 多摄像头同步控制
- 实时视频流采集
- 帧级图像处理
- 视频录制管理
- 多画面拼接显示

---

## 2. 环境要求
- Python 3.8+
- OpenCV 4.5+
- NumPy 1.21+
- 支持的摄像头设备（USBIP摄像头）

---

## 3. 安装说明
```bash
# 安装核心依赖
pip install opencv-python numpy
```

---

## 4. 快速开始
### 4.1 单摄像头操作
```python
from camera_system import Camera

# 初始化摄像头
cam = Camera(device_index=0, width=1280, height=720)

# 开始录制
cam.start_recording(output.mp4)

# 获取实时帧
frame = cam.read()

# 停止录制
cam.stop_recording()
```

### 4.2 多摄像头操作
```python
from camera_system import MultiCamera

# 初始化双摄像头系统
multi_cam = MultiCamera(camera_ids=[0, 1])

# 同步启动录制
multi_cam.start_recording_all(experiment_1)

# 显示组合画面
multi_cam.visualize(layout=(1, 2))

# 安全关闭系统
multi_cam.close_all()
```

---

## 5. 核心类说明

### 5.1 Camera类
#### 初始化参数
 参数  类型  说明 
------------------
 `device_index`  int  摄像头设备ID 
 `width`  int  采集宽度（默认1280） 
 `height`  int  采集高度（默认720） 
 `fps`  int  帧率（默认30） 

#### 核心方法
```python
# 开始视频录制
def start_recording(self, filename str, save_path str = .recordings)

# 停止录制
def stop_recording(self)

# 获取当前帧
def read(self) - np.ndarray

# 保存单帧图像
def save_frame(self, filename str, save_path str, img_type str = jpg)
```

### 5.2 MultiCamera类
#### 初始化参数
 参数  类型  说明 
------------------
 `camera_ids`  List[int]  摄像头ID列表 
 `width`  int  统一采集宽度 
 `height`  int  统一采集高度 
 `fps`  int  统一帧率 

#### 核心方法
```python
# 批量启动录制
def start_recording_all(self, session_name str)

# 停止所有录制
def stop_recording_all(self)

# 实时画面拼接显示
def visualize(self, layout tuple = (1, 2), scale float = 0.5)

# 安全关闭系统
def close_all(self)
```

---

## 6. 高级功能
### 6.1 自定义画面布局
```python
# 3摄像头田字格布局
multi_cam.visualize(layout=(2, 2), scale=0.7)
```

### 6.2 同步元数据记录
```python
# 获取系统状态
status = multi_cam.get_all_status()
print(f当前帧率 {status[0]['fps']})
```

---

## 7. 示例代码
### 7.1 实验数据采集
```python
multi_cam = MultiCamera([0, 1])

try
    multi_cam.start_recording_all(exp_202308)
    for _ in range(300)  # 采集5分钟（30fps）
        frames = multi_cam.read_all()
        process_frames(frames)  # 自定义处理逻辑
        time.sleep(130)
finally
    multi_cam.close_all()
```

### 7.2 实时监控系统
```python
multi_cam = MultiCamera([0, 1, 2])
multi_cam.visualize(layout=(2, 2))

while True
    key = cv2.waitKey(1)
    if key == ord('s')
        multi_cam.save_frames_all(snapshot)
    elif key == ord('q')
        break

multi_cam.close_all()
```

---

## 8. 注意事项
1. 设备初始化顺序影响ID分配
2. 分辨率设置需硬件支持
3. 推荐使用SSD存储视频数据
4. 多摄像头同步存在±1帧误差
5. 释放资源需调用close_all()

---

## 9. 故障排查
 现象  解决方案 
----------------
 无法打开摄像头  检查设备连接权限 
 画面卡顿  降低分辨率帧率 
 文件写入失败  检查磁盘空间权限 
 内存泄漏  确保调用close_all() 

```

 注意：本指南假设项目文件结构如下：
 ```
 project_root
 ├── camera_system
 │   ├── __init__.py
 │   ├── camera.py      # Camera类实现
 │   └── multi_cam.py   # MultiCamera类实现
 └── examples          # 使用示例
 ```