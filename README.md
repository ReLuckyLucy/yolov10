# [YOLOv10:实时端到端目标检测](https://arxiv.org/abs/2405.14458)




<p align="center">
  <img src="figures/latency.svg" width=48%>
  <img src="figures/params.svg" width=48%> <br>
</p>


## 性能
| Model | Test Size | #Params | FLOPs | AP<sup>val</sup> | Latency |
|:---------------|:----:|:---:|:--:|:--:|:--:|
| [YOLOv10-N](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10n.pt) |   640  |     2.3M    |   6.7G   |     38.5%     | 1.84ms |
| [YOLOv10-S](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10s.pt) |   640  |     7.2M    |   21.6G  |     46.3%     | 2.49ms |
| [YOLOv10-M](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10m.pt) |   640  |     15.4M   |   59.1G  |     51.1%     | 4.74ms |
| [YOLOv10-B](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10b.pt) |   640  |     19.1M   |  92.0G |     52.5%     | 5.74ms |
| [YOLOv10-L](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10l.pt) |   640  |     24.4M   |  120.3G   |     53.2%     | 7.28ms |
| [YOLOv10-X](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10x.pt) |   640  |     29.5M    |   160.4G   |     54.4%     | 10.70ms |



## 下载

`conda` virtual environment is recommended. 
```
conda create -n yolov10 python=3.9
conda activate yolov10
pip install -r requirements.txt
pip install -e .
```



## 验证

[`yolov10n.pt`](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10n.pt)  [`yolov10s.pt`](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10s.pt)  [`yolov10m.pt`](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10m.pt)  [`yolov10b.pt`](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10b.pt)  [`yolov10l.pt`](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10l.pt)  [`yolov10x.pt`](https://github.com/jameslahm/yolov10/releases/download/v1.0/yolov10x.pt)  
```
yolo val model=yolov10n/s/m/b/l/x.pt data=coco.yaml batch=256
```



## 训练 

```
yolo detect train data=coco.yaml model=yolov10n/s/m/b/l/x.yaml epochs=500 batch=256 imgsz=640 device=0,1,2,3,4,5,6,7
```



## 预测

```
yolo predict model=yolov10n/s/m/b/l/x.pt
```



## 出口

```
# End-to-End ONNX
yolo export model=yolov10n/s/m/b/l/x.pt format=onnx opset=13 simplify
# Predict with ONNX
yolo predict model=yolov10n/s/m/b/l/x.onnx

# End-to-End TensorRT
yolo export model=yolov10n/s/m/b/l/x.pt format=engine half=True simplify opset=13 workspace=16
# Or
trtexec --onnx=yolov10n/s/m/b/l/x.onnx --saveEngine=yolov10n/s/m/b/l/x.engine --fp16
# Predict with TensorRT
yolo predict model=yolov10n/s/m/b/l/x.engine
```



## 可视化查看

让我们尝试部署一下，譬如先导出个 onnx 模型出来看看：

```
yolo export model=yolov10s.pt format=onnx opset=13 simplify
```

好了，接下来通过执行 `pip install netron` 安装个可视化工具来看看导出的节点信息：

```
# run python fisrt
import netron
netron.start('/path/to/yolov10s.onnx')
```

先直接通过 Ultralytics 框架预测一个测试下能否正常推理：

```
yolo predict model=yolov10s.onnx source=ultralytics/assets/bus.jpg
```

