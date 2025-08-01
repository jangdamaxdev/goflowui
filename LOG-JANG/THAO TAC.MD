## NODE LIBRARY

Không thực hiện gửi API

## MODEL LIBRARY

### Refresh

Request URL: /api/experiment/models

### Load all models

Request URL: /api/experiment/models/\***\*\*\*\*\***

## WOEKFLOW (M)

Request URL: /api/userdata/workflows%2F.index.json

### Refresh

Request URL: /api/userdata

Query params:

```
Raw query string: dir=workflows&recurse=true&split=false&full_info=true
dir: workflows
recurse: true
split: false
full_info: true
```

## Toggle buttom panel

Request URL: /internal/logs/raw
Response: Status: 200 OK
HIện tất cả log, dưới dạng trả về JSON

```json
{
  "entries": [
    {
      "t": "2025-08-01T20:21:41.600904",
      "m": "Checkpoint files will always be loaded safely.\n"
    },
    {
      "t": "2025-08-01T20:21:41.636666",
      "m": "Total VRAM 16239 MB, total RAM 16239 MB\n"
    },
    {
      "t": "2025-08-01T20:21:41.636666",
      "m": "pytorch version: 2.7.1+cpu\n"
    },
    {
      "t": "2025-08-01T20:21:41.637667",
      "m": "Set vram state to: DISABLED\n"
    },
    {
      "t": "2025-08-01T20:21:41.637667",
      "m": "Device: cpu\n"
    },
    {
      "t": "2025-08-01T20:21:44.534082",
      "m": "Using sub quadratic optimization for attention, if you have memory or speed issues try using: --use-split-cross-attention\n"
    },
    {
      "t": "2025-08-01T20:22:00.635694",
      "m": "Python version: 3.12.10 (tags/v3.12.10:0cc8128, Apr  8 2025, 12:21:36) [MSC v.1943 64 bit (AMD64)]\n"
    },
    {
      "t": "2025-08-01T20:22:00.635694",
      "m": "ComfyUI version: 0.3.47\n"
    },
    {
      "t": "2025-08-01T20:22:00.669718",
      "m": "ComfyUI frontend version: 1.23.4\n"
    },
    {
      "t": "2025-08-01T20:22:00.672693",
      "m": "[Prompt Server] web root: E:\\PROJECT\\goflow\\ComfyUI\\.venv\\Lib\\site-packages\\comfyui_frontend_package\\static\n"
    },
    {
      "t": "2025-08-01T20:22:01.746418",
      "m": "Traceback (most recent call last):\n  File \"E:\\PROJECT\\goflow\\ComfyUI\\nodes.py\", line 2127, in load_custom_node\n    module_spec.loader.exec_module(module)\n  File \"<frozen importlib._bootstrap_external>\", line 995, in exec_module\n  File \"<frozen importlib._bootstrap_external>\", line 1132, in get_code\n  File \"<frozen importlib._bootstrap_external>\", line 1190, in get_data\nFileNotFoundError: [Errno 2] No such file or directory: 'E:\\\\PROJECT\\\\goflow\\\\ComfyUI\\\\custom_nodes\\\\workflow_templates\\\\__init__.py'\n\n"
    },
    {
      "t": "2025-08-01T20:22:01.747418",
      "m": "Cannot import E:\\PROJECT\\goflow\\ComfyUI\\custom_nodes\\workflow_templates module for custom nodes: [Errno 2] No such file or directory: 'E:\\\\PROJECT\\\\goflow\\\\ComfyUI\\\\custom_nodes\\\\workflow_templates\\\\__init__.py'\n"
    },
    {
      "t": "2025-08-01T20:22:01.747418",
      "m": "\nImport times for custom nodes:\n"
    },
    {
      "t": "2025-08-01T20:22:01.747418",
      "m": "   0.0 seconds: E:\\PROJECT\\goflow\\ComfyUI\\custom_nodes\\websocket_image_save.py\n"
    },
    {
      "t": "2025-08-01T20:22:01.747418",
      "m": "   0.0 seconds (IMPORT FAILED): E:\\PROJECT\\goflow\\ComfyUI\\custom_nodes\\workflow_templates\n"
    },
    {
      "t": "2025-08-01T20:22:01.747418",
      "m": "\n"
    },
    {
      "t": "2025-08-01T20:22:02.274957",
      "m": "Context impl SQLiteImpl.\n"
    },
    {
      "t": "2025-08-01T20:22:02.275963",
      "m": "Will assume non-transactional DDL.\n"
    },
    {
      "t": "2025-08-01T20:22:02.276964",
      "m": "No target revision found.\n"
    },
    {
      "t": "2025-08-01T20:22:02.286985",
      "m": "Starting server\n\n"
    },
    {
      "t": "2025-08-01T20:22:02.287959",
      "m": "To see the GUI go to: http://127.0.0.1:8188\n"
    },
    {
      "t": "2025-08-01T21:54:58.150094",
      "m": "WARNING: request with non matching host and origin localhost:8188 != localhost:5173, returning 403\n"
    },
    {
      "t": "2025-08-01T22:08:54.526074",
      "m": "got prompt\n"
    },
    {
      "t": "2025-08-01T22:08:54.527082",
      "m": "Failed to validate prompt for output 13:\n"
    },
    {
      "t": "2025-08-01T22:08:54.527082",
      "m": "* (prompt):\n"
    },
    {
      "t": "2025-08-01T22:08:54.527082",
      "m": "  - Required input is missing: images\n"
    },
    {
      "t": "2025-08-01T22:08:54.528068",
      "m": "* SaveImageWebsocket 13:\n"
    },
    {
      "t": "2025-08-01T22:08:54.528068",
      "m": "  - Required input is missing: images\n"
    },
    {
      "t": "2025-08-01T22:08:54.528068",
      "m": "Output will be ignored\n"
    },
    {
      "t": "2025-08-01T22:08:54.528068",
      "m": "invalid prompt: {'type': 'prompt_outputs_failed_validation', 'message': 'Prompt outputs failed validation', 'details': 'Required input is missing: images', 'extra_info': {}}\n"
    }
  ],
  "size": {
    "cols": 165,
    "rows": 12
  }
}
```

