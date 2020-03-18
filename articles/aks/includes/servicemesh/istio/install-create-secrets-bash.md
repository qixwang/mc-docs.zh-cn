---
author: rockboyfor
ms.topic: include
origin.date: 10/09/2019
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: fda9f995feb3fe3d2d0fbe6aa1e23c67cd37c9dd
ms.sourcegitcommit: 3c98f52b6ccca469e598d327cd537caab2fde83f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2020
ms.locfileid: "79290702"
---
### <a name="add-grafana-secret"></a>添加 Grafana 机密

请将 `REPLACE_WITH_YOUR_SECURE_PASSWORD` 令牌替换为你的密码，然后运行以下命令：

```bash
GRAFANA_USERNAME=$(echo -n "grafana" | base64)
GRAFANA_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE
EOF
```

### <a name="add-kiali-secret"></a>添加 Kiali 机密

请将 `REPLACE_WITH_YOUR_SECURE_PASSWORD` 令牌替换为你的密码，然后运行以下命令：

```bash
KIALI_USERNAME=$(echo -n "kiali" | base64)
KIALI_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE
EOF
```

<!-- Update_Description: update meta properties, wording update, update link -->