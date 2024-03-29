---
layout: docs
title: 生成 MacOS icns
cover: /assets/covers/4b5189c4-219e-4bcf-88fd-64754bbf72e7.jpeg
description: 使用 Flomi App 生成 MacOS icns 文件。
group: practice
toc: true
category: blogs
tags: [flomi]
---
本文使用 [Flomi](https://flomi.sipt.top) 生成 MacOS icns 文件，需要自行下载。

## 快速使用

准备一张 `1024 x 1024` 的图标图片。

点击下载 Pipeline 导出源文件：
<a href="https://flomi.sipt.top/docs/1.0/assets/resources/generateIconsV2.flomiext" target="_blank">Flomi generate MacOS icns extention</a> , 导入到 Flomi 可以直接使用。

Pipeline 列表右击，点击 `Import...`，选择下载的 extension 文件

![导入 Pipeline](/assets/resource/blogs/01/SCR-20230111-k2l.png)

选中导入的 Pipeline，在主工作区选择 `Sips PNG v2` 的 Stage，配置它的 Environments，点击 `Src File` 输入框后的图标，在文件浏览框里选中 之前准备的 `1024 x 1024` 的图标图片，下面要输出的图标尺寸可以任选
![配置 Sips PNG v2 Stage Environments](/assets/resource/blogs/01/SCR-20230111-k3g.png)

在主工作区选择 `Iconutil icns v2` 的 Stage，配置它的 Environments，点击 `Save to` 输入框后的图标，选择文件保存路径以及文件名

![配置 Iconutil icns v2 Environments](/assets/resource/blogs/01/SCR-20230111-k3t.png)

点击运行即可

![运行](/assets/resource/blogs/01/SCR-20230111-kjn.png)

## 自建 Pipeline Generate macOS icns

本篇 Pipeline 导出源文件：
<a href="https://flomi.sipt.top/docs/1.0/assets/resources/generateIconsV2.flomiext" target="_blank">flomi extention</a> 

在 MacOS 中，一个 APP 的图标格式为 icns，其中包含了多张不同分辨率的图标，系统会根据当前设备分辨率自动选择。生成步骤：

1. 准备一张 `1024 x 1024` 的图标图片。
2. 通过 `sips` 命令生成多张分辨率不同的图片。
3. 通过 `iconutil` 命令生成 icns。

这里会使 Flomi 来运行 Shell 脚本的方式进行生成 `icns`。

### Sips PNG Job

<img class="mb-4 img-fluid rounded-3" src="/docs/{{< param docs_version >}}/assets/img/guides/SCR-20221221-mce.png" width="1000" alt="">

这里输入基本信息后，我们对 Job 的需要 手动输入的 Enviroments 进行可视化定义，如下图:

<img class="mb-4 img-fluid rounded-3" src="/docs/{{< param docs_version >}}/assets/img/guides/SCR-20221221-np3.png" width="1000" alt="">

可以对输出的尺寸通过 checkbox 选择，输入输出也可以使用图形化进行选择，JSON 代码如下，点击 Preview (！！！记得保存！！！)：

<div style="height: 500px; overflow: scroll;">

``` JSON
{
  "viewBox": {
    "labelWidth": "20%",
    "widgets": [
      {
        "fileInput": {
          "exts": [
            "png"
          ],
          "kind": "file",
          "name": "inputFile",
          "op": "select",
          "placeholder": "1024x1024 PNG"
        },
        "label": "Src File"
      },
      {
        "albel": "size",
        "checkbox": {
          "label": "16x16",
          "name": "c16"
        }
      },
      {
        "checkbox": {
          "label": "16x16@2x",
          "name": "c16x2"
        }
      },
      {
        "checkbox": {
          "label": "32x32",
          "name": "c32"
        }
      },
      {
        "checkbox": {
          "label": "32x32@2x",
          "name": "c32x2"
        }
      },
      {
        "checkbox": {
          "label": "64x64",
          "name": "c64"
        }
      },
      {
        "checkbox": {
          "label": "64x64@2x",
          "name": "c64x2"
        }
      },
      {
        "checkbox": {
          "label": "128x128",
          "name": "c128"
        }
      },
      {
        "checkbox": {
          "label": "128x128@2x",
          "name": "c128x2"
        }
      },
      {
        "checkbox": {
          "label": "256x256",
          "name": "c256"
        }
      },
      {
        "checkbox": {
          "label": "256x256@2x",
          "name": "c256x2"
        }
      },
      {
        "checkbox": {
          "label": "512x512",
          "name": "c512"
        }
      },
      {
        "checkbox": {
          "label": "512x512@2x",
          "name": "c512x2"
        }
      },
      {
        "checkbox": {
          "extensionView": {
            "labelWidth": "0",
            "widgets": [
              {
                "fileInput": {
                  "kind": "dir",
                  "name": "outputDir",
                  "op": "select",
                  "placeholder": "Output Dir path."
                }
              }
            ]
          },
          "label": "Custom",
          "name": "customOutput"
        },
        "label": "Output Dir"
      }
    ]
  },
  "value": {
    "Custom": "false",
    "c128": "true",
    "c128x2": "true",
    "c16": "true",
    "c16x2": "true",
    "c256": "true",
    "c256x2": "true",
    "c32": "true",
    "c32x2": "true",
    "c512": "true",
    "c512x2": "true",
    "c64": "true",
    "c64x2": "true",
    "customOutput": "false",
    "inputFile": "/Users/xinyanwu/Downloads/icon.png",
    "outputDir": ""
  }
}
```

</div>

这里，我们需要进行 Job 的执行目录编写 Shell，选择 `Show in Folder...`：

<img class="mb-4 img-fluid rounded-3" src="/docs/{{< param docs_version >}}/assets/img/guides/SCR-20221221-mss.png" width="1000" alt="">

会用文件浏览器打开 Job 的执行目录，我们在下面进行 shell 编写代码如下：

<div style="height: 500px; overflow: scroll;">

```shell
#!/bin/bash

function showError() {
    echo '{"success":false, "mesage": "'$1'"}'
}

# 判断 inputFile 环境是否为空
if [ -z "$inputFile" ]; then
    showError "inputFile is empty"
    exit 0
fi

# 判断 inputFile 文件是否存在
if [ ! -f "$inputFile" ]; then
    showError "inputFile is not exist"
    exit 0
fi

destDir=$TMPDIR"flomi.iconset"

# 判断 custom 有值且为 "true"
if [ "$custom" = "true" ]; then
    if [ ! -z "$outputDir" ]; then
        destDir=$outputDir # 如果存在自定义目录立即替换
    fi
fi

# 判断 outputDir 目录是否存在
if [ -d "$destDir" ]; then
    rm -rf $destDir
fi
mkdir -p $destDir

# 生成 iconset
if [ "$c16" = "true" ]; then
    sips -z 16 16 $inputFile --out $destDir/icon_16x16.png > /dev/null
fi
if [ "$c16x2" = "true" ]; then
    sips -z 32 32 $inputFile --out $destDir/icon_16x16@2x.png > /dev/null
fi
if [ "$c32" = "true" ]; then
    sips -z 32 32 $inputFile --out $destDir/icon_32x32.png > /dev/null
fi
if [ "$c32x2" = "true" ]; then
    sips -z 64 64 $inputFile --out $destDir/icon_32x32@2x.png > /dev/null
fi
if [ "$c64" = "true" ]; then
    sips -z 64 64 $inputFile --out $destDir/icon_64x64.png > /dev/null
fi
if [ "$c64x2" = "true" ]; then
    sips -z 128 128 $inputFile --out $destDir/icon_64x64@2x.png > /dev/null
fi
if [ "$c128" = "true" ]; then
    sips -z 128 128 $inputFile --out $destDir/icon_128x128.png > /dev/null
fi
if [ "$c128x2" = "true" ]; then
    sips -z 256 256 $inputFile --out $destDir/icon_128x128@2x.png > /dev/null
fi
if [ "$c256" = "true" ]; then
    sips -z 256 256 $inputFile --out $destDir/icon_256x256.png > /dev/null
fi
if [ "$c256x2" = "true" ]; then
    sips -z 512 512 $inputFile --out $destDir/icon_256x256@2x.png > /dev/null
fi
if [ "$c512" = "true" ]; then
    sips -z 512 512 $inputFile --out $destDir/icon_512x512.png > /dev/null
fi
if [ "$c512x2" = "true" ]; then
    sips -z 1024 1024 $inputFile --out $destDir/icon_512x512@2x.png > /dev/null
fi

echo '{"success":true, "outputs": {"outputDir": "'$destDir'"}}'
```

</div>

在 Runner Command 中，输入
```bash
./sips_png.sh
```

这里可以对 Enviroments 进行变更后，按下 `CMD + R`，进行调试。

这里会把目录输出到 `outputs.outputDir` 中，后续 Job 可以获取到。

### Iconutil icns Job

1. 直接从 `Sips PNG` 上 `Duplicate` 出来，简单编辑一下
<img class="mb-4 img-fluid rounded-3" src="/docs/{{< param docs_version >}}/assets/img/guides/SCR-20221220-s6w.png" width="1000" alt="">
<img class="mb-4 img-fluid rounded-3" src="/docs/{{< param docs_version >}}/assets/img/guides/SCR-20221220-s7n.png" width="1000" alt="">

2. 编辑 Environmnents 如下：
```JSON
{
  "viewBox": {
    "labelWidth": "20%",
    "widgets": [
      {
        "fileInput": {
          "exts": [
            "icns"
          ],
          "kind": "file",
          "name": "outputFile",
          "op": "save",
          "placeholder": "*.icns"
        },
        "label": "Save to"
      }
    ]
  },
  "value": {
    "outputFile": ""
  }
}
```

Runner Command 如下：
```bash
iconutil -c icns {{.outputDir}} -o ${outputFile} && echo '{"success": true}'
```

### 创建 Pipeline

这里创建一个新的 Pipeline

<img class="mb-4 img-fluid rounded-3" src="/docs/{{< param docs_version >}}/assets/img/guides/SCR-20221221-o1n.png" width="1000" alt="">

把 `Sipns PNG v2` 和 `Iconutil icns` 拖拽进来，并串起来，选中对应 Stage 对 Environments 进行配置，即可 `CMD + R` 运行。运行成功后可在选择的文件夹中看到 icns 文件。

<img class="mb-4 img-fluid rounded-3" src="/docs/{{< param docs_version >}}/assets/img/guides/SCR-20221221-o70.png" width="1000" alt="">