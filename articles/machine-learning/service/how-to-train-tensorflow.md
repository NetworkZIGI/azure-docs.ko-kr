---
title: TensorFlow를 사용하여 모델 학습
titleSuffix: Azure Machine Learning service
description: TensorFlow Estimator를 사용하여 TensorFlow 모델의 단일 노드 및 분산 학습을 실행하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: c76a94695114888ca8946106528fe179ff81c811
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244728"
---
# <a name="train-tensorflow-models-with-azure-machine-learning-service"></a>Azure Machine Learning Service를 사용하여 TensorFlow 모델 학습

Azure Machine Learning에서는 TensorFlow를 사용한 DNN(심층 신경망) 학습을 위해 `Estimator`의 사용자 지정 `TensorFlow` 클래스를 제공합니다. Azure SDK의 `TensorFlow` Estimator([`tf.estimator.Estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator) 클래스와 통합되지 않음)를 사용하면 Azure Compute의 단일 노드 및 분산 실행 둘 다에 대해 TensorFlow 학습 작업을 쉽게 제출할 수 있습니다.

## <a name="single-node-training"></a>단일 노드 학습
`TensorFlow` Estimator를 사용하는 학습은 [기본 `Estimator`](how-to-train-ml-models.md)를 사용하는 방식과 비슷하므로 먼저 방법 문서를 확인하고 이 문서에서 소개하는 개념을 이해해야 합니다.
  
TensorFlow 작업을 실행하려면 `TensorFlow` 개체를 인스턴스화합니다. [계산 대상](how-to-set-up-training-targets.md#amlcompute) 개체 `compute_target`는 이미 만든 상태여야 합니다.

```Python
from azureml.train.dnn import TensorFlow

script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py',
                    conda_packages=['scikit-learn'],
                    use_gpu=True)
```

여기서는 TensorFlow 생성자에 대해 다음 매개 변수를 지정합니다.

매개 변수 | 설명
--|--
`source_directory` | 학습 작업에 필요한 모든 코드가 포함된 로컬 디렉터리입니다. 이 폴더는 로컬 컴퓨터에서 원격 컴퓨터로 복사됩니다.
`script_params` | <명령줄 인수, 값> 쌍 형식으로 학습 스크립트 `entry_script`에 대한 명령줄 인수를 지정하는 사전입니다.
`compute_target` | 이 경우 학습 스크립트가 Azure Machine Learning 컴퓨팅([AmlCompute](how-to-set-up-training-targets.md#amlcompute)) 클러스터에서 실행되는 원격 컴퓨팅 대상
`entry_script` | 원격 계산에서 실행할 학습 스크립트의 파일 경로(`source_directory` 기준)입니다. 이 파일 및 이 파일이 의존하는 추가 파일은 이 폴더에 있어야 합니다.
`conda_packages` | conda를 통해 설치할 학습 스크립트에 필요한 Python 패키지의 목록입니다. 여기서 학습 스크립트는 `sklearn`을 사용하여 데이터를 로드하므로 이 패키지를 설치하도록 지정합니다.  생성자에는 필요한 모든 pip 패키지에 사용할 수 있는 `pip_packages` 매개 변수도 있습니다.
`use_gpu` | 학습에 GPU를 활용하려면 이 플래그를 `True`로 설정합니다. 기본값은 `False`입니다.

여기서는 TensorFlow Estimator를 사용하므로 학습에 사용되는 컨테이너는 TensorFlow 패키지 및 CPU와 GPU에서 학습에 필요한 관련 종속성을 기본적으로 포함합니다.

다음으로는 TensorFlow 작업을 제출합니다.
```Python
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>분산 학습
TensorFlow Estimator에서는 Azure VM의 여러 CPU 및 GPU 클러스터에서 광범위하게 모델을 학습시킬 수도 있습니다. API 호출만 몇 번 수행하면 분산 TensorFlow 학습을 쉽게 실행할 수 있습니다. 이러한 워크로드를 수행하는 데 필요한 모든 인프라와 오케스트레이션은 Azure Machine Learning이 백그라운드에서 관리합니다.

Azure Machine Learning은 TensorFlow에서 두 가지 분산 학습 방법을 지원합니다.
* [Horovod](https://github.com/uber/horovod) 프레임워크를 사용하는 MPI 기반 분산 학습
* 매개 변수 서버 방법을 통한 기본 [분산 TensorFlow](https://www.tensorflow.org/deploy/distributed)

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod)는 Uber에서 개발한 분산 학습용 오픈 소스 ring-allreduce 프레임워크입니다.

Horovod 프레임워크를 사용하여 분산 TensorFlow를 실행하려면 다음과 같이 TensorFlow 개체를 만듭니다.

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi',
                    use_gpu=True)
```

위의 코드는 TensorFlow 생성자에 대해 다음의 새 매개 변수를 표시합니다.

매개 변수 | 설명 | 기본값
--|--|--
`node_count` | 학습 작업에 사용할 노드의 수입니다. | `1`
`process_count_per_node` | 각 노드에서 실행할 프로세스(또는 “작업자”)의 수입니다.|`1`
`distributed_backend` | Estimator가 MPI를 통해 제공하는 분산 학습 시작을 위한 백 엔드입니다. MPI(와 Horovod)를 사용하여 병렬 또는 분산 학습을 수행하려는 경우(예: `node_count`>1 또는 `process_count_per_node`>1이거나 둘 다 해당됨) `distributed_backend='mpi'`를 설정합니다. Azure Machine Learning에서 사용하는 MPI 구현은 [Open MPI](https://www.open-mpi.org/)입니다. | `None`

위의 예제에서는 작업자 2명(노드당 작업자 1명)을 통해 분산 학습을 실행합니다.

Horovod 및 해당 종속성은 자동으로 설치되므로 다음과 같이 학습 스크립트 `train.py`에 가져오기만 하면 됩니다.

```Python
import tensorflow as tf
import horovod
```

마지막으로 TensorFlow 작업을 제출합니다.
```Python
run = exp.submit(tf_est)
```

### <a name="parameter-server"></a>매개 변수 서버
매개 변수 서버 모델을 사용하는 [기본 분산 TensorFlow](https://www.tensorflow.org/deploy/distributed)를 실행할 수도 있습니다. 이 방법에서는 매개 변수 서버 및 작업자 클러스터 전반에 걸쳐 학습을 실행합니다. 작업자는 학습 중에 그라데이션을 계산하고 매개 변수 서버는 그라데이션을 집계합니다.

TensorFlow 개체를 생성합니다.

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps',
                    use_gpu=True)
```

위의 코드에서 TensorFlow 생성자에 대한 다음 매개 변수를 자세히 살펴보세요.

매개 변수 | 설명 | 기본값
--|--|--
`worker_count` | 작업자 수입니다. | `1`
`parameter_server_count` | 매개 변수 서버 수입니다. | `1`
`distributed_backend` | 분산 학습에 사용할 백 엔드입니다. 매개 변수 서버를 통해 분산 학습을 수행하려면 `distributed_backend='ps'`를 설정합니다. | `None`

#### <a name="note-on-tfconfig"></a>`TF_CONFIG` 관련 참고 사항
Azure Machine Learning이 `TF_CONFIG` 환경 변수를 자동으로 설정하도록 하려면 [`tf.train.ClusterSpec`](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec)에 대한 클러스터의 네트워크 주소와 포트도 필요합니다.

`TF_CONFIG` 환경 변수는 JSON 문자열입니다. 매개 변수 서버의 변수 예제는 다음과 같습니다.
```
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

TensorFlow의 고급 [`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator) API를 사용 중인 경우 TensorFlow는 이 `TF_CONFIG` 변수를 구문 분석하여 클러스터 사양을 자동으로 작성합니다. 

TensorFlow의 하위 수준 코어 API를 학습에 사용 중인 경우에는 학습 코드에서 `TF_CONFIG` 변수를 구문 분석하고 `tf.train.ClusterSpec`을 직접 작성해야 합니다. [이 예제](https://aka.ms/aml-notebook-tf-ps)에서는 다음과 같이 **학습 스크립트**에서 해당 작업을 수행합니다.

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

학습 스크립트 작성과 TensorFlow 개체 만들기를 완료한 후에는 학습 작업을 제출할 수 있습니다.
```Python
run = exp.submit(tf_est)
```

## <a name="examples"></a>예

분산형 딥러닝에서 Notebook은 다음을 참조하세요.
* [how-to-use-azureml/training-with-deep-learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>다음 단계
* [학습 중에 실행 메트릭 추적](how-to-track-experiments.md)
* [하이퍼 매개 변수 조정](how-to-tune-hyperparameters.md)
* [학습된 모델 배포](how-to-deploy-and-where.md)
