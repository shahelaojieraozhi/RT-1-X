# Step of Run RT-1 repo

ref https://github.com/google-research/robotics_transformer/issues/1    @**[TX-Leo](https://github.com/TX-Leo)**  and  @**[tyk15886924522](https://github.com/tyk15886924522)**



## Conda env

```sh
conda create -n rt1
conda activate rt1
# install python3.11（Python 3.11.5）
conda install python
```

## 1.tensor2robot

### 1)git clone tensor2robot

```
git clone https://github.com/google-research/tensor2robot.git
```

### 2)modify tensor2robot/requirements.txt

```
pybullet==2.5.0  --> pybullet
```

after modify.  tensor2robot/requirements.txt: 

```sh
absl-py>=0.5.0
numpy>=1.13.3
tensorflow>=1.13.0
tensorflow-serving-api>=1.13.0
gin-config>=0.1.4
pybullet==2.5.0
Pillow==5.3.0
gym>=0.10.9
tensorflow-probability>=0.6.0
tf-slim>=1.0
```

### 3)pip install

```
pip install -r tensor2robot/requirements.txt
```

### 4)test

```
python -m tensor2robot.research.pose_env.pose_env_test
```

**If success,  then show:**

```sh
[ RUN      ] PoseEnvTest.test_PoseEnv
[       OK ] PoseEnvTest.test_PoseEnv
----------------------------------------------------------------------
Ran 1 test in 0.599s

OK
```



## 2.rt1

### 1)git clone rt1

```
git clone https://github.com/google-research/robotics_transformer.git
```

### 2)Comment out the last line of robotics_transformer/requirements.txt

```
# git+https://github.com/google-research/tensor2robot#tensor2robot
```

### 3)pip install

```
pip install -r robotics_transformer/requirements.txt
```

### 4)Downgrade the protobuf package to 3.20.1

```
pip install protobuf==3.20.1
```

### 5)!!!!!modify tensor2robot/utils/tensorspec_utils.py

after modify  tensor2robot/utils/tensorspec_utils.py   （show line 30~34）

```
from google.protobuf import text_format
import tensorflow as tf

nest = tf.nest
TSPEC = tf.TensorSpec
```

### 6)Compile the protobufs

```
cd tensor2robot/proto
protoc -I=./ --python_out=`pwd` t2r.proto
```

then you can get a .py file named as t2r_pb2.py  ,  **note:** 'pwd' is not your admin password, it is just str

### 7)test action tokenizer

```
python -m robotics_transformer.tokenizers.action_tokenizer_test
```



**If success,  then show:**

```sh
.....
[       OK ] ActionTokenizerTest.testTokenize_invalid_action_spec_shape
[ RUN      ] ActionTokenizerTest.test_session
[  SKIPPED ] ActionTokenizerTest.test_session
----------------------------------------------------------------------
Ran 9 tests in 0.502s

OK (skipped=1)
```

