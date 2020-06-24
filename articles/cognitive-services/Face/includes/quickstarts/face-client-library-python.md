---
title: 人脸 Python 客户端库快速入门
description: 本快速入门将帮助你开始使用适用于 Python 的人脸客户端库来检测、查找相似内容、识别、验证等。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: include
ms.date: 06/12/2020
ms.author: v-tawe
origin.date: 04/14/2020
ms.openlocfilehash: dc89baf01754545c15cb969107180deee784809a
ms.sourcegitcommit: 3de7d92ac955272fd140ec47b3a0a7b1e287ca14
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84723784"
---
适用于 Python 的人脸客户端库入门。 请按照以下步骤安装程序包并试用基本任务的示例代码。 通过人脸服务，可以访问用于检测和识别图像中的人脸的高级算法。

使用适用于 Python 的人脸客户端库可以：

* 在图像中检测人脸
* 查找相似人脸
* 创建和训练人员组
* 识别人脸
* 验证人脸
* 创建用于数据迁移的快照

[参考文档](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python) | [库源代码](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-vision-face) | <!--Pipy not available-->[示例](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>先决条件

* Azure 订阅 - [创建试用订阅](https://wd.azure.cn/pricing/1rmb-trial-full)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>设置

### <a name="create-a-face-azure-resource"></a>创建人脸 Azure 资源

Azure 认知服务由你订阅的 Azure 资源表示。 在本地计算机上使用 [Azure 门户](/cognitive-services/cognitive-services-apis-create-account)或 [Azure CLI](/cognitive-services/cognitive-services-apis-create-account-cli) 创建人脸资源。 <!-- not available-->

获取试用订阅或资源的密钥后，请为该密钥和终结点[创建环境变量](/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)，分别名为 `FACE_SUBSCRIPTION_KEY` 和 `FACE_ENDPOINT`。
 
### <a name="create-a-new-python-application"></a>创建新的 Python 应用程序

创建新的 Python 脚本 &mdash; 例如 *quickstart-file.py*。 在喜好的编辑器或 IDE 中打开该文件，并导入以下库。

```python
import asyncio
import io
import glob
import os
import sys
import time
import uuid
import requests
from urllib.parse import urlparse
from io import BytesIO
from PIL import Image, ImageDraw
from azure.cognitiveservices.vision.face import FaceClient
from msrest.authentication import CognitiveServicesCredentials
from azure.cognitiveservices.vision.face.models import TrainingStatusType, Person, SnapshotObjectType, OperationStatusType
```

然后，为该资源的 Azure 终结点和密钥创建变量。

```python
# Set the FACE_SUBSCRIPTION_KEY environment variable with your key as the value.
# This key will serve all examples in this document.
KEY = os.environ['FACE_SUBSCRIPTION_KEY']

# Set the FACE_ENDPOINT environment variable with the endpoint from your Face service in Azure.
# This endpoint will be used in all examples in this quickstart.
ENDPOINT = os.environ['FACE_ENDPOINT']
```

> [!NOTE]
> 如果在启动应用程序后创建了环境变量，则需要关闭再重新打开运行该应用程序的编辑器、IDE 或 shell 才能访问该变量。

### <a name="install-the-client-library"></a>安装客户端库

可使用以下方式安装客户端库：

```console
pip install --upgrade azure-cognitiveservices-vision-face
```

## <a name="object-model"></a>对象模型

以下类和接口将处理人脸 Python 客户端库的某些主要功能。

|名称|说明|
|---|---|
|[FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python) | 此类代表使用人脸服务的授权，使用所有人脸功能时都需要用到它。 请使用你的订阅信息实例化此类，然后使用它来生成其他类的实例。 |
|[FaceOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python)|此类处理可对人脸执行的基本检测和识别任务。 |
|[DetectedFace](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python)|此类代表已从图像中的单个人脸检测到的所有数据。 可以使用它来检索有关人脸的详细信息。|
|[FaceListOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.facelistoperations?view=azure-python)|此类管理云中存储的 **FaceList** 构造，这些构造存储各种不同的人脸。 |
|[PersonGroupPersonOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongrouppersonoperations?view=azure-python)| 此类管理云中存储的 **Person** 构造，这些构造存储属于单个人员的一组人脸。|
|[PersonGroupOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.persongroupoperations?view=azure-python)| 此类管理云中存储的 **PersonGroup** 构造，这些构造存储各种不同的 **Person** 对象。 |
|[ShapshotOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.snapshotoperations?view=azure-python)|此类管理快照功能；可以使用它来暂时保存所有基于云的人脸数据，并将这些数据迁移到新的 Azure 订阅。 |

## <a name="code-examples"></a>代码示例

这些代码片段演示如何使用适用于 Python 的人脸客户端库执行以下任务：

* [对客户端进行身份验证](#authenticate-the-client)
* [检测图像中的人脸](#detect-faces-in-an-image)
* [查找相似人脸](#find-similar-faces)
* [创建和训练人员组](#create-and-train-a-person-group)
* [识别人脸](#identify-a-face)
* [验证人脸](#verify-faces)
* [创建用于数据迁移的快照](#take-a-snapshot-for-data-migration)

## <a name="authenticate-the-client"></a>验证客户端

> [!NOTE]
> 本快速入门假设你已为人脸密钥[创建了名为 `FACE_SUBSCRIPTION_KEY` 的环境变量](../../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication)。

使用终结点和密钥实例化某个客户端。 使用密钥创建 [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) 对象，然后在终结点上使用该对象创建 [FaceClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.faceclient?view=azure-python) 对象。

```python
# Create an authenticated FaceClient.
face_client = FaceClient(ENDPOINT, CognitiveServicesCredentials(KEY))
```

## <a name="detect-faces-in-an-image"></a>在图像中检测人脸

以下代码检测远程图像中的人脸。 它将检测到的人脸 ID 输出到控制台，并将其存储在程序内存中。 然后，它在包含多个人员的图像中检测人脸，并将其 ID 输出到控制台。 更改 [detect_with_url](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#detect-with-url-url--return-face-id-true--return-face-landmarks-false--return-face-attributes-none--recognition-model--recognition-01---return-recognition-model-false--detection-model--detection-01---custom-headers-none--raw-false----operation-config-) 方法中的参数可以返回包含每个 [DetectedFace](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.models.detectedface?view=azure-python) 对象的不同信息。

```python
# Detect a face in an image that contains a single face
single_face_image_url = 'https://www.biography.com/.image/t_share/MTQ1MzAyNzYzOTgxNTE0NTEz/john-f-kennedy---mini-biography.jpg'
single_image_name = os.path.basename(single_face_image_url)
detected_faces = face_client.face.detect_with_url(url=single_face_image_url)
if not detected_faces:
    raise Exception('No face detected from image {}'.format(single_image_name))

# Display the detected face ID in the first single-face image.
# Face IDs are used for comparison to faces (their IDs) detected in other images.
print('Detected face ID from', single_image_name, ':')
for face in detected_faces: print (face.face_id)
print()

# Save this ID for use in Find Similar
first_image_face_ID = detected_faces[0].face_id
```

有关更多检测方案，请参阅 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py) 上的示例代码。

### <a name="display-and-frame-faces"></a>显示和定格人脸

下面的代码使用 DetectedFace.faceRectangle 属性将给定的图像输出到显示屏并在人脸周围绘制矩形。

```python
# Detect a face in an image that contains a single face
single_face_image_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
single_image_name = os.path.basename(single_face_image_url)
detected_faces = face_client.face.detect_with_url(url=single_face_image_url)
if not detected_faces:
    raise Exception('No face detected from image {}'.format(single_image_name))

# Convert width height to a point in a rectangle
def getRectangle(faceDictionary):
    rect = faceDictionary.face_rectangle
    left = rect.left
    top = rect.top
    right = left + rect.width
    bottom = top + rect.height
    
    return ((left, top), (right, bottom))


# Download the image from the url
response = requests.get(single_face_image_url)
img = Image.open(BytesIO(response.content))

# For each face returned use the face rectangle and draw a red box.
print('Drawing rectangle around face... see popup for results.')
draw = ImageDraw.Draw(img)
for face in detected_faces:
    draw.rectangle(getRectangle(face), outline='red')

# Display the image in the users default image browser.
img.show()
```

![一位年轻的妇女，其脸部周围绘制了一个红色矩形](../../images/face-rectangle-result.png)

## <a name="find-similar-faces"></a>查找相似人脸

以下代码采用检测到的单个人脸，并搜索其他一组人脸，以找到匹配项。 找到匹配项后，它会将匹配的人脸的矩形坐标输出到控制台。 

### <a name="find-matches"></a>查找匹配项

首先，运行上一部分（[检测图像中的人脸](#detect-faces-in-an-image)）所示的代码，以保存对单个人脸的引用。 然后运行以下代码，以获取对图像组中多个人脸的引用。

```python
# Detect the faces in an image that contains multiple faces
# Each detected face gets assigned a new ID
multi_face_image_url = "http://www.historyplace.com/kennedy/president-family-portrait-closeup.jpg"
multi_image_name = os.path.basename(multi_face_image_url)
detected_faces2 = face_client.face.detect_with_url(url=multi_face_image_url)
```

然后添加以下代码块，以查找该组中第一个人脸的实例。 若要了解如何修改此行为，请参阅 [find_similar](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/azure.cognitiveservices.vision.face.operations.faceoperations?view=azure-python#find-similar-face-id--face-list-id-none--large-face-list-id-none--face-ids-none--max-num-of-candidates-returned-20--mode--matchperson---custom-headers-none--raw-false----operation-config-) 方法。

```python
# Search through faces detected in group image for the single face from first image.
# First, create a list of the face IDs found in the second image.
second_image_face_IDs = list(map(lambda x: x.face_id, detected_faces2))
# Next, find similar face IDs like the one detected in the first image.
similar_faces = face_client.face.find_similar(face_id=first_image_face_ID, face_ids=second_image_face_IDs)
if not similar_faces[0]:
    print('No similar faces found in', multi_image_name, '.')
```

### <a name="print-matches"></a>输出匹配项

使用以下代码将匹配详细信息输出到控制台。

```python
# Print the details of the similar faces detected
print('Similar faces found in', multi_image_name + ':')
for face in similar_faces:
    first_image_face_ID = face.face_id
    # The similar face IDs of the single face image and the group image do not need to match, 
    # they are only used for identification purposes in each image.
    # The similar faces are matched using the Cognitive Services algorithm in find_similar().
    face_info = next(x for x in detected_faces2 if x.face_id == first_image_face_ID)
    if face_info:
        print('  Face ID: ', first_image_face_ID)
        print('  Face rectangle:')
        print('    Left: ', str(face_info.face_rectangle.left))
        print('    Top: ', str(face_info.face_rectangle.top))
        print('    Width: ', str(face_info.face_rectangle.width))
        print('    Height: ', str(face_info.face_rectangle.height))
```

## <a name="create-and-train-a-person-group"></a>创建和训练人员组

以下代码创建包含三个不同 **Person** 对象的 **PersonGroup**。 它将每个 **Person** 与一组示例图像相关联，然后进行训练以便能够识别每个人。 

### <a name="create-persongroup"></a>创建 PersonGroup

若要逐步完成此方案，需将以下图像保存到项目的根目录： https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images 。

此图像组包含三组人脸图像，这些图像对应于三个不同的人。 该代码定义三个 **Person** 对象，并将其关联到以 `woman`、`man` 和 `child` 开头的图像文件。

设置图像后，在脚本的顶部为要创建的 **PersonGroup** 对象定义一个标签。

```python
# Used in the Person Group Operations,  Snapshot Operations, and Delete Person Group examples.
# You can call list_person_groups to print a list of preexisting PersonGroups.
# SOURCE_PERSON_GROUP_ID should be all lowercase and alphanumeric. For example, 'mygroupname' (dashes are OK).
PERSON_GROUP_ID = 'my-unique-person-group'

# Used for the Snapshot and Delete Person Group examples.
TARGET_PERSON_GROUP_ID = str(uuid.uuid4()) # assign a random ID (or name it anything)
```

然后将以下代码添加到脚本的底部。 此代码创建一个 **PersonGroup** 对象和三个 **Person** 对象。

```python
'''
Create the PersonGroup
'''
# Create empty Person Group. Person Group ID must be lower case, alphanumeric, and/or with '-', '_'.
print('Person group:', PERSON_GROUP_ID)
face_client.person_group.create(person_group_id=PERSON_GROUP_ID, name=PERSON_GROUP_ID)

# Define woman friend
woman = face_client.person_group_person.create(PERSON_GROUP_ID, "Woman")
# Define man friend
man = face_client.person_group_person.create(PERSON_GROUP_ID, "Man")
# Define child friend
child = face_client.person_group_person.create(PERSON_GROUP_ID, "Child")
```

### <a name="assign-faces-to-persons"></a>将人脸添加到 Person

以下代码按图像前缀对图像排序、检测人脸，然后将人脸分配到每个 **Person** 对象。

```python
'''
Detect faces and register to correct person
'''
# Find all jpeg images of friends in working directory
woman_images = [file for file in glob.glob('*.jpg') if file.startswith("woman")]
man_images = [file for file in glob.glob('*.jpg') if file.startswith("man")]
child_images = [file for file in glob.glob('*.jpg') if file.startswith("child")]

# Add to a woman person
for image in woman_images:
    w = open(image, 'r+b')
    face_client.person_group_person.add_face_from_stream(PERSON_GROUP_ID, woman.person_id, w)

# Add to a man person
for image in man_images:
    m = open(image, 'r+b')
    face_client.person_group_person.add_face_from_stream(PERSON_GROUP_ID, man.person_id, m)

# Add to a child person
for image in child_images:
    ch = open(image, 'r+b')
    face_client.person_group_person.add_face_from_stream(PERSON_GROUP_ID, child.person_id, ch)
```

### <a name="train-persongroup"></a>训练 PersonGroup

分配人脸后，必须训练 **PersonGroup**，使其能够识别与其每个 **Person** 对象关联的视觉特征。 以下代码调用异步 **train** 方法并轮询结果，然后将状态输出到控制台。

```python
'''
Train PersonGroup
'''
print()
print('Training the person group...')
# Train the person group
face_client.person_group.train(PERSON_GROUP_ID)

while (True):
    training_status = face_client.person_group.get_training_status(PERSON_GROUP_ID)
    print("Training status: {}.".format(training_status.status))
    print()
    if (training_status.status is TrainingStatusType.succeeded):
        break
    elif (training_status.status is TrainingStatusType.failed):
        sys.exit('Training the person group has failed.')
    time.sleep(5)
```

## <a name="identify-a-face"></a>识别人脸

以下代码采用包含多个人脸的图像，并尝试在该图像中查找每个人的标识。 它将每个检测到的人脸与某个 **PersonGroup**（面部特征已知的不同 **Person** 对象的数据库）进行比较。

> [!IMPORTANT]
> 若要运行此示例，必须先运行[创建和训练人员组](#create-and-train-a-person-group)中的代码。

### <a name="get-a-test-image"></a>获取测试图像

以下代码在项目根目录中查找图像 _test-image-person-group.jpg_，并检测该图像中的人脸。 可以使用用于 **PersonGroup** 管理的图像查找此图像： https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/Face/images 。

```python
'''
Identify a face against a defined PersonGroup
'''
# Group image for testing against
group_photo = 'test-image-person-group.jpg'
IMAGES_FOLDER = os.path.join(os.path.dirname(os.path.realpath(__file__)))
# Get test image
test_image_array = glob.glob(os.path.join(IMAGES_FOLDER, group_photo))
image = open(test_image_array[0], 'r+b')

# Detect faces
face_ids = []
faces = face_client.face.detect_with_stream(image)
for face in faces:
    face_ids.append(face.face_id)
```

### <a name="identify-faces"></a>标识人脸

**identify** 方法采用检测到的人脸数组，并将其与 **PersonGroup** 进行比较。 如果检测到的某个人脸与某个人相匹配，则它会保存结果。**** 此代码将详细的匹配结果输出到控制台。

```python
# Identify faces
results = face_client.face.identify(face_ids, PERSON_GROUP_ID)
print('Identifying faces in {}'.format(os.path.basename(image.name)))
if not results:
    print('No person identified in the person group for faces from {}.'.format(os.path.basename(image.name)))
for person in results:
    print('Person for face ID {} is identified in {} with a confidence of {}.'.format(person.face_id, os.path.basename(image.name), person.candidates[0].confidence)) # Get topmost confidence score
```

## <a name="verify-faces"></a>验证人脸

验证操作采用某个人脸 ID 和其他人脸 ID 或 Person 对象，并确定它们是否属于同一个人****。

以下代码检测两个源图像中的人脸，然后针对从目标图像检测到的人脸来验证它们。

### <a name="get-test-images"></a>获取测试图像

以下代码块声明将指向验证操作的源和目标图像的变量。

```python
# Base url for the Verify and Facelist/Large Facelist operations
IMAGE_BASE_URL = 'https://csdx.blob.core.windows.net/resources/Face/Images/'
```

```python
# Create a list to hold the target photos of the same person
target_image_file_names = ['Family1-Dad1.jpg', 'Family1-Dad2.jpg']
# The source photos contain this person
source_image_file_name1 = 'Family1-Dad3.jpg'
source_image_file_name2 = 'Family1-Son1.jpg'
```

### <a name="detect-faces-for-verification"></a>检测人脸进行验证

以下代码检测源和目标图像中的人脸并将其保存到变量中。

```python
# Detect face(s) from source image 1, returns a list[DetectedFaces]
detected_faces1 = face_client.face.detect_with_url(IMAGE_BASE_URL + source_image_file_name1)
# Add the returned face's face ID
source_image1_id = detected_faces1[0].face_id
print('{} face(s) detected from image {}.'.format(len(detected_faces1), source_image_file_name1))

# Detect face(s) from source image 2, returns a list[DetectedFaces]
detected_faces2 = face_client.face.detect_with_url(IMAGE_BASE_URL + source_image_file_name2)
# Add the returned face's face ID
source_image2_id = detected_faces2[0].face_id
print('{} face(s) detected from image {}.'.format(len(detected_faces2), source_image_file_name2))

# List for the target face IDs (uuids)
detected_faces_ids = []
# Detect faces from target image url list, returns a list[DetectedFaces]
for image_file_name in target_image_file_names:
    detected_faces = face_client.face.detect_with_url(IMAGE_BASE_URL + image_file_name)
    # Add the returned face's face ID
    detected_faces_ids.append(detected_faces[0].face_id)
    print('{} face(s) detected from image {}.'.format(len(detected_faces), image_file_name))
```

### <a name="get-verification-results"></a>获取验证结果

以下代码将每个源图像与目标图像进行比较并打印出一条消息，指示它们是否属于同一个人。

```python
# Verification example for faces of the same person. The higher the confidence, the more identical the faces in the images are.
# Since target faces are the same person, in this example, we can use the 1st ID in the detected_faces_ids list to compare.
verify_result_same = face_client.face.verify_face_to_face(source_image1_id, detected_faces_ids[0])
print('Faces from {} & {} are of the same person, with confidence: {}'
    .format(source_image_file_name1, target_image_file_names[0], verify_result_same.confidence)
    if verify_result_same.is_identical
    else 'Faces from {} & {} are of a different person, with confidence: {}'
        .format(source_image_file_name1, target_image_file_names[0], verify_result_same.confidence))

# Verification example for faces of different persons.
# Since target faces are same person, in this example, we can use the 1st ID in the detected_faces_ids list to compare.
verify_result_diff = face_client.face.verify_face_to_face(source_image2_id, detected_faces_ids[0])
print('Faces from {} & {} are of the same person, with confidence: {}'
    .format(source_image_file_name2, target_image_file_names[0], verify_result_diff.confidence)
    if verify_result_diff.is_identical
    else 'Faces from {} & {} are of a different person, with confidence: {}'
        .format(source_image_file_name2, target_image_file_names[0], verify_result_diff.confidence))
```

## <a name="take-a-snapshot-for-data-migration"></a>创建用于数据迁移的快照

利用快照功能，可将已保存的人脸数据（例如训练的 **PersonGroup**）移到不同的 Azure 认知服务人脸订阅。 例如，如果你使用免费试用订阅创建了一个 **PersonGroup** 对象，现在想要将其迁移到付费订阅，则可以使用此功能。 有关快照功能的大致概述，请参阅[迁移人脸数据](../../Face-API-How-to-Topics/how-to-migrate-face-data.md)。

此示例将迁移你在[创建和训练人员组](#create-and-train-a-person-group)中创建的 **PersonGroup**。 可以先完成该部分，或者使用你自己的人脸数据构造。

### <a name="set-up-target-subscription"></a>设置目标订阅

首先，必须有另一个已包含人脸资源的 Azure 订阅；可以遵循[设置](#setting-up)部分中的步骤做好此准备。 

然后在脚本顶部附近创建以下变量。 还需要为 Azure 帐户的订阅 ID 以及新（目标）帐户的密钥、终结点和订阅 ID 创建新的环境变量。 

```python
'''
Snapshot operations variables
These are only used for the snapshot example. Set your environment variables accordingly.
'''
# Source endpoint, the location/subscription where the original person group is located.
SOURCE_ENDPOINT = ENDPOINT
# Source subscription key. Must match the source endpoint region.
SOURCE_KEY = os.environ['FACE_SUBSCRIPTION_KEY']
# Source subscription ID. Found in the Azure portal in the Overview page of your Face (or any) resource.
SOURCE_ID = os.environ['AZURE_SUBSCRIPTION_ID']
# Person group name that will get created in this quickstart's Person Group Operations example.
SOURCE_PERSON_GROUP_ID = PERSON_GROUP_ID
# Target endpoint. This is your 2nd Face subscription.
TARGET_ENDPOINT = os.environ['FACE_ENDPOINT2']
# Target subscription key. Must match the target endpoint region.
TARGET_KEY = os.environ['FACE_SUBSCRIPTION_KEY2']
# Target subscription ID. It will be the same as the source ID if created Face resources from the same 
# subscription (but moving from region to region). If they are differnt subscriptions, add the other target ID here.
TARGET_ID = os.environ['AZURE_SUBSCRIPTION_ID']
# NOTE: We do not need to specify the target PersonGroup ID here because we generate it with this example.
# Each new location you transfer a person group to will have a generated, new person group ID for that region.
```

### <a name="authenticate-target-client"></a>对目标客户端进行身份验证

稍后需要在脚本中将当前客户端对象保存为源客户端，然后对目标订阅的新客户端对象进行身份验证。 

```python
'''
Authenticate
'''
# Use your source client already created (it has the person group ID you need in it).
face_client_source = face_client
# Create a new FaceClient instance for your target with authentication.
face_client_target = FaceClient(TARGET_ENDPOINT, CognitiveServicesCredentials(TARGET_KEY))
```

### <a name="use-a-snapshot"></a>使用快照

剩余的快照操作将在异步函数中进行。 

1. 第一步是**创建**快照，以将原始订阅的人脸数据保存到临时云位置。 此方法返回用于查询操作状态的 ID。

    ```python
        '''
    Snapshot operations in 4 steps
    '''
    async def run():
        # STEP 1, take a snapshot of your person group, then track status.
        # This list must include all subscription IDs from which you want to access the snapshot.
        source_list = [SOURCE_ID, TARGET_ID]
        # You may have many sources, if transferring from many regions
        # remove any duplicates from the list. Passing the same subscription ID more than once causes
        # the Snapshot.take operation to fail.
        source_list = list(dict.fromkeys(source_list))

        # Note Snapshot.take is not asynchronous.
        # For information about Snapshot.take see:
        # https://github.com/Azure/azure-sdk-for-python/blob/master/azure-cognitiveservices-vision-face/azure/cognitiveservices/vision/face/operations/snapshot_operations.py#L36
        take_snapshot_result = face_client_source.snapshot.take(
            type=SnapshotObjectType.person_group,
            object_id=PERSON_GROUP_ID,
            apply_scope=source_list,
            # Set this to tell Snapshot.take to return the response; otherwise it returns None.
            raw=True
            )
        # Get operation ID from response for tracking
        # Snapshot.type return value is of type msrest.pipeline.ClientRawResponse. See:
        # https://docs.microsoft.com/en-us/python/api/msrest/msrest.pipeline.clientrawresponse?view=azure-python
        take_operation_id = take_snapshot_result.response.headers['Operation-Location'].replace('/operations/', '')

        print('Taking snapshot( operation ID:', take_operation_id, ')...')
    ```

1. 接下来，不断查询该 ID，直到操作完成。

    ```python
        # STEP 2, Wait for snapshot taking to complete.
    take_status = await wait_for_operation(face_client_source, take_operation_id)

    # Get snapshot id from response.
    snapshot_id = take_status.resource_location.replace ('/snapshots/', '')

    print('Snapshot ID:', snapshot_id)
    print('Taking snapshot... Done\n')
    ```

    此代码使用应单独定义的 `wait_for_operation` 函数：

    ```python
        # Helper function that waits and checks status of API call processing.
    async def wait_for_operation(client, operation_id):
        # Track progress of taking the snapshot.
        # Note Snapshot.get_operation_status is not asynchronous.
        # For information about Snapshot.get_operation_status see:
        # https://github.com/Azure/azure-sdk-for-python/blob/master/azure-cognitiveservices-vision-face/azure/cognitiveservices/vision/face/operations/snapshot_operations.py#L466
        result = client.snapshot.get_operation_status(operation_id=operation_id)

        status = result.status.lower()
        print('Operation status:', status)
        if ('notstarted' == status or 'running' == status):
            print("Waiting 10 seconds...")
            await asyncio.sleep(10)
            result = await wait_for_operation(client, operation_id)
        elif ('failed' == status):
            raise Exception("Operation failed. Reason:" + result.message)
        return result
    ```

1. 返回到异步函数。 使用 **apply** 操作将人脸数据写入目标订阅。 此方法也会返回一个 ID。

    ```python
        # STEP 3, apply the snapshot to target region(s)
    # Snapshot.apply is not asynchronous.
    # For information about Snapshot.apply see:
    # https://github.com/Azure/azure-sdk-for-python/blob/master/azure-cognitiveservices-vision-face/azure/cognitiveservices/vision/face/operations/snapshot_operations.py#L366
    apply_snapshot_result = face_client_target.snapshot.apply(
        snapshot_id=snapshot_id,
        # Generate a new UUID for the target person group ID.
        object_id=TARGET_PERSON_GROUP_ID,
        # Set this to tell Snapshot.apply to return the response; otherwise it returns None.
        raw=True
        )
    apply_operation_id = apply_snapshot_result.response.headers['Operation-Location'].replace('/operations/', '')
    print('Applying snapshot( operation ID:', apply_operation_id, ')...')
    ```

1. 再次使用 `wait_for_operation` 函数查询该 ID，直到操作完成。

    ```python
        # STEP 4, wait for applying snapshot process to complete.
    await wait_for_operation(face_client_target, apply_operation_id)
    print('Applying snapshot... Done\n')
    print('End of transfer.')
    print()
    ```

完成这些步骤后，即可从新的（目标）订阅访问你的人脸数据构造。

## <a name="run-the-application"></a>运行应用程序

在快速入门文件中使用 `python` 命令运行应用程序。

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>清理资源

如果想要清理并删除认知服务订阅，可以删除资源或资源组。 删除资源组同时也会删除与之相关联的任何其他资源。

* [Portal](/cognitive-services/cognitive-services-apis-create-account#clean-up-resources)
* [Azure CLI](/cognitive-services/cognitive-services-apis-create-account-cli#clean-up-resources)

如果你在本快速入门中创建了 **PersonGroup** 并想要删除它，请在脚本中运行以下代码：

```python
# Delete the main person group.
face_client.person_group.delete(person_group_id=PERSON_GROUP_ID)
print("Deleted the person group {} from the source location.".format(PERSON_GROUP_ID))
print()
```

如果你在本快速入门中使用快照功能迁移了数据，则还需要删除保存到目标订阅的 **PersonGroup**。

```python
# Delete the person group in the target region.
face_client_target.person_group.delete(TARGET_PERSON_GROUP_ID)
print("Deleted the person group {} from the target location.".format(TARGET_PERSON_GROUP_ID))
```

## <a name="next-steps"></a>后续步骤

本快速入门介绍了如何使用适用于 Python 的人脸库来执行基本任务。 接下来，请在参考文档中详细了解该库。

> [!div class="nextstepaction"]
> [人脸 API 参考 (Python)](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-face/?view=azure-python)

* [什么是人脸服务？](../../overview.md)
* 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/Face/FaceQuickstart.py) 上找到此示例的源代码。
