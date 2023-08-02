
## 06. GPU 확인
설치가 잘 되었는지 확인한다.<br/>
Anaconda Prompt를 실행하여 아래 코드를 입력한다.<br/>
#### Tensorflow
```bash
python
>>> from tensorflow.python.client import device_lib
>>> print(device_lib.list_local_devices())
```
아래 `physical_device_desc:`에 자신의 그래픽카드가 뜨면 Tensorflow 설치는 성공이다.<br/>
#### Pytorch
```bash
python
>>> import torch
>>> DEVICE = ("cuda" if torch.cuda.is_available() else "cpu")
>>> print(DEVICE)
```
아래 `cuda`가 뜨면 Pytorch 설치는 성공이다.<br/>