## Menu chính

### Workflow

#### Browser templates

    Request URL: /templates/audio_ace_step_1_t2a_instrumentals-1.mp3
    Status: 206  Partial Content
    Content-type: audio/mpeg

    Request URL: /templates/api_kling_flf-1.webp
    Status: 200  OK
    Content-type: application/octet-stream

Khi chọn 1 template:
Request URL: /templates/image2image.json
Status: 200 OK
Content-type: application/json

```json
{
  "id": "7cb6261d-3b03-4171-bbd1-a4b256b42404",
  "revision": 0,
  "last_node_id": 19,
  "last_link_id": 19,
  "nodes": [
    {
      "id": 14,
      "type": "CheckpointLoaderSimple",
      "pos": [-90, 200],
      "size": [315, 98],
      "flags": {},
      "order": 0,
      "mode": 0,
      "inputs": [],
      "outputs": [
        {
          "name": "MODEL",
          "type": "MODEL",
          "slot_index": 0,
          "links": [13]
        },
        {
          "name": "CLIP",
          "type": "CLIP",
          "slot_index": 1,
          "links": [14, 15]
        },
        {
          "name": "VAE",
          "type": "VAE",
          "slot_index": 2,
          "links": [16, 17]
        }
      ],
      "properties": {
        "Node name for S&R": "CheckpointLoaderSimple",
        "cnr_id": "comfy-core",
        "ver": "0.3.44",
        "models": [
          {
            "name": "v1-5-pruned-emaonly-fp16.safetensors",
            "url": "https://huggingface.co/Comfy-Org/stable-diffusion-v1-5-archive/resolve/main/v1-5-pruned-emaonly-fp16.safetensors?download=true",
            "directory": "checkpoints"
          }
        ]
      },
      "widgets_values": ["v1-5-pruned-emaonly-fp16.safetensors"],
      "color": "#322",
      "bgcolor": "#533"
    },
    {
      "id": 6,
      "type": "CLIPTextEncode",
      "pos": [280, 210],
      "size": [422.84503173828125, 164.31304931640625],
      "flags": {},
      "order": 5,
      "mode": 0,
      "inputs": [
        {
          "name": "clip",
          "type": "CLIP",
          "link": 14
        }
      ],
      "outputs": [
        {
          "name": "CONDITIONING",
          "type": "CONDITIONING",
          "slot_index": 0,
          "links": [4]
        }
      ],
      "properties": {
        "Node name for S&R": "CLIPTextEncode",
        "cnr_id": "comfy-core",
        "ver": "0.3.44"
      },
      "widgets_values": [
        "photograph of victorian woman with wings, sky clouds, meadow grass\n"
      ],
      "color": "#232",
      "bgcolor": "#353"
    },
    {
      "id": 7,
      "type": "CLIPTextEncode",
      "pos": [280, 410],
      "size": [425.27801513671875, 180.6060791015625],
      "flags": {},
      "order": 6,
      "mode": 0,
      "inputs": [
        {
          "name": "clip",
          "type": "CLIP",
          "link": 15
        }
      ],
      "outputs": [
        {
          "name": "CONDITIONING",
          "type": "CONDITIONING",
          "slot_index": 0,
          "links": [6]
        }
      ],
      "properties": {
        "Node name for S&R": "CLIPTextEncode",
        "cnr_id": "comfy-core",
        "ver": "0.3.44"
      },
      "widgets_values": ["watermark, text\n"],
      "color": "#223",
      "bgcolor": "#335"
    },
    {
      "id": 16,
      "type": "MarkdownNote",
      "pos": [-440, 160],
      "size": [314.95745849609375, 133.0336456298828],
      "flags": {},
      "order": 1,
      "mode": 0,
      "inputs": [],
      "outputs": [],
      "properties": {},
      "widgets_values": [
        "[Tutorials](https://docs.comfy.org/tutorials/basic/image-to-image)|[教程](https://docs.comfy.org/zh-CN/tutorials/basic/image-to-image)\n\nDownload  [v1-5-pruned-emaonly-fp16.safetensors](https://huggingface.co/Comfy-Org/stable-diffusion-v1-5-archive/resolve/main/v1-5-pruned-emaonly-fp16.safetensors?download=true) and save it under  **ComfyUI/models/checkpoints** "
      ],
      "color": "#432",
      "bgcolor": "#653"
    },
    {
      "id": 3,
      "type": "KSampler",
      "pos": [740, 180],
      "size": [315, 262],
      "flags": {},
      "order": 9,
      "mode": 0,
      "inputs": [
        {
          "name": "model",
          "type": "MODEL",
          "link": 13
        },
        {
          "name": "positive",
          "type": "CONDITIONING",
          "link": 4
        },
        {
          "name": "negative",
          "type": "CONDITIONING",
          "link": 6
        },
        {
          "name": "latent_image",
          "type": "LATENT",
          "link": 11
        }
      ],
      "outputs": [
        {
          "name": "LATENT",
          "type": "LATENT",
          "slot_index": 0,
          "links": [7]
        }
      ],
      "properties": {
        "Node name for S&R": "KSampler",
        "cnr_id": "comfy-core",
        "ver": "0.3.44"
      },
      "widgets_values": [
        280823642470253,
        "randomize",
        20,
        8,
        "dpmpp_2m",
        "normal",
        0.8700000000000001
      ]
    },
    {
      "id": 8,
      "type": "VAEDecode",
      "pos": [1080, 180],
      "size": [210, 46],
      "flags": {},
      "order": 10,
      "mode": 0,
      "inputs": [
        {
          "name": "samples",
          "type": "LATENT",
          "link": 7
        },
        {
          "name": "vae",
          "type": "VAE",
          "link": 17
        }
      ],
      "outputs": [
        {
          "name": "IMAGE",
          "type": "IMAGE",
          "slot_index": 0,
          "links": [9]
        }
      ],
      "properties": {
        "Node name for S&R": "VAEDecode",
        "cnr_id": "comfy-core",
        "ver": "0.3.44"
      },
      "widgets_values": []
    },
    {
      "id": 9,
      "type": "SaveImage",
      "pos": [1080, 280],
      "size": [450, 430],
      "flags": {},
      "order": 11,
      "mode": 0,
      "inputs": [
        {
          "name": "images",
          "type": "IMAGE",
          "link": 9
        }
      ],
      "outputs": [],
      "properties": {
        "cnr_id": "comfy-core",
        "ver": "0.3.44"
      },
      "widgets_values": ["ComfyUI"]
    },
    {
      "id": 10,
      "type": "LoadImage",
      "pos": [-90, 410],
      "size": [310, 314],
      "flags": {},
      "order": 2,
      "mode": 0,
      "inputs": [],
      "outputs": [
        {
          "name": "IMAGE",
          "type": "IMAGE",
          "slot_index": 0,
          "links": [18]
        },
        {
          "name": "MASK",
          "type": "MASK",
          "links": null
        }
      ],
      "properties": {
        "Node name for S&R": "LoadImage",
        "cnr_id": "comfy-core",
        "ver": "0.3.44"
      },
      "widgets_values": ["example.png", "image"],
      "color": "#322",
      "bgcolor": "#533"
    },
    {
      "id": 12,
      "type": "VAEEncode",
      "pos": [560, 650],
      "size": [210, 46],
      "flags": {},
      "order": 8,
      "mode": 0,
      "inputs": [
        {
          "name": "pixels",
          "type": "IMAGE",
          "link": 19
        },
        {
          "name": "vae",
          "type": "VAE",
          "link": 16
        }
      ],
      "outputs": [
        {
          "name": "LATENT",
          "type": "LATENT",
          "slot_index": 0,
          "links": [11]
        }
      ],
      "properties": {
        "Node name for S&R": "VAEEncode",
        "cnr_id": "comfy-core",
        "ver": "0.3.44"
      },
      "widgets_values": []
    },
    {
      "id": 17,
      "type": "MarkdownNote",
      "pos": [740, 490],
      "size": [310, 110],
      "flags": {},
      "order": 3,
      "mode": 0,
      "inputs": [],
      "outputs": [],
      "properties": {},
      "widgets_values": [
        "When using the image - to - image workflow, you should remember that the **denoise** value should be less than 1. The closer the value is to 0, the more features of the input image the output image will retain."
      ],
      "color": "#432",
      "bgcolor": "#653"
    },
    {
      "id": 18,
      "type": "ImageScaleToTotalPixels",
      "pos": [280, 650],
      "size": [260, 82],
      "flags": {},
      "order": 7,
      "mode": 0,
      "inputs": [
        {
          "name": "image",
          "type": "IMAGE",
          "link": 18
        }
      ],
      "outputs": [
        {
          "name": "IMAGE",
          "type": "IMAGE",
          "links": [19]
        }
      ],
      "properties": {
        "Node name for S&R": "ImageScaleToTotalPixels"
      },
      "widgets_values": ["nearest-exact", 0.25]
    },
    {
      "id": 19,
      "type": "MarkdownNote",
      "pos": [280, 780],
      "size": [270, 120],
      "flags": {},
      "order": 4,
      "mode": 0,
      "inputs": [],
      "outputs": [],
      "title": "About Scale Image to Total Pixels",
      "properties": {},
      "widgets_values": [
        "The model this template uses is trained based on a 512*512 image dataset. So if you use an input image that's too large, it might cause some issues. We've added the Scale image to total pixels node to scale images. If you're quite familiar with this model, you can remove it."
      ],
      "color": "#432",
      "bgcolor": "#653"
    }
  ],
  "links": [
    [4, 6, 0, 3, 1, "CONDITIONING"],
    [6, 7, 0, 3, 2, "CONDITIONING"],
    [7, 3, 0, 8, 0, "LATENT"],
    [9, 8, 0, 9, 0, "IMAGE"],
    [11, 12, 0, 3, 3, "LATENT"],
    [13, 14, 0, 3, 0, "MODEL"],
    [14, 14, 1, 6, 0, "CLIP"],
    [15, 14, 1, 7, 0, "CLIP"],
    [16, 14, 2, 12, 1, "VAE"],
    [17, 14, 2, 8, 1, "VAE"],
    [18, 10, 0, 18, 0, "IMAGE"],
    [19, 18, 0, 12, 0, "IMAGE"]
  ],
  "groups": [
    {
      "id": 1,
      "title": "Step2 - Upload image",
      "bounding": [-100, 330, 340, 420],
      "color": "#3f789e",
      "font_size": 24,
      "flags": {}
    },
    {
      "id": 2,
      "title": "Step1 - Load model",
      "bounding": [-100, 130, 340, 180],
      "color": "#3f789e",
      "font_size": 24,
      "flags": {}
    },
    {
      "id": 3,
      "title": "Step3 - Prompt",
      "bounding": [270, 130, 450, 470],
      "color": "#3f789e",
      "font_size": 24,
      "flags": {}
    }
  ],
  "config": {},
  "extra": {
    "ds": {
      "scale": 0.925107135909973,
      "offset": [590.5116922244889, -18.70663404599139]
    },
    "frontendVersion": "1.24.1"
  },
  "version": 0.4
}
```

#### Save workflow

thực hiện đặt tên workflow là `2`
Request URL: /api/userdata/workflows%2F2.json
Method: POST

```
Raw query string: overwrite=false&full_info=true
overwrite: false
full_info: true
```

Xem file `request body.json`

Status: 200 OK
Xem file `response body.json`

Lúc này khi reload lại trang GUI, mục `### 6. Request URL: /api/userdata.` đã có thêm dữ liệu:

```json
[
  {
    "path": "2.json",
    "size": 4589,
    "modified": 1754062278.7850394
  }
]
```

Đồng thời gọi lại lệnh `Request URL: /api/userdata/workflows%2F2.json` nhưng với body rỗng, được `Status: 304  Not Modified`

#### Export

Xuất ra file Json giống như file `request body.json` (là 1)

#### Export (API)

Xuất file cấu trúc gọn hơn, khả năng load lại như nhau

### Theme

Request URL: /api/settings/Comfy.ColorPalette
với body `"dark"` hoặc `"light"`
Status: 200 OK
