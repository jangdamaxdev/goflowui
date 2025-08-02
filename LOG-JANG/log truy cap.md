  Log request `--cpu`
## Cấu hình chạy Vue
### npm run dev (mặc định) và SETUP .ENV
Để `DEV_SERVER_COMFYUI_URL=http://localhost:8188`
Sau khi load vào địa chỉ do Vite đưa ra là: `http://localhost:5173`
Phía Core Backend có log CLI:
`Feature flags negotiated for client 616993e5479941a5a4a6fa59f5af68a8: {'supports_preview_metadata': True}`
Phía CLient instance của class ComfyApi là `api` có giá trị:
```json
{
  "api_host": "localhost:5173",
  "api_base": "",
  "initialClientId": "616993e5479941a5a4a6fa59f5af68a8",
  "user": "Jang",
  "socket": null,
  "reportedUnknownMessageTypes": {},
  "serverFeatureFlags": {}
}
```
Dù thay đổi `DEV_SERVER_COMFYUI_URL` sang `Mockoon port 9999` nhưng luôn là `"api_host": "localhost:5173"`
Nên Các lệnh gọi `fetchApi` đều dùng `this.api_host = location.host`. 
Trong khi location có giá trị như sau:
```json
{
  "ancestorOrigins": {},
  "href": "http://localhost:5173/",
  "origin": "http://localhost:5173",
  "protocol": "http:",
  "host": "localhost:5173",
  "hostname": "localhost",
  "port": "5173",
  "pathname": "/",
  "search": "",
  "hash": ""
}
```
## Quy trình load ban đầu
### 1. Request URL: /api/userdata/user.css. Response 404.

### 2. Request URL: /api/users. Response 200:

```json
{
  "storage": "server",
  "migrated": true
}
```

### 3. Request URL: /api/i18n. Response 200: {}

### 4. Request URL: /api/system_stats. Response 200:

```json
{
  "system": {
    "os": "posix",
    "ram_total": 6064128000,
    "ram_free": 3338358784,
    "comfyui_version": "0.3.47",
    "required_frontend_version": "1.23.4",
    "python_version": "3.12.3 (main, Jun 18 2025, 17:59:45) [GCC 13.3.0]",
    "pytorch_version": "2.7.1+cu126",
    "embedded_python": false,
    "argv": ["main.py", "--cpu"]
  },
  "devices": [
    {
      "name": "cpu",
      "type": "cpu",
      "index": null,
      "vram_total": 6064128000,
      "vram_free": 3338358784,
      "torch_vram_total": 6064128000,
      "torch_vram_free": 3338358784
    }
  ]
}
```

### 5. Request URL: /api/settings. Response 200:

```json
{
  "Comfy.InstalledVersion": "1.25.2",
  "Comfy.TutorialCompleted": true,
  "Comfy.Release.Version": "0.3.47",
  "Comfy.Release.Status": "what's new seen",
  "Comfy.Release.Timestamp": 1754015614585
}
```

### 6. Request URL: /api/userdata.

Query params:

```
Raw query string: dir=workflows&recurse=true&split=false&full_info=true
dir: workflows
recurse: true
split: false
full_info: true
```

Response 200: []

### 7.Request URL: /api/extensions. Status: 200 OK:

```json
[
  "/extensions/core/groupNode.js",
  "/extensions/core/clipspace.js",
  "/extensions/core/widgetInputs.js",
  "/extensions/core/groupNodeManage.js",
  "/extensions/core/maskEditorOld.js",
  "/extensions/core/load3d/ModelExporter.js",
  "/extensions/core/load3d/EventManager.js",
  "/extensions/core/load3d/RecordingManager.js",
  "/extensions/core/load3d/ControlsManager.js",
  "/extensions/core/load3d/LoaderManager.js",
  "/extensions/core/load3d/NodeStorage.js",
  "/extensions/core/load3d/CameraManager.js",
  "/extensions/core/load3d/PreviewManager.js",
  "/extensions/core/load3d/AnimationManager.js",
  "/extensions/core/load3d/ModelManager.js",
  "/extensions/core/load3d/ViewHelperManager.js",
  "/extensions/core/load3d/LightingManager.js",
  "/extensions/core/load3d/SceneManager.js"
]
```

### 8. Request URL: /api/object_info. Status: 200 OK

```json
{
  "KSampler": {
    "input": {
      "required": {
        "model": [
          "MODEL",
          {
            "tooltip": "The model used for denoising the input latent."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ],
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000,
            "tooltip": "The number of steps used in the denoising process."
          }
        ],
        "cfg": [
          "FLOAT",
          {
            "default": 8,
            "min": 0,
            "max": 100,
            "step": 0.1,
            "round": 0.01,
            "tooltip": "The Classifier-Free Guidance scale balances creativity and adherence to the prompt. Higher values result in images more closely matching the prompt however too high values will negatively impact quality."
          }
        ],
        "sampler_name": [
          [
            "euler",
            "euler_cfg_pp",
            "euler_ancestral",
            "euler_ancestral_cfg_pp",
            "heun",
            "heunpp2",
            "dpm_2",
            "dpm_2_ancestral",
            "lms",
            "dpm_fast",
            "dpm_adaptive",
            "dpmpp_2s_ancestral",
            "dpmpp_2s_ancestral_cfg_pp",
            "dpmpp_sde",
            "dpmpp_sde_gpu",
            "dpmpp_2m",
            "dpmpp_2m_cfg_pp",
            "dpmpp_2m_sde",
            "dpmpp_2m_sde_gpu",
            "dpmpp_3m_sde",
            "dpmpp_3m_sde_gpu",
            "ddpm",
            "lcm",
            "ipndm",
            "ipndm_v",
            "deis",
            "res_multistep",
            "res_multistep_cfg_pp",
            "res_multistep_ancestral",
            "res_multistep_ancestral_cfg_pp",
            "gradient_estimation",
            "gradient_estimation_cfg_pp",
            "er_sde",
            "seeds_2",
            "seeds_3",
            "sa_solver",
            "sa_solver_pece",
            "ddim",
            "uni_pc",
            "uni_pc_bh2"
          ],
          {
            "tooltip": "The algorithm used when sampling, this can affect the quality, speed, and style of the generated output."
          }
        ],
        "scheduler": [
          [
            "simple",
            "sgm_uniform",
            "karras",
            "exponential",
            "ddim_uniform",
            "beta",
            "normal",
            "linear_quadratic",
            "kl_optimal"
          ],
          {
            "tooltip": "The scheduler controls how noise is gradually removed to form the image."
          }
        ],
        "positive": [
          "CONDITIONING",
          {
            "tooltip": "The conditioning describing the attributes you want to include in the image."
          }
        ],
        "negative": [
          "CONDITIONING",
          {
            "tooltip": "The conditioning describing the attributes you want to exclude from the image."
          }
        ],
        "latent_image": [
          "LATENT",
          {
            "tooltip": "The latent image to denoise."
          }
        ],
        "denoise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "tooltip": "The amount of denoising applied, lower values will maintain the structure of the initial image allowing for image to image sampling."
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "seed",
        "steps",
        "cfg",
        "sampler_name",
        "scheduler",
        "positive",
        "negative",
        "latent_image",
        "denoise"
      ]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "KSampler",
    "display_name": "KSampler",
    "description": "Uses the provided model, positive and negative conditioning to denoise the latent image.",
    "python_module": "nodes",
    "category": "sampling",
    "output_node": false,
    "output_tooltips": ["The denoised latent."]
  },
  "CheckpointLoaderSimple": {
    "input": {
      "required": {
        "ckpt_name": [
          [],
          {
            "tooltip": "The name of the checkpoint (model) to load."
          }
        ]
      }
    },
    "input_order": {
      "required": ["ckpt_name"]
    },
    "output": ["MODEL", "CLIP", "VAE"],
    "output_is_list": [false, false, false],
    "output_name": ["MODEL", "CLIP", "VAE"],
    "name": "CheckpointLoaderSimple",
    "display_name": "Load Checkpoint",
    "description": "Loads a diffusion model checkpoint, diffusion models are used to denoise latents.",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false,
    "output_tooltips": [
      "The model used for denoising latents.",
      "The CLIP model used for encoding text prompts.",
      "The VAE model used for encoding and decoding images to and from latent space."
    ]
  },
  "CLIPTextEncode": {
    "input": {
      "required": {
        "text": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true,
            "tooltip": "The text to be encoded."
          }
        ],
        "clip": [
          "CLIP",
          {
            "tooltip": "The CLIP model used for encoding the text."
          }
        ]
      }
    },
    "input_order": {
      "required": ["text", "clip"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncode",
    "display_name": "CLIP Text Encode (Prompt)",
    "description": "Encodes a text prompt using a CLIP model into an embedding that can be used to guide the diffusion model towards generating specific images.",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false,
    "output_tooltips": [
      "A conditioning containing the embedded text used to guide the diffusion model."
    ]
  },
  "CLIPSetLastLayer": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "stop_at_clip_layer": [
          "INT",
          {
            "default": -1,
            "min": -24,
            "max": -1,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "stop_at_clip_layer"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "CLIPSetLastLayer",
    "display_name": "CLIP Set Last Layer",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "VAEDecode": {
    "input": {
      "required": {
        "samples": [
          "LATENT",
          {
            "tooltip": "The latent to be decoded."
          }
        ],
        "vae": [
          "VAE",
          {
            "tooltip": "The VAE model used for decoding the latent."
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "vae"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "VAEDecode",
    "display_name": "VAE Decode",
    "description": "Decodes latent images back into pixel space images.",
    "python_module": "nodes",
    "category": "latent",
    "output_node": false,
    "output_tooltips": ["The decoded image."]
  },
  "VAEEncode": {
    "input": {
      "required": {
        "pixels": ["IMAGE"],
        "vae": ["VAE"]
      }
    },
    "input_order": {
      "required": ["pixels", "vae"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "VAEEncode",
    "display_name": "VAE Encode",
    "description": "",
    "python_module": "nodes",
    "category": "latent",
    "output_node": false
  },
  "VAEEncodeForInpaint": {
    "input": {
      "required": {
        "pixels": ["IMAGE"],
        "vae": ["VAE"],
        "mask": ["MASK"],
        "grow_mask_by": [
          "INT",
          {
            "default": 6,
            "min": 0,
            "max": 64,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["pixels", "vae", "mask", "grow_mask_by"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "VAEEncodeForInpaint",
    "display_name": "VAE Encode (for Inpainting)",
    "description": "",
    "python_module": "nodes",
    "category": "latent/inpaint",
    "output_node": false
  },
  "VAELoader": {
    "input": {
      "required": {
        "vae_name": [[]]
      }
    },
    "input_order": {
      "required": ["vae_name"]
    },
    "output": ["VAE"],
    "output_is_list": [false],
    "output_name": ["VAE"],
    "name": "VAELoader",
    "display_name": "Load VAE",
    "description": "",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false
  },
  "EmptyLatentImage": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 512,
            "min": 16,
            "max": 16384,
            "step": 8,
            "tooltip": "The width of the latent images in pixels."
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 16,
            "max": 16384,
            "step": 8,
            "tooltip": "The height of the latent images in pixels."
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096,
            "tooltip": "The number of latent images in the batch."
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyLatentImage",
    "display_name": "Empty Latent Image",
    "description": "Create a new batch of empty latent images to be denoised via sampling.",
    "python_module": "nodes",
    "category": "latent",
    "output_node": false,
    "output_tooltips": ["The empty latent image batch."]
  },
  "LatentUpscale": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "upscale_method": [
          ["nearest-exact", "bilinear", "area", "bicubic", "bislerp"]
        ],
        "width": [
          "INT",
          {
            "default": 512,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "crop": [["disabled", "center"]]
      }
    },
    "input_order": {
      "required": ["samples", "upscale_method", "width", "height", "crop"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentUpscale",
    "display_name": "Upscale Latent",
    "description": "",
    "python_module": "nodes",
    "category": "latent",
    "output_node": false
  },
  "LatentUpscaleBy": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "upscale_method": [
          ["nearest-exact", "bilinear", "area", "bicubic", "bislerp"]
        ],
        "scale_by": [
          "FLOAT",
          {
            "default": 1.5,
            "min": 0.01,
            "max": 8,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "upscale_method", "scale_by"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentUpscaleBy",
    "display_name": "Upscale Latent By",
    "description": "",
    "python_module": "nodes",
    "category": "latent",
    "output_node": false
  },
  "LatentFromBatch": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "batch_index": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 63
          }
        ],
        "length": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 64
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "batch_index", "length"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentFromBatch",
    "display_name": "Latent From Batch",
    "description": "",
    "python_module": "nodes",
    "category": "latent/batch",
    "output_node": false
  },
  "RepeatLatentBatch": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "amount": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 64
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "amount"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "RepeatLatentBatch",
    "display_name": "Repeat Latent Batch",
    "description": "",
    "python_module": "nodes",
    "category": "latent/batch",
    "output_node": false
  },
  "SaveImage": {
    "input": {
      "required": {
        "images": [
          "IMAGE",
          {
            "tooltip": "The images to save."
          }
        ],
        "filename_prefix": [
          "STRING",
          {
            "default": "ComfyUI",
            "tooltip": "The prefix for the file to save. This may include formatting information such as %date:yyyy-MM-dd% or %Empty Latent Image.width% to include values from nodes."
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["images", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveImage",
    "display_name": "Save Image",
    "description": "Saves the input images to your ComfyUI output directory.",
    "python_module": "nodes",
    "category": "image",
    "output_node": true
  },
  "PreviewImage": {
    "input": {
      "required": {
        "images": ["IMAGE"]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["images"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "PreviewImage",
    "display_name": "Preview Image",
    "description": "Saves the input images to your ComfyUI output directory.",
    "python_module": "nodes",
    "category": "image",
    "output_node": true
  },
  "LoadImage": {
    "input": {
      "required": {
        "image": [
          ["example.png"],
          {
            "image_upload": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["image"]
    },
    "output": ["IMAGE", "MASK"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "MASK"],
    "name": "LoadImage",
    "display_name": "Load Image",
    "description": "",
    "python_module": "nodes",
    "category": "image",
    "output_node": false
  },
  "LoadImageMask": {
    "input": {
      "required": {
        "image": [
          ["example.png"],
          {
            "image_upload": true
          }
        ],
        "channel": [["alpha", "red", "green", "blue"]]
      }
    },
    "input_order": {
      "required": ["image", "channel"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "LoadImageMask",
    "display_name": "Load Image (as Mask)",
    "description": "",
    "python_module": "nodes",
    "category": "mask",
    "output_node": false
  },
  "LoadImageOutput": {
    "input": {
      "required": {
        "image": [
          "COMBO",
          {
            "image_upload": true,
            "image_folder": "output",
            "remote": {
              "route": "/internal/files/output",
              "refresh_button": true,
              "control_after_refresh": "first"
            }
          }
        ]
      }
    },
    "input_order": {
      "required": ["image"]
    },
    "output": ["IMAGE", "MASK"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "MASK"],
    "name": "LoadImageOutput",
    "display_name": "Load Image (from Outputs)",
    "description": "Load an image from the output folder. When the refresh button is clicked, the node will update the image list and automatically select the first image, allowing for easy iteration.",
    "python_module": "nodes",
    "category": "image",
    "output_node": false,
    "experimental": true
  },
  "ImageScale": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "upscale_method": [
          ["nearest-exact", "bilinear", "area", "bicubic", "lanczos"]
        ],
        "width": [
          "INT",
          {
            "default": 512,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "crop": [["disabled", "center"]]
      }
    },
    "input_order": {
      "required": ["image", "upscale_method", "width", "height", "crop"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageScale",
    "display_name": "Upscale Image",
    "description": "",
    "python_module": "nodes",
    "category": "image/upscaling",
    "output_node": false
  },
  "ImageScaleBy": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "upscale_method": [
          ["nearest-exact", "bilinear", "area", "bicubic", "lanczos"]
        ],
        "scale_by": [
          "FLOAT",
          {
            "default": 1,
            "min": 0.01,
            "max": 8,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "upscale_method", "scale_by"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageScaleBy",
    "display_name": "Upscale Image By",
    "description": "",
    "python_module": "nodes",
    "category": "image/upscaling",
    "output_node": false
  },
  "ImageInvert": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["image"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageInvert",
    "display_name": "Invert Image",
    "description": "",
    "python_module": "nodes",
    "category": "image",
    "output_node": false
  },
  "ImageBatch": {
    "input": {
      "required": {
        "image1": ["IMAGE"],
        "image2": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["image1", "image2"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageBatch",
    "display_name": "Batch Images",
    "description": "",
    "python_module": "nodes",
    "category": "image",
    "output_node": false
  },
  "ImagePadForOutpaint": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "left": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "top": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "right": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "bottom": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "feathering": [
          "INT",
          {
            "default": 40,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "left", "top", "right", "bottom", "feathering"]
    },
    "output": ["IMAGE", "MASK"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "MASK"],
    "name": "ImagePadForOutpaint",
    "display_name": "Pad Image for Outpainting",
    "description": "",
    "python_module": "nodes",
    "category": "image",
    "output_node": false
  },
  "EmptyImage": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ],
        "color": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16777215,
            "step": 1,
            "display": "color"
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "batch_size", "color"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "EmptyImage",
    "display_name": "EmptyImage",
    "description": "",
    "python_module": "nodes",
    "category": "image",
    "output_node": false
  },
  "ConditioningAverage": {
    "input": {
      "required": {
        "conditioning_to": ["CONDITIONING"],
        "conditioning_from": ["CONDITIONING"],
        "conditioning_to_strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "conditioning_to",
        "conditioning_from",
        "conditioning_to_strength"
      ]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningAverage",
    "display_name": "ConditioningAverage",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "ConditioningCombine": {
    "input": {
      "required": {
        "conditioning_1": ["CONDITIONING"],
        "conditioning_2": ["CONDITIONING"]
      }
    },
    "input_order": {
      "required": ["conditioning_1", "conditioning_2"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningCombine",
    "display_name": "Conditioning (Combine)",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "ConditioningConcat": {
    "input": {
      "required": {
        "conditioning_to": ["CONDITIONING"],
        "conditioning_from": ["CONDITIONING"]
      }
    },
    "input_order": {
      "required": ["conditioning_to", "conditioning_from"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningConcat",
    "display_name": "Conditioning (Concat)",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "ConditioningSetArea": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "width": [
          "INT",
          {
            "default": 64,
            "min": 64,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 64,
            "min": 64,
            "max": 16384,
            "step": 8
          }
        ],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["conditioning", "width", "height", "x", "y", "strength"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetArea",
    "display_name": "Conditioning (Set Area)",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "ConditioningSetAreaPercentage": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "width": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "height": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "y": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["conditioning", "width", "height", "x", "y", "strength"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetAreaPercentage",
    "display_name": "Conditioning (Set Area with Percentage)",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "ConditioningSetAreaStrength": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["conditioning", "strength"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetAreaStrength",
    "display_name": "ConditioningSetAreaStrength",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "ConditioningSetMask": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "mask": ["MASK"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "set_cond_area": [["default", "mask bounds"]]
      }
    },
    "input_order": {
      "required": ["conditioning", "mask", "strength", "set_cond_area"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetMask",
    "display_name": "Conditioning (Set Mask)",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "KSamplerAdvanced": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "add_noise": [["enable", "disable"]],
        "noise_seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true
          }
        ],
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "cfg": [
          "FLOAT",
          {
            "default": 8,
            "min": 0,
            "max": 100,
            "step": 0.1,
            "round": 0.01
          }
        ],
        "sampler_name": [
          [
            "euler",
            "euler_cfg_pp",
            "euler_ancestral",
            "euler_ancestral_cfg_pp",
            "heun",
            "heunpp2",
            "dpm_2",
            "dpm_2_ancestral",
            "lms",
            "dpm_fast",
            "dpm_adaptive",
            "dpmpp_2s_ancestral",
            "dpmpp_2s_ancestral_cfg_pp",
            "dpmpp_sde",
            "dpmpp_sde_gpu",
            "dpmpp_2m",
            "dpmpp_2m_cfg_pp",
            "dpmpp_2m_sde",
            "dpmpp_2m_sde_gpu",
            "dpmpp_3m_sde",
            "dpmpp_3m_sde_gpu",
            "ddpm",
            "lcm",
            "ipndm",
            "ipndm_v",
            "deis",
            "res_multistep",
            "res_multistep_cfg_pp",
            "res_multistep_ancestral",
            "res_multistep_ancestral_cfg_pp",
            "gradient_estimation",
            "gradient_estimation_cfg_pp",
            "er_sde",
            "seeds_2",
            "seeds_3",
            "sa_solver",
            "sa_solver_pece",
            "ddim",
            "uni_pc",
            "uni_pc_bh2"
          ]
        ],
        "scheduler": [
          [
            "simple",
            "sgm_uniform",
            "karras",
            "exponential",
            "ddim_uniform",
            "beta",
            "normal",
            "linear_quadratic",
            "kl_optimal"
          ]
        ],
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "latent_image": ["LATENT"],
        "start_at_step": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 10000
          }
        ],
        "end_at_step": [
          "INT",
          {
            "default": 10000,
            "min": 0,
            "max": 10000
          }
        ],
        "return_with_leftover_noise": [["disable", "enable"]]
      }
    },
    "input_order": {
      "required": [
        "model",
        "add_noise",
        "noise_seed",
        "steps",
        "cfg",
        "sampler_name",
        "scheduler",
        "positive",
        "negative",
        "latent_image",
        "start_at_step",
        "end_at_step",
        "return_with_leftover_noise"
      ]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "KSamplerAdvanced",
    "display_name": "KSampler (Advanced)",
    "description": "",
    "python_module": "nodes",
    "category": "sampling",
    "output_node": false
  },
  "SetLatentNoiseMask": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "mask": ["MASK"]
      }
    },
    "input_order": {
      "required": ["samples", "mask"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "SetLatentNoiseMask",
    "display_name": "Set Latent Noise Mask",
    "description": "",
    "python_module": "nodes",
    "category": "latent/inpaint",
    "output_node": false
  },
  "LatentComposite": {
    "input": {
      "required": {
        "samples_to": ["LATENT"],
        "samples_from": ["LATENT"],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "feather": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples_to", "samples_from", "x", "y", "feather"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentComposite",
    "display_name": "Latent Composite",
    "description": "",
    "python_module": "nodes",
    "category": "latent",
    "output_node": false
  },
  "LatentBlend": {
    "input": {
      "required": {
        "samples1": ["LATENT"],
        "samples2": ["LATENT"],
        "blend_factor": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples1", "samples2", "blend_factor"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentBlend",
    "display_name": "Latent Blend",
    "description": "",
    "python_module": "nodes",
    "category": "_for_testing",
    "output_node": false
  },
  "LatentRotate": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "rotation": [["none", "90 degrees", "180 degrees", "270 degrees"]]
      }
    },
    "input_order": {
      "required": ["samples", "rotation"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentRotate",
    "display_name": "Rotate Latent",
    "description": "",
    "python_module": "nodes",
    "category": "latent/transform",
    "output_node": false
  },
  "LatentFlip": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "flip_method": [["x-axis: vertically", "y-axis: horizontally"]]
      }
    },
    "input_order": {
      "required": ["samples", "flip_method"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentFlip",
    "display_name": "Flip Latent",
    "description": "",
    "python_module": "nodes",
    "category": "latent/transform",
    "output_node": false
  },
  "LatentCrop": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "width": [
          "INT",
          {
            "default": 512,
            "min": 64,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 64,
            "max": 16384,
            "step": 8
          }
        ],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "width", "height", "x", "y"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentCrop",
    "display_name": "Crop Latent",
    "description": "",
    "python_module": "nodes",
    "category": "latent/transform",
    "output_node": false
  },
  "LoraLoader": {
    "input": {
      "required": {
        "model": [
          "MODEL",
          {
            "tooltip": "The diffusion model the LoRA will be applied to."
          }
        ],
        "clip": [
          "CLIP",
          {
            "tooltip": "The CLIP model the LoRA will be applied to."
          }
        ],
        "lora_name": [
          [],
          {
            "tooltip": "The name of the LoRA."
          }
        ],
        "strength_model": [
          "FLOAT",
          {
            "default": 1,
            "min": -100,
            "max": 100,
            "step": 0.01,
            "tooltip": "How strongly to modify the diffusion model. This value can be negative."
          }
        ],
        "strength_clip": [
          "FLOAT",
          {
            "default": 1,
            "min": -100,
            "max": 100,
            "step": 0.01,
            "tooltip": "How strongly to modify the CLIP model. This value can be negative."
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "clip",
        "lora_name",
        "strength_model",
        "strength_clip"
      ]
    },
    "output": ["MODEL", "CLIP"],
    "output_is_list": [false, false],
    "output_name": ["MODEL", "CLIP"],
    "name": "LoraLoader",
    "display_name": "Load LoRA",
    "description": "LoRAs are used to modify diffusion and CLIP models, altering the way in which latents are denoised such as applying styles. Multiple LoRA nodes can be linked together.",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false,
    "output_tooltips": [
      "The modified diffusion model.",
      "The modified CLIP model."
    ]
  },
  "CLIPLoader": {
    "input": {
      "required": {
        "clip_name": [[]],
        "type": [
          [
            "stable_diffusion",
            "stable_cascade",
            "sd3",
            "stable_audio",
            "mochi",
            "ltxv",
            "pixart",
            "cosmos",
            "lumina2",
            "wan",
            "hidream",
            "chroma",
            "ace",
            "omnigen2"
          ]
        ]
      },
      "optional": {
        "device": [
          ["default", "cpu"],
          {
            "advanced": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip_name", "type"],
      "optional": ["device"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "CLIPLoader",
    "display_name": "Load CLIP",
    "description": "[Recipes]\n\nstable_diffusion: clip-l\nstable_cascade: clip-g\nsd3: t5 xxl/ clip-g / clip-l\nstable_audio: t5 base\nmochi: t5 xxl\ncosmos: old t5 xxl\nlumina2: gemma 2 2B\nwan: umt5 xxl\n hidream: llama-3.1 (Recommend) or t5\nomnigen2: qwen vl 2.5 3B",
    "python_module": "nodes",
    "category": "advanced/loaders",
    "output_node": false
  },
  "UNETLoader": {
    "input": {
      "required": {
        "unet_name": [[]],
        "weight_dtype": [
          ["default", "fp8_e4m3fn", "fp8_e4m3fn_fast", "fp8_e5m2"]
        ]
      }
    },
    "input_order": {
      "required": ["unet_name", "weight_dtype"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "UNETLoader",
    "display_name": "Load Diffusion Model",
    "description": "",
    "python_module": "nodes",
    "category": "advanced/loaders",
    "output_node": false
  },
  "DualCLIPLoader": {
    "input": {
      "required": {
        "clip_name1": [[]],
        "clip_name2": [[]],
        "type": [["sdxl", "sd3", "flux", "hunyuan_video", "hidream"]]
      },
      "optional": {
        "device": [
          ["default", "cpu"],
          {
            "advanced": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip_name1", "clip_name2", "type"],
      "optional": ["device"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "DualCLIPLoader",
    "display_name": "DualCLIPLoader",
    "description": "[Recipes]\n\nsdxl: clip-l, clip-g\nsd3: clip-l, clip-g / clip-l, t5 / clip-g, t5\nflux: clip-l, t5\nhidream: at least one of t5 or llama, recommended t5 and llama",
    "python_module": "nodes",
    "category": "advanced/loaders",
    "output_node": false
  },
  "CLIPVisionEncode": {
    "input": {
      "required": {
        "clip_vision": ["CLIP_VISION"],
        "image": ["IMAGE"],
        "crop": [["center", "none"]]
      }
    },
    "input_order": {
      "required": ["clip_vision", "image", "crop"]
    },
    "output": ["CLIP_VISION_OUTPUT"],
    "output_is_list": [false],
    "output_name": ["CLIP_VISION_OUTPUT"],
    "name": "CLIPVisionEncode",
    "display_name": "CLIP Vision Encode",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "StyleModelApply": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "style_model": ["STYLE_MODEL"],
        "clip_vision_output": ["CLIP_VISION_OUTPUT"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.001
          }
        ],
        "strength_type": [["multiply", "attn_bias"]]
      }
    },
    "input_order": {
      "required": [
        "conditioning",
        "style_model",
        "clip_vision_output",
        "strength",
        "strength_type"
      ]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "StyleModelApply",
    "display_name": "Apply Style Model",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning/style_model",
    "output_node": false
  },
  "unCLIPConditioning": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "clip_vision_output": ["CLIP_VISION_OUTPUT"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": -10,
            "max": 10,
            "step": 0.01
          }
        ],
        "noise_augmentation": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "conditioning",
        "clip_vision_output",
        "strength",
        "noise_augmentation"
      ]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "unCLIPConditioning",
    "display_name": "unCLIPConditioning",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning",
    "output_node": false
  },
  "ControlNetApply": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "control_net": ["CONTROL_NET"],
        "image": ["IMAGE"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["conditioning", "control_net", "image", "strength"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ControlNetApply",
    "display_name": "Apply ControlNet (OLD)",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning/controlnet",
    "output_node": false,
    "deprecated": true
  },
  "ControlNetApplyAdvanced": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "control_net": ["CONTROL_NET"],
        "image": ["IMAGE"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      },
      "optional": {
        "vae": ["VAE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "control_net",
        "image",
        "strength",
        "start_percent",
        "end_percent"
      ],
      "optional": ["vae"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "ControlNetApplyAdvanced",
    "display_name": "Apply ControlNet",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning/controlnet",
    "output_node": false
  },
  "ControlNetLoader": {
    "input": {
      "required": {
        "control_net_name": [[]]
      }
    },
    "input_order": {
      "required": ["control_net_name"]
    },
    "output": ["CONTROL_NET"],
    "output_is_list": [false],
    "output_name": ["CONTROL_NET"],
    "name": "ControlNetLoader",
    "display_name": "Load ControlNet Model",
    "description": "",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false
  },
  "DiffControlNetLoader": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "control_net_name": [[]]
      }
    },
    "input_order": {
      "required": ["model", "control_net_name"]
    },
    "output": ["CONTROL_NET"],
    "output_is_list": [false],
    "output_name": ["CONTROL_NET"],
    "name": "DiffControlNetLoader",
    "display_name": "Load ControlNet Model (diff)",
    "description": "",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false
  },
  "StyleModelLoader": {
    "input": {
      "required": {
        "style_model_name": [[]]
      }
    },
    "input_order": {
      "required": ["style_model_name"]
    },
    "output": ["STYLE_MODEL"],
    "output_is_list": [false],
    "output_name": ["STYLE_MODEL"],
    "name": "StyleModelLoader",
    "display_name": "Load Style Model",
    "description": "",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false
  },
  "CLIPVisionLoader": {
    "input": {
      "required": {
        "clip_name": [[]]
      }
    },
    "input_order": {
      "required": ["clip_name"]
    },
    "output": ["CLIP_VISION"],
    "output_is_list": [false],
    "output_name": ["CLIP_VISION"],
    "name": "CLIPVisionLoader",
    "display_name": "Load CLIP Vision",
    "description": "",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false
  },
  "VAEDecodeTiled": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "vae": ["VAE"],
        "tile_size": [
          "INT",
          {
            "default": 512,
            "min": 64,
            "max": 4096,
            "step": 32
          }
        ],
        "overlap": [
          "INT",
          {
            "default": 64,
            "min": 0,
            "max": 4096,
            "step": 32
          }
        ],
        "temporal_size": [
          "INT",
          {
            "default": 64,
            "min": 8,
            "max": 4096,
            "step": 4,
            "tooltip": "Only used for video VAEs: Amount of frames to decode at a time."
          }
        ],
        "temporal_overlap": [
          "INT",
          {
            "default": 8,
            "min": 4,
            "max": 4096,
            "step": 4,
            "tooltip": "Only used for video VAEs: Amount of frames to overlap."
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "samples",
        "vae",
        "tile_size",
        "overlap",
        "temporal_size",
        "temporal_overlap"
      ]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "VAEDecodeTiled",
    "display_name": "VAE Decode (Tiled)",
    "description": "",
    "python_module": "nodes",
    "category": "_for_testing",
    "output_node": false
  },
  "VAEEncodeTiled": {
    "input": {
      "required": {
        "pixels": ["IMAGE"],
        "vae": ["VAE"],
        "tile_size": [
          "INT",
          {
            "default": 512,
            "min": 64,
            "max": 4096,
            "step": 64
          }
        ],
        "overlap": [
          "INT",
          {
            "default": 64,
            "min": 0,
            "max": 4096,
            "step": 32
          }
        ],
        "temporal_size": [
          "INT",
          {
            "default": 64,
            "min": 8,
            "max": 4096,
            "step": 4,
            "tooltip": "Only used for video VAEs: Amount of frames to encode at a time."
          }
        ],
        "temporal_overlap": [
          "INT",
          {
            "default": 8,
            "min": 4,
            "max": 4096,
            "step": 4,
            "tooltip": "Only used for video VAEs: Amount of frames to overlap."
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "pixels",
        "vae",
        "tile_size",
        "overlap",
        "temporal_size",
        "temporal_overlap"
      ]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "VAEEncodeTiled",
    "display_name": "VAE Encode (Tiled)",
    "description": "",
    "python_module": "nodes",
    "category": "_for_testing",
    "output_node": false
  },
  "unCLIPCheckpointLoader": {
    "input": {
      "required": {
        "ckpt_name": [[]]
      }
    },
    "input_order": {
      "required": ["ckpt_name"]
    },
    "output": ["MODEL", "CLIP", "VAE", "CLIP_VISION"],
    "output_is_list": [false, false, false, false],
    "output_name": ["MODEL", "CLIP", "VAE", "CLIP_VISION"],
    "name": "unCLIPCheckpointLoader",
    "display_name": "unCLIPCheckpointLoader",
    "description": "",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false
  },
  "GLIGENLoader": {
    "input": {
      "required": {
        "gligen_name": [[]]
      }
    },
    "input_order": {
      "required": ["gligen_name"]
    },
    "output": ["GLIGEN"],
    "output_is_list": [false],
    "output_name": ["GLIGEN"],
    "name": "GLIGENLoader",
    "display_name": "GLIGENLoader",
    "description": "",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false
  },
  "GLIGENTextBoxApply": {
    "input": {
      "required": {
        "conditioning_to": ["CONDITIONING"],
        "clip": ["CLIP"],
        "gligen_textbox_model": ["GLIGEN"],
        "text": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "width": [
          "INT",
          {
            "default": 64,
            "min": 8,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 64,
            "min": 8,
            "max": 16384,
            "step": 8
          }
        ],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "conditioning_to",
        "clip",
        "gligen_textbox_model",
        "text",
        "width",
        "height",
        "x",
        "y"
      ]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "GLIGENTextBoxApply",
    "display_name": "GLIGENTextBoxApply",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning/gligen",
    "output_node": false
  },
  "InpaintModelConditioning": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "pixels": ["IMAGE"],
        "mask": ["MASK"],
        "noise_mask": [
          "BOOLEAN",
          {
            "default": true,
            "tooltip": "Add a noise mask to the latent so sampling will only happen within the mask. Might improve results or completely break things depending on the model."
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "pixels",
        "mask",
        "noise_mask"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "InpaintModelConditioning",
    "display_name": "InpaintModelConditioning",
    "description": "",
    "python_module": "nodes",
    "category": "conditioning/inpaint",
    "output_node": false
  },
  "CheckpointLoader": {
    "input": {
      "required": {
        "config_name": [
          [
            "anything_v3.yaml",
            "v1-inference.yaml",
            "v1-inference_clip_skip_2.yaml",
            "v1-inference_clip_skip_2_fp16.yaml",
            "v1-inference_fp16.yaml",
            "v1-inpainting-inference.yaml",
            "v2-inference-v.yaml",
            "v2-inference-v_fp32.yaml",
            "v2-inference.yaml",
            "v2-inference_fp32.yaml",
            "v2-inpainting-inference.yaml"
          ]
        ],
        "ckpt_name": [[]]
      }
    },
    "input_order": {
      "required": ["config_name", "ckpt_name"]
    },
    "output": ["MODEL", "CLIP", "VAE"],
    "output_is_list": [false, false, false],
    "output_name": ["MODEL", "CLIP", "VAE"],
    "name": "CheckpointLoader",
    "display_name": "Load Checkpoint With Config (DEPRECATED)",
    "description": "",
    "python_module": "nodes",
    "category": "advanced/loaders",
    "output_node": false,
    "deprecated": true
  },
  "DiffusersLoader": {
    "input": {
      "required": {
        "model_path": [[]]
      }
    },
    "input_order": {
      "required": ["model_path"]
    },
    "output": ["MODEL", "CLIP", "VAE"],
    "output_is_list": [false, false, false],
    "output_name": ["MODEL", "CLIP", "VAE"],
    "name": "DiffusersLoader",
    "display_name": "DiffusersLoader",
    "description": "",
    "python_module": "nodes",
    "category": "advanced/loaders/deprecated",
    "output_node": false
  },
  "LoadLatent": {
    "input": {
      "required": {
        "latent": [[]]
      }
    },
    "input_order": {
      "required": ["latent"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LoadLatent",
    "display_name": "LoadLatent",
    "description": "",
    "python_module": "nodes",
    "category": "_for_testing",
    "output_node": false
  },
  "SaveLatent": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "filename_prefix": [
          "STRING",
          {
            "default": "latents/ComfyUI"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["samples", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveLatent",
    "display_name": "SaveLatent",
    "description": "",
    "python_module": "nodes",
    "category": "_for_testing",
    "output_node": true
  },
  "ConditioningZeroOut": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"]
      }
    },
    "input_order": {
      "required": ["conditioning"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningZeroOut",
    "display_name": "ConditioningZeroOut",
    "description": "",
    "python_module": "nodes",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "ConditioningSetTimestepRange": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "start": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": ["conditioning", "start", "end"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetTimestepRange",
    "display_name": "ConditioningSetTimestepRange",
    "description": "",
    "python_module": "nodes",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "LoraLoaderModelOnly": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "lora_name": [[]],
        "strength_model": [
          "FLOAT",
          {
            "default": 1,
            "min": -100,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "lora_name", "strength_model"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "LoraLoaderModelOnly",
    "display_name": "LoraLoaderModelOnly",
    "description": "LoRAs are used to modify diffusion and CLIP models, altering the way in which latents are denoised such as applying styles. Multiple LoRA nodes can be linked together.",
    "python_module": "nodes",
    "category": "loaders",
    "output_node": false,
    "output_tooltips": [
      "The modified diffusion model.",
      "The modified CLIP model."
    ]
  },
  "LatentAdd": {
    "input": {
      "required": {
        "samples1": ["LATENT"],
        "samples2": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["samples1", "samples2"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentAdd",
    "display_name": "LatentAdd",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced",
    "output_node": false
  },
  "LatentSubtract": {
    "input": {
      "required": {
        "samples1": ["LATENT"],
        "samples2": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["samples1", "samples2"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentSubtract",
    "display_name": "LatentSubtract",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced",
    "output_node": false
  },
  "LatentMultiply": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "multiplier": [
          "FLOAT",
          {
            "default": 1,
            "min": -10,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "multiplier"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentMultiply",
    "display_name": "LatentMultiply",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced",
    "output_node": false
  },
  "LatentInterpolate": {
    "input": {
      "required": {
        "samples1": ["LATENT"],
        "samples2": ["LATENT"],
        "ratio": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples1", "samples2", "ratio"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentInterpolate",
    "display_name": "LatentInterpolate",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced",
    "output_node": false
  },
  "LatentBatch": {
    "input": {
      "required": {
        "samples1": ["LATENT"],
        "samples2": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["samples1", "samples2"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentBatch",
    "display_name": "LatentBatch",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/batch",
    "output_node": false
  },
  "LatentBatchSeedBehavior": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "seed_behavior": [
          ["random", "fixed"],
          {
            "default": "fixed"
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "seed_behavior"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentBatchSeedBehavior",
    "display_name": "LatentBatchSeedBehavior",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced",
    "output_node": false
  },
  "LatentApplyOperation": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "operation": ["LATENT_OPERATION"]
      }
    },
    "input_order": {
      "required": ["samples", "operation"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentApplyOperation",
    "display_name": "LatentApplyOperation",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced/operations",
    "output_node": false,
    "experimental": true
  },
  "LatentApplyOperationCFG": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "operation": ["LATENT_OPERATION"]
      }
    },
    "input_order": {
      "required": ["model", "operation"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "LatentApplyOperationCFG",
    "display_name": "LatentApplyOperationCFG",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced/operations",
    "output_node": false,
    "experimental": true
  },
  "LatentOperationTonemapReinhard": {
    "input": {
      "required": {
        "multiplier": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["multiplier"]
    },
    "output": ["LATENT_OPERATION"],
    "output_is_list": [false],
    "output_name": ["LATENT_OPERATION"],
    "name": "LatentOperationTonemapReinhard",
    "display_name": "LatentOperationTonemapReinhard",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced/operations",
    "output_node": false,
    "experimental": true
  },
  "LatentOperationSharpen": {
    "input": {
      "required": {
        "sharpen_radius": [
          "INT",
          {
            "default": 9,
            "min": 1,
            "max": 31,
            "step": 1
          }
        ],
        "sigma": [
          "FLOAT",
          {
            "default": 1,
            "min": 0.1,
            "max": 10,
            "step": 0.1
          }
        ],
        "alpha": [
          "FLOAT",
          {
            "default": 0.1,
            "min": 0,
            "max": 5,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["sharpen_radius", "sigma", "alpha"]
    },
    "output": ["LATENT_OPERATION"],
    "output_is_list": [false],
    "output_name": ["LATENT_OPERATION"],
    "name": "LatentOperationSharpen",
    "display_name": "LatentOperationSharpen",
    "description": "",
    "python_module": "comfy_extras.nodes_latent",
    "category": "latent/advanced/operations",
    "output_node": false,
    "experimental": true
  },
  "HypernetworkLoader": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "hypernetwork_name": [[]],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": -10,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "hypernetwork_name", "strength"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "HypernetworkLoader",
    "display_name": "HypernetworkLoader",
    "description": "",
    "python_module": "comfy_extras.nodes_hypernetwork",
    "category": "loaders",
    "output_node": false
  },
  "UpscaleModelLoader": {
    "input": {
      "required": {
        "model_name": [[]]
      }
    },
    "input_order": {
      "required": ["model_name"]
    },
    "output": ["UPSCALE_MODEL"],
    "output_is_list": [false],
    "output_name": ["UPSCALE_MODEL"],
    "name": "UpscaleModelLoader",
    "display_name": "Load Upscale Model",
    "description": "",
    "python_module": "comfy_extras.nodes_upscale_model",
    "category": "loaders",
    "output_node": false
  },
  "ImageUpscaleWithModel": {
    "input": {
      "required": {
        "upscale_model": ["UPSCALE_MODEL"],
        "image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["upscale_model", "image"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageUpscaleWithModel",
    "display_name": "Upscale Image (using Model)",
    "description": "",
    "python_module": "comfy_extras.nodes_upscale_model",
    "category": "image/upscaling",
    "output_node": false
  },
  "ImageBlend": {
    "input": {
      "required": {
        "image1": ["IMAGE"],
        "image2": ["IMAGE"],
        "blend_factor": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blend_mode": [
          [
            "normal",
            "multiply",
            "screen",
            "overlay",
            "soft_light",
            "difference"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["image1", "image2", "blend_factor", "blend_mode"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageBlend",
    "display_name": "Image Blend",
    "description": "",
    "python_module": "comfy_extras.nodes_post_processing",
    "category": "image/postprocessing",
    "output_node": false
  },
  "ImageBlur": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "blur_radius": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 31,
            "step": 1
          }
        ],
        "sigma": [
          "FLOAT",
          {
            "default": 1,
            "min": 0.1,
            "max": 10,
            "step": 0.1
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "blur_radius", "sigma"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageBlur",
    "display_name": "Image Blur",
    "description": "",
    "python_module": "comfy_extras.nodes_post_processing",
    "category": "image/postprocessing",
    "output_node": false
  },
  "ImageQuantize": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "colors": [
          "INT",
          {
            "default": 256,
            "min": 1,
            "max": 256,
            "step": 1
          }
        ],
        "dither": [
          [
            "none",
            "floyd-steinberg",
            "bayer-2",
            "bayer-4",
            "bayer-8",
            "bayer-16"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["image", "colors", "dither"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageQuantize",
    "display_name": "Image Quantize",
    "description": "",
    "python_module": "comfy_extras.nodes_post_processing",
    "category": "image/postprocessing",
    "output_node": false
  },
  "ImageSharpen": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "sharpen_radius": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 31,
            "step": 1
          }
        ],
        "sigma": [
          "FLOAT",
          {
            "default": 1,
            "min": 0.1,
            "max": 10,
            "step": 0.01
          }
        ],
        "alpha": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 5,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "sharpen_radius", "sigma", "alpha"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageSharpen",
    "display_name": "Image Sharpen",
    "description": "",
    "python_module": "comfy_extras.nodes_post_processing",
    "category": "image/postprocessing",
    "output_node": false
  },
  "ImageScaleToTotalPixels": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "upscale_method": [
          ["nearest-exact", "bilinear", "area", "bicubic", "lanczos"]
        ],
        "megapixels": [
          "FLOAT",
          {
            "default": 1,
            "min": 0.01,
            "max": 16,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "upscale_method", "megapixels"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageScaleToTotalPixels",
    "display_name": "Scale Image to Total Pixels",
    "description": "",
    "python_module": "comfy_extras.nodes_post_processing",
    "category": "image/upscaling",
    "output_node": false
  },
  "LatentCompositeMasked": {
    "input": {
      "required": {
        "destination": ["LATENT"],
        "source": ["LATENT"],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 8
          }
        ],
        "resize_source": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "optional": {
        "mask": ["MASK"]
      }
    },
    "input_order": {
      "required": ["destination", "source", "x", "y", "resize_source"],
      "optional": ["mask"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "LatentCompositeMasked",
    "display_name": "LatentCompositeMasked",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "latent",
    "output_node": false
  },
  "ImageCompositeMasked": {
    "input": {
      "required": {
        "destination": ["IMAGE"],
        "source": ["IMAGE"],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "resize_source": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "optional": {
        "mask": ["MASK"]
      }
    },
    "input_order": {
      "required": ["destination", "source", "x", "y", "resize_source"],
      "optional": ["mask"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageCompositeMasked",
    "display_name": "ImageCompositeMasked",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "image",
    "output_node": false
  },
  "MaskToImage": {
    "input": {
      "required": {
        "mask": ["MASK"]
      }
    },
    "input_order": {
      "required": ["mask"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "MaskToImage",
    "display_name": "Convert Mask to Image",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "ImageToMask": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "channel": [["red", "green", "blue", "alpha"]]
      }
    },
    "input_order": {
      "required": ["image", "channel"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "ImageToMask",
    "display_name": "Convert Image to Mask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "ImageColorToMask": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "color": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16777215,
            "step": 1,
            "display": "color"
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "color"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "ImageColorToMask",
    "display_name": "ImageColorToMask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "SolidMask": {
    "input": {
      "required": {
        "value": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "width": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["value", "width", "height"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "SolidMask",
    "display_name": "SolidMask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "InvertMask": {
    "input": {
      "required": {
        "mask": ["MASK"]
      }
    },
    "input_order": {
      "required": ["mask"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "InvertMask",
    "display_name": "InvertMask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "CropMask": {
    "input": {
      "required": {
        "mask": ["MASK"],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "width": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["mask", "x", "y", "width", "height"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "CropMask",
    "display_name": "CropMask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "MaskComposite": {
    "input": {
      "required": {
        "destination": ["MASK"],
        "source": ["MASK"],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "operation": [["multiply", "add", "subtract", "and", "or", "xor"]]
      }
    },
    "input_order": {
      "required": ["destination", "source", "x", "y", "operation"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "MaskComposite",
    "display_name": "MaskComposite",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "FeatherMask": {
    "input": {
      "required": {
        "mask": ["MASK"],
        "left": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "top": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "right": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "bottom": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["mask", "left", "top", "right", "bottom"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "FeatherMask",
    "display_name": "FeatherMask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "GrowMask": {
    "input": {
      "required": {
        "mask": ["MASK"],
        "expand": [
          "INT",
          {
            "default": 0,
            "min": -16384,
            "max": 16384,
            "step": 1
          }
        ],
        "tapered_corners": [
          "BOOLEAN",
          {
            "default": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["mask", "expand", "tapered_corners"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "GrowMask",
    "display_name": "GrowMask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "ThresholdMask": {
    "input": {
      "required": {
        "mask": ["MASK"],
        "value": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["mask", "value"]
    },
    "output": ["MASK"],
    "output_is_list": [false],
    "output_name": ["MASK"],
    "name": "ThresholdMask",
    "display_name": "ThresholdMask",
    "description": "",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": false
  },
  "MaskPreview": {
    "input": {
      "required": {
        "mask": ["MASK"]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["mask"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "MaskPreview",
    "display_name": "MaskPreview",
    "description": "Saves the input images to your ComfyUI output directory.",
    "python_module": "comfy_extras.nodes_mask",
    "category": "mask",
    "output_node": true
  },
  "PorterDuffImageComposite": {
    "input": {
      "required": {
        "source": ["IMAGE"],
        "source_alpha": ["MASK"],
        "destination": ["IMAGE"],
        "destination_alpha": ["MASK"],
        "mode": [
          [
            "ADD",
            "CLEAR",
            "DARKEN",
            "DST",
            "DST_ATOP",
            "DST_IN",
            "DST_OUT",
            "DST_OVER",
            "LIGHTEN",
            "MULTIPLY",
            "OVERLAY",
            "SCREEN",
            "SRC",
            "SRC_ATOP",
            "SRC_IN",
            "SRC_OUT",
            "SRC_OVER",
            "XOR"
          ],
          {
            "default": "DST"
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "source",
        "source_alpha",
        "destination",
        "destination_alpha",
        "mode"
      ]
    },
    "output": ["IMAGE", "MASK"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "MASK"],
    "name": "PorterDuffImageComposite",
    "display_name": "Porter-Duff Image Composite",
    "description": "",
    "python_module": "comfy_extras.nodes_compositing",
    "category": "mask/compositing",
    "output_node": false
  },
  "SplitImageWithAlpha": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["image"]
    },
    "output": ["IMAGE", "MASK"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "MASK"],
    "name": "SplitImageWithAlpha",
    "display_name": "Split Image with Alpha",
    "description": "",
    "python_module": "comfy_extras.nodes_compositing",
    "category": "mask/compositing",
    "output_node": false
  },
  "JoinImageWithAlpha": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "alpha": ["MASK"]
      }
    },
    "input_order": {
      "required": ["image", "alpha"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "JoinImageWithAlpha",
    "display_name": "Join Image with Alpha",
    "description": "",
    "python_module": "comfy_extras.nodes_compositing",
    "category": "mask/compositing",
    "output_node": false
  },
  "RebatchLatents": {
    "input": {
      "required": {
        "latents": ["LATENT"],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["latents", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [true],
    "output_name": ["LATENT"],
    "name": "RebatchLatents",
    "display_name": "Rebatch Latents",
    "description": "",
    "python_module": "comfy_extras.nodes_rebatch",
    "category": "latent/batch",
    "output_node": false
  },
  "RebatchImages": {
    "input": {
      "required": {
        "images": ["IMAGE"],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["images", "batch_size"]
    },
    "output": ["IMAGE"],
    "output_is_list": [true],
    "output_name": ["IMAGE"],
    "name": "RebatchImages",
    "display_name": "Rebatch Images",
    "description": "",
    "python_module": "comfy_extras.nodes_rebatch",
    "category": "image/batch",
    "output_node": false
  },
  "ModelMergeSimple": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "ratio": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model1", "model2", "ratio"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeSimple",
    "display_name": "ModelMergeSimple",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": false
  },
  "ModelMergeBlocks": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "input": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "out": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model1", "model2", "input", "middle", "out"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeBlocks",
    "display_name": "ModelMergeBlocks",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": false
  },
  "ModelMergeSubtract": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "multiplier": [
          "FLOAT",
          {
            "default": 1,
            "min": -10,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model1", "model2", "multiplier"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeSubtract",
    "display_name": "ModelMergeSubtract",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": false
  },
  "ModelMergeAdd": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"]
      }
    },
    "input_order": {
      "required": ["model1", "model2"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeAdd",
    "display_name": "ModelMergeAdd",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": false
  },
  "CheckpointSave": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "clip": ["CLIP"],
        "vae": ["VAE"],
        "filename_prefix": [
          "STRING",
          {
            "default": "checkpoints/ComfyUI"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["model", "clip", "vae", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "CheckpointSave",
    "display_name": "Save Checkpoint",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": true
  },
  "CLIPMergeSimple": {
    "input": {
      "required": {
        "clip1": ["CLIP"],
        "clip2": ["CLIP"],
        "ratio": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip1", "clip2", "ratio"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "CLIPMergeSimple",
    "display_name": "CLIPMergeSimple",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": false
  },
  "CLIPMergeSubtract": {
    "input": {
      "required": {
        "clip1": ["CLIP"],
        "clip2": ["CLIP"],
        "multiplier": [
          "FLOAT",
          {
            "default": 1,
            "min": -10,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip1", "clip2", "multiplier"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "CLIPMergeSubtract",
    "display_name": "CLIPMergeSubtract",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": false
  },
  "CLIPMergeAdd": {
    "input": {
      "required": {
        "clip1": ["CLIP"],
        "clip2": ["CLIP"]
      }
    },
    "input_order": {
      "required": ["clip1", "clip2"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "CLIPMergeAdd",
    "display_name": "CLIPMergeAdd",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": false
  },
  "CLIPSave": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "filename_prefix": [
          "STRING",
          {
            "default": "clip/ComfyUI"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["clip", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "CLIPSave",
    "display_name": "CLIPSave",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": true
  },
  "VAESave": {
    "input": {
      "required": {
        "vae": ["VAE"],
        "filename_prefix": [
          "STRING",
          {
            "default": "vae/ComfyUI_vae"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["vae", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "VAESave",
    "display_name": "VAESave",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": true
  },
  "ModelSave": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "filename_prefix": [
          "STRING",
          {
            "default": "diffusion_models/ComfyUI"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["model", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "ModelSave",
    "display_name": "ModelSave",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging",
    "category": "advanced/model_merging",
    "output_node": true
  },
  "TomePatchModel": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "ratio": [
          "FLOAT",
          {
            "default": 0.3,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "ratio"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "TomePatchModel",
    "display_name": "TomePatchModel",
    "description": "",
    "python_module": "comfy_extras.nodes_tomesd",
    "category": "model_patches/unet",
    "output_node": false
  },
  "CLIPTextEncodeSDXLRefiner": {
    "input": {
      "required": {
        "ascore": [
          "FLOAT",
          {
            "default": 6,
            "min": 0,
            "max": 1000,
            "step": 0.01
          }
        ],
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "text": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "clip": ["CLIP"]
      }
    },
    "input_order": {
      "required": ["ascore", "width", "height", "text", "clip"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeSDXLRefiner",
    "display_name": "CLIPTextEncodeSDXLRefiner",
    "description": "",
    "python_module": "comfy_extras.nodes_clip_sdxl",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "CLIPTextEncodeSDXL": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "crop_w": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384
          }
        ],
        "crop_h": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384
          }
        ],
        "target_width": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "target_height": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "text_g": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "text_l": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "clip",
        "width",
        "height",
        "crop_w",
        "crop_h",
        "target_width",
        "target_height",
        "text_g",
        "text_l"
      ]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeSDXL",
    "display_name": "CLIPTextEncodeSDXL",
    "description": "",
    "python_module": "comfy_extras.nodes_clip_sdxl",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "Canny": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "low_threshold": [
          "FLOAT",
          {
            "default": 0.4,
            "min": 0.01,
            "max": 0.99,
            "step": 0.01
          }
        ],
        "high_threshold": [
          "FLOAT",
          {
            "default": 0.8,
            "min": 0.01,
            "max": 0.99,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "low_threshold", "high_threshold"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "Canny",
    "display_name": "Canny",
    "description": "",
    "python_module": "comfy_extras.nodes_canny",
    "category": "image/preprocessors",
    "output_node": false
  },
  "FreeU": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "b1": [
          "FLOAT",
          {
            "default": 1.1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "b2": [
          "FLOAT",
          {
            "default": 1.2,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "s1": [
          "FLOAT",
          {
            "default": 0.9,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "s2": [
          "FLOAT",
          {
            "default": 0.2,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "b1", "b2", "s1", "s2"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "FreeU",
    "display_name": "FreeU",
    "description": "",
    "python_module": "comfy_extras.nodes_freelunch",
    "category": "model_patches/unet",
    "output_node": false
  },
  "FreeU_V2": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "b1": [
          "FLOAT",
          {
            "default": 1.3,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "b2": [
          "FLOAT",
          {
            "default": 1.4,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "s1": [
          "FLOAT",
          {
            "default": 0.9,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "s2": [
          "FLOAT",
          {
            "default": 0.2,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "b1", "b2", "s1", "s2"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "FreeU_V2",
    "display_name": "FreeU_V2",
    "description": "",
    "python_module": "comfy_extras.nodes_freelunch",
    "category": "model_patches/unet",
    "output_node": false
  },
  "SamplerCustom": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "add_noise": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "noise_seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true
          }
        ],
        "cfg": [
          "FLOAT",
          {
            "default": 8,
            "min": 0,
            "max": 100,
            "step": 0.1,
            "round": 0.01
          }
        ],
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "sampler": ["SAMPLER"],
        "sigmas": ["SIGMAS"],
        "latent_image": ["LATENT"]
      }
    },
    "input_order": {
      "required": [
        "model",
        "add_noise",
        "noise_seed",
        "cfg",
        "positive",
        "negative",
        "sampler",
        "sigmas",
        "latent_image"
      ]
    },
    "output": ["LATENT", "LATENT"],
    "output_is_list": [false, false],
    "output_name": ["output", "denoised_output"],
    "name": "SamplerCustom",
    "display_name": "SamplerCustom",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling",
    "output_node": false
  },
  "BasicScheduler": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "scheduler": [
          [
            "simple",
            "sgm_uniform",
            "karras",
            "exponential",
            "ddim_uniform",
            "beta",
            "normal",
            "linear_quadratic",
            "kl_optimal"
          ]
        ],
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "denoise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "scheduler", "steps", "denoise"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "BasicScheduler",
    "display_name": "BasicScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "KarrasScheduler": {
    "input": {
      "required": {
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "sigma_max": [
          "FLOAT",
          {
            "default": 14.614642,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "sigma_min": [
          "FLOAT",
          {
            "default": 0.0291675,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "rho": [
          "FLOAT",
          {
            "default": 7,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["steps", "sigma_max", "sigma_min", "rho"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "KarrasScheduler",
    "display_name": "KarrasScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "ExponentialScheduler": {
    "input": {
      "required": {
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "sigma_max": [
          "FLOAT",
          {
            "default": 14.614642,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "sigma_min": [
          "FLOAT",
          {
            "default": 0.0291675,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["steps", "sigma_max", "sigma_min"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "ExponentialScheduler",
    "display_name": "ExponentialScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "PolyexponentialScheduler": {
    "input": {
      "required": {
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "sigma_max": [
          "FLOAT",
          {
            "default": 14.614642,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "sigma_min": [
          "FLOAT",
          {
            "default": 0.0291675,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "rho": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["steps", "sigma_max", "sigma_min", "rho"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "PolyexponentialScheduler",
    "display_name": "PolyexponentialScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "LaplaceScheduler": {
    "input": {
      "required": {
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "sigma_max": [
          "FLOAT",
          {
            "default": 14.614642,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "sigma_min": [
          "FLOAT",
          {
            "default": 0.0291675,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "mu": [
          "FLOAT",
          {
            "default": 0,
            "min": -10,
            "max": 10,
            "step": 0.1,
            "round": false
          }
        ],
        "beta": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 10,
            "step": 0.1,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["steps", "sigma_max", "sigma_min", "mu", "beta"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "LaplaceScheduler",
    "display_name": "LaplaceScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "VPScheduler": {
    "input": {
      "required": {
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "beta_d": [
          "FLOAT",
          {
            "default": 19.9,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "beta_min": [
          "FLOAT",
          {
            "default": 0.1,
            "min": 0,
            "max": 5000,
            "step": 0.01,
            "round": false
          }
        ],
        "eps_s": [
          "FLOAT",
          {
            "default": 0.001,
            "min": 0,
            "max": 1,
            "step": 0.0001,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["steps", "beta_d", "beta_min", "eps_s"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "VPScheduler",
    "display_name": "VPScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "BetaSamplingScheduler": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "alpha": [
          "FLOAT",
          {
            "default": 0.6,
            "min": 0,
            "max": 50,
            "step": 0.01,
            "round": false
          }
        ],
        "beta": [
          "FLOAT",
          {
            "default": 0.6,
            "min": 0,
            "max": 50,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "steps", "alpha", "beta"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "BetaSamplingScheduler",
    "display_name": "BetaSamplingScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "SDTurboScheduler": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "steps": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 10
          }
        ],
        "denoise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "steps", "denoise"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "SDTurboScheduler",
    "display_name": "SDTurboScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "KSamplerSelect": {
    "input": {
      "required": {
        "sampler_name": [
          [
            "euler",
            "euler_cfg_pp",
            "euler_ancestral",
            "euler_ancestral_cfg_pp",
            "heun",
            "heunpp2",
            "dpm_2",
            "dpm_2_ancestral",
            "lms",
            "dpm_fast",
            "dpm_adaptive",
            "dpmpp_2s_ancestral",
            "dpmpp_2s_ancestral_cfg_pp",
            "dpmpp_sde",
            "dpmpp_sde_gpu",
            "dpmpp_2m",
            "dpmpp_2m_cfg_pp",
            "dpmpp_2m_sde",
            "dpmpp_2m_sde_gpu",
            "dpmpp_3m_sde",
            "dpmpp_3m_sde_gpu",
            "ddpm",
            "lcm",
            "ipndm",
            "ipndm_v",
            "deis",
            "res_multistep",
            "res_multistep_cfg_pp",
            "res_multistep_ancestral",
            "res_multistep_ancestral_cfg_pp",
            "gradient_estimation",
            "gradient_estimation_cfg_pp",
            "er_sde",
            "seeds_2",
            "seeds_3",
            "sa_solver",
            "sa_solver_pece",
            "ddim",
            "uni_pc",
            "uni_pc_bh2"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["sampler_name"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "KSamplerSelect",
    "display_name": "KSamplerSelect",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerEulerAncestral": {
    "input": {
      "required": {
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["eta", "s_noise"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerEulerAncestral",
    "display_name": "SamplerEulerAncestral",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerEulerAncestralCFGPP": {
    "input": {
      "required": {
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "round": false
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["eta", "s_noise"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerEulerAncestralCFGPP",
    "display_name": "SamplerEulerAncestralCFG++",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerLMS": {
    "input": {
      "required": {
        "order": [
          "INT",
          {
            "default": 4,
            "min": 1,
            "max": 100
          }
        ]
      }
    },
    "input_order": {
      "required": ["order"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerLMS",
    "display_name": "SamplerLMS",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerDPMPP_3M_SDE": {
    "input": {
      "required": {
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "noise_device": [["gpu", "cpu"]]
      }
    },
    "input_order": {
      "required": ["eta", "s_noise", "noise_device"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerDPMPP_3M_SDE",
    "display_name": "SamplerDPMPP_3M_SDE",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerDPMPP_2M_SDE": {
    "input": {
      "required": {
        "solver_type": [["midpoint", "heun"]],
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "noise_device": [["gpu", "cpu"]]
      }
    },
    "input_order": {
      "required": ["solver_type", "eta", "s_noise", "noise_device"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerDPMPP_2M_SDE",
    "display_name": "SamplerDPMPP_2M_SDE",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerDPMPP_SDE": {
    "input": {
      "required": {
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "r": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "noise_device": [["gpu", "cpu"]]
      }
    },
    "input_order": {
      "required": ["eta", "s_noise", "r", "noise_device"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerDPMPP_SDE",
    "display_name": "SamplerDPMPP_SDE",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerDPMPP_2S_Ancestral": {
    "input": {
      "required": {
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["eta", "s_noise"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerDPMPP_2S_Ancestral",
    "display_name": "SamplerDPMPP_2S_Ancestral",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerDPMAdaptative": {
    "input": {
      "required": {
        "order": [
          "INT",
          {
            "default": 3,
            "min": 2,
            "max": 3
          }
        ],
        "rtol": [
          "FLOAT",
          {
            "default": 0.05,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "atol": [
          "FLOAT",
          {
            "default": 0.0078,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "h_init": [
          "FLOAT",
          {
            "default": 0.05,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "pcoeff": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "icoeff": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "dcoeff": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "accept_safety": [
          "FLOAT",
          {
            "default": 0.81,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "eta": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "order",
        "rtol",
        "atol",
        "h_init",
        "pcoeff",
        "icoeff",
        "dcoeff",
        "accept_safety",
        "eta",
        "s_noise"
      ]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerDPMAdaptative",
    "display_name": "SamplerDPMAdaptative",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerER_SDE": {
    "input": {
      "required": {
        "solver_type": [
          "COMBO",
          {
            "options": ["ER-SDE", "Reverse-time SDE", "ODE"]
          }
        ],
        "max_stage": [
          "INT",
          {
            "default": 3,
            "min": 1,
            "max": 3
          }
        ],
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false,
            "tooltip": "Stochastic strength of reverse-time SDE.\nWhen eta=0, it reduces to deterministic ODE. This setting doesn't apply to ER-SDE solver type."
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["solver_type", "max_stage", "eta", "s_noise"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerER_SDE",
    "display_name": "SamplerER_SDE",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerSASolver": {
    "input": {
      "required": {
        "model": ["MODEL", {}],
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01,
            "round": false
          }
        ],
        "sde_start_percent": [
          "FLOAT",
          {
            "default": 0.2,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "sde_end_percent": [
          "FLOAT",
          {
            "default": 0.8,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "s_noise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": false
          }
        ],
        "predictor_order": [
          "INT",
          {
            "default": 3,
            "min": 1,
            "max": 6
          }
        ],
        "corrector_order": [
          "INT",
          {
            "default": 4,
            "min": 0,
            "max": 6
          }
        ],
        "use_pece": ["BOOLEAN", {}],
        "simple_order_2": ["BOOLEAN", {}]
      }
    },
    "input_order": {
      "required": [
        "model",
        "eta",
        "sde_start_percent",
        "sde_end_percent",
        "s_noise",
        "predictor_order",
        "corrector_order",
        "use_pece",
        "simple_order_2"
      ]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerSASolver",
    "display_name": "SamplerSASolver",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SplitSigmas": {
    "input": {
      "required": {
        "sigmas": ["SIGMAS"],
        "step": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 10000
          }
        ]
      }
    },
    "input_order": {
      "required": ["sigmas", "step"]
    },
    "output": ["SIGMAS", "SIGMAS"],
    "output_is_list": [false, false],
    "output_name": ["high_sigmas", "low_sigmas"],
    "name": "SplitSigmas",
    "display_name": "SplitSigmas",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/sigmas",
    "output_node": false
  },
  "SplitSigmasDenoise": {
    "input": {
      "required": {
        "sigmas": ["SIGMAS"],
        "denoise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["sigmas", "denoise"]
    },
    "output": ["SIGMAS", "SIGMAS"],
    "output_is_list": [false, false],
    "output_name": ["high_sigmas", "low_sigmas"],
    "name": "SplitSigmasDenoise",
    "display_name": "SplitSigmasDenoise",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/sigmas",
    "output_node": false
  },
  "FlipSigmas": {
    "input": {
      "required": {
        "sigmas": ["SIGMAS"]
      }
    },
    "input_order": {
      "required": ["sigmas"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "FlipSigmas",
    "display_name": "FlipSigmas",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/sigmas",
    "output_node": false
  },
  "SetFirstSigma": {
    "input": {
      "required": {
        "sigmas": ["SIGMAS"],
        "sigma": [
          "FLOAT",
          {
            "default": 136,
            "min": 0,
            "max": 20000,
            "step": 0.001,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["sigmas", "sigma"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "SetFirstSigma",
    "display_name": "SetFirstSigma",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/sigmas",
    "output_node": false
  },
  "ExtendIntermediateSigmas": {
    "input": {
      "required": {
        "sigmas": ["SIGMAS"],
        "steps": [
          "INT",
          {
            "default": 2,
            "min": 1,
            "max": 100
          }
        ],
        "start_at_sigma": [
          "FLOAT",
          {
            "default": -1,
            "min": -1,
            "max": 20000,
            "step": 0.01,
            "round": false
          }
        ],
        "end_at_sigma": [
          "FLOAT",
          {
            "default": 12,
            "min": 0,
            "max": 20000,
            "step": 0.01,
            "round": false
          }
        ],
        "spacing": [["linear", "cosine", "sine"]]
      }
    },
    "input_order": {
      "required": [
        "sigmas",
        "steps",
        "start_at_sigma",
        "end_at_sigma",
        "spacing"
      ]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "ExtendIntermediateSigmas",
    "display_name": "ExtendIntermediateSigmas",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/sigmas",
    "output_node": false
  },
  "SamplingPercentToSigma": {
    "input": {
      "required": {
        "model": ["MODEL", {}],
        "sampling_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.0001
          }
        ],
        "return_actual_sigma": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Return the actual sigma value instead of the value used for interval checks.\nThis only affects results at 0.0 and 1.0."
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "sampling_percent", "return_actual_sigma"]
    },
    "output": ["FLOAT"],
    "output_is_list": [false],
    "output_name": ["sigma_value"],
    "name": "SamplingPercentToSigma",
    "display_name": "SamplingPercentToSigma",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/sigmas",
    "output_node": false
  },
  "CFGGuider": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "cfg": [
          "FLOAT",
          {
            "default": 8,
            "min": 0,
            "max": 100,
            "step": 0.1,
            "round": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "positive", "negative", "cfg"]
    },
    "output": ["GUIDER"],
    "output_is_list": [false],
    "output_name": ["GUIDER"],
    "name": "CFGGuider",
    "display_name": "CFGGuider",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/guiders",
    "output_node": false
  },
  "DualCFGGuider": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "cond1": ["CONDITIONING"],
        "cond2": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "cfg_conds": [
          "FLOAT",
          {
            "default": 8,
            "min": 0,
            "max": 100,
            "step": 0.1,
            "round": 0.01
          }
        ],
        "cfg_cond2_negative": [
          "FLOAT",
          {
            "default": 8,
            "min": 0,
            "max": 100,
            "step": 0.1,
            "round": 0.01
          }
        ],
        "style": [["regular", "nested"]]
      }
    },
    "input_order": {
      "required": [
        "model",
        "cond1",
        "cond2",
        "negative",
        "cfg_conds",
        "cfg_cond2_negative",
        "style"
      ]
    },
    "output": ["GUIDER"],
    "output_is_list": [false],
    "output_name": ["GUIDER"],
    "name": "DualCFGGuider",
    "display_name": "DualCFGGuider",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/guiders",
    "output_node": false
  },
  "BasicGuider": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "conditioning": ["CONDITIONING"]
      }
    },
    "input_order": {
      "required": ["model", "conditioning"]
    },
    "output": ["GUIDER"],
    "output_is_list": [false],
    "output_name": ["GUIDER"],
    "name": "BasicGuider",
    "display_name": "BasicGuider",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/guiders",
    "output_node": false
  },
  "RandomNoise": {
    "input": {
      "required": {
        "noise_seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["noise_seed"]
    },
    "output": ["NOISE"],
    "output_is_list": [false],
    "output_name": ["NOISE"],
    "name": "RandomNoise",
    "display_name": "RandomNoise",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/noise",
    "output_node": false
  },
  "DisableNoise": {
    "input": {
      "required": {}
    },
    "input_order": {
      "required": []
    },
    "output": ["NOISE"],
    "output_is_list": [false],
    "output_name": ["NOISE"],
    "name": "DisableNoise",
    "display_name": "DisableNoise",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling/noise",
    "output_node": false
  },
  "AddNoise": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "noise": ["NOISE"],
        "sigmas": ["SIGMAS"],
        "latent_image": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["model", "noise", "sigmas", "latent_image"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "AddNoise",
    "display_name": "AddNoise",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "_for_testing/custom_sampling/noise",
    "output_node": false
  },
  "SamplerCustomAdvanced": {
    "input": {
      "required": {
        "noise": ["NOISE"],
        "guider": ["GUIDER"],
        "sampler": ["SAMPLER"],
        "sigmas": ["SIGMAS"],
        "latent_image": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["noise", "guider", "sampler", "sigmas", "latent_image"]
    },
    "output": ["LATENT", "LATENT"],
    "output_is_list": [false, false],
    "output_name": ["output", "denoised_output"],
    "name": "SamplerCustomAdvanced",
    "display_name": "SamplerCustomAdvanced",
    "description": "",
    "python_module": "comfy_extras.nodes_custom_sampler",
    "category": "sampling/custom_sampling",
    "output_node": false
  },
  "HyperTile": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "tile_size": [
          "INT",
          {
            "default": 256,
            "min": 1,
            "max": 2048
          }
        ],
        "swap_size": [
          "INT",
          {
            "default": 2,
            "min": 1,
            "max": 128
          }
        ],
        "max_depth": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 10
          }
        ],
        "scale_depth": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "tile_size",
        "swap_size",
        "max_depth",
        "scale_depth"
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "HyperTile",
    "display_name": "HyperTile",
    "description": "",
    "python_module": "comfy_extras.nodes_hypertile",
    "category": "model_patches/unet",
    "output_node": false
  },
  "ModelSamplingDiscrete": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "sampling": [["eps", "v_prediction", "lcm", "x0", "img_to_img"]],
        "zsnr": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "sampling", "zsnr"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingDiscrete",
    "display_name": "ModelSamplingDiscrete",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "ModelSamplingContinuousEDM": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "sampling": [
          ["v_prediction", "edm", "edm_playground_v2.5", "eps", "cosmos_rflow"]
        ],
        "sigma_max": [
          "FLOAT",
          {
            "default": 120,
            "min": 0,
            "max": 1000,
            "step": 0.001,
            "round": false
          }
        ],
        "sigma_min": [
          "FLOAT",
          {
            "default": 0.002,
            "min": 0,
            "max": 1000,
            "step": 0.001,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "sampling", "sigma_max", "sigma_min"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingContinuousEDM",
    "display_name": "ModelSamplingContinuousEDM",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "ModelSamplingContinuousV": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "sampling": [["v_prediction"]],
        "sigma_max": [
          "FLOAT",
          {
            "default": 500,
            "min": 0,
            "max": 1000,
            "step": 0.001,
            "round": false
          }
        ],
        "sigma_min": [
          "FLOAT",
          {
            "default": 0.03,
            "min": 0,
            "max": 1000,
            "step": 0.001,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "sampling", "sigma_max", "sigma_min"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingContinuousV",
    "display_name": "ModelSamplingContinuousV",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "ModelSamplingStableCascade": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "shift": [
          "FLOAT",
          {
            "default": 2,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "shift"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingStableCascade",
    "display_name": "ModelSamplingStableCascade",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "ModelSamplingSD3": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "shift": [
          "FLOAT",
          {
            "default": 3,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "shift"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingSD3",
    "display_name": "ModelSamplingSD3",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "ModelSamplingAuraFlow": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "shift": [
          "FLOAT",
          {
            "default": 1.73,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "shift"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingAuraFlow",
    "display_name": "ModelSamplingAuraFlow",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "ModelSamplingFlux": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "max_shift": [
          "FLOAT",
          {
            "default": 1.15,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ],
        "base_shift": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ],
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "max_shift", "base_shift", "width", "height"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingFlux",
    "display_name": "ModelSamplingFlux",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "RescaleCFG": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "multiplier": [
          "FLOAT",
          {
            "default": 0.7,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "multiplier"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "RescaleCFG",
    "display_name": "RescaleCFG",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/model",
    "output_node": false
  },
  "ModelComputeDtype": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "dtype": [["default", "fp32", "fp16", "bf16"]]
      }
    },
    "input_order": {
      "required": ["model", "dtype"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelComputeDtype",
    "display_name": "ModelComputeDtype",
    "description": "",
    "python_module": "comfy_extras.nodes_model_advanced",
    "category": "advanced/debug/model",
    "output_node": false
  },
  "PatchModelAddDownscale": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "block_number": [
          "INT",
          {
            "default": 3,
            "min": 1,
            "max": 32,
            "step": 1
          }
        ],
        "downscale_factor": [
          "FLOAT",
          {
            "default": 2,
            "min": 0.1,
            "max": 9,
            "step": 0.001
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 0.35,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "downscale_after_skip": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "downscale_method": [
          ["bicubic", "nearest-exact", "bilinear", "area", "bislerp"]
        ],
        "upscale_method": [
          ["bicubic", "nearest-exact", "bilinear", "area", "bislerp"]
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "block_number",
        "downscale_factor",
        "start_percent",
        "end_percent",
        "downscale_after_skip",
        "downscale_method",
        "upscale_method"
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "PatchModelAddDownscale",
    "display_name": "PatchModelAddDownscale (Kohya Deep Shrink)",
    "description": "",
    "python_module": "comfy_extras.nodes_model_downscale",
    "category": "model_patches/unet",
    "output_node": false
  },
  "ImageCrop": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "width": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "x": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "y": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "width", "height", "x", "y"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageCrop",
    "display_name": "Image Crop",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/transform",
    "output_node": false
  },
  "RepeatImageBatch": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "amount": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "amount"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RepeatImageBatch",
    "display_name": "RepeatImageBatch",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/batch",
    "output_node": false
  },
  "ImageFromBatch": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "batch_index": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 4095
          }
        ],
        "length": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "batch_index", "length"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageFromBatch",
    "display_name": "ImageFromBatch",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/batch",
    "output_node": false
  },
  "ImageAddNoise": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "seed", "strength"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageAddNoise",
    "display_name": "ImageAddNoise",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image",
    "output_node": false
  },
  "SaveAnimatedWEBP": {
    "input": {
      "required": {
        "images": ["IMAGE"],
        "filename_prefix": [
          "STRING",
          {
            "default": "ComfyUI"
          }
        ],
        "fps": [
          "FLOAT",
          {
            "default": 6,
            "min": 0.01,
            "max": 1000,
            "step": 0.01
          }
        ],
        "lossless": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "quality": [
          "INT",
          {
            "default": 80,
            "min": 0,
            "max": 100
          }
        ],
        "method": [["default", "fastest", "slowest"]]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": [
        "images",
        "filename_prefix",
        "fps",
        "lossless",
        "quality",
        "method"
      ],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveAnimatedWEBP",
    "display_name": "SaveAnimatedWEBP",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/animation",
    "output_node": true
  },
  "SaveAnimatedPNG": {
    "input": {
      "required": {
        "images": ["IMAGE"],
        "filename_prefix": [
          "STRING",
          {
            "default": "ComfyUI"
          }
        ],
        "fps": [
          "FLOAT",
          {
            "default": 6,
            "min": 0.01,
            "max": 1000,
            "step": 0.01
          }
        ],
        "compress_level": [
          "INT",
          {
            "default": 4,
            "min": 0,
            "max": 9
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["images", "filename_prefix", "fps", "compress_level"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveAnimatedPNG",
    "display_name": "SaveAnimatedPNG",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/animation",
    "output_node": true
  },
  "SaveSVGNode": {
    "input": {
      "required": {
        "svg": ["SVG"],
        "filename_prefix": [
          "STRING",
          {
            "default": "svg/ComfyUI",
            "tooltip": "The prefix for the file to save. This may include formatting information such as %date:yyyy-MM-dd% or %Empty Latent Image.width% to include values from nodes."
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["svg", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveSVGNode",
    "display_name": "SaveSVGNode",
    "description": "Save SVG files on disk.",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/save",
    "output_node": true
  },
  "ImageStitch": {
    "input": {
      "required": {
        "image1": ["IMAGE"],
        "direction": [
          ["right", "down", "left", "up"],
          {
            "default": "right"
          }
        ],
        "match_image_size": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "spacing_width": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 1024,
            "step": 2
          }
        ],
        "spacing_color": [
          ["white", "black", "red", "green", "blue"],
          {
            "default": "white"
          }
        ]
      },
      "optional": {
        "image2": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "image1",
        "direction",
        "match_image_size",
        "spacing_width",
        "spacing_color"
      ],
      "optional": ["image2"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageStitch",
    "display_name": "Image Stitch",
    "description": "\nStitches image2 to image1 in the specified direction.\nIf image2 is not provided, returns image1 unchanged.\nOptional spacing can be added between images.\n",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/transform",
    "output_node": false
  },
  "ResizeAndPadImage": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "target_width": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "target_height": [
          "INT",
          {
            "default": 512,
            "min": 1,
            "max": 16384,
            "step": 1
          }
        ],
        "padding_color": [["white", "black"]],
        "interpolation": [
          ["area", "bicubic", "nearest-exact", "bilinear", "lanczos"]
        ]
      }
    },
    "input_order": {
      "required": [
        "image",
        "target_width",
        "target_height",
        "padding_color",
        "interpolation"
      ]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ResizeAndPadImage",
    "display_name": "ResizeAndPadImage",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/transform",
    "output_node": false
  },
  "GetImageSize": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "hidden": {
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["image"],
      "hidden": ["unique_id"]
    },
    "output": ["INT", "INT", "INT"],
    "output_is_list": [false, false, false],
    "output_name": ["width", "height", "batch_size"],
    "name": "GetImageSize",
    "display_name": "Get Image Size",
    "description": "Returns width and height of the image, and passes it through unchanged.",
    "python_module": "comfy_extras.nodes_images",
    "category": "image",
    "output_node": false
  },
  "ImageRotate": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "rotation": [["none", "90 degrees", "180 degrees", "270 degrees"]]
      }
    },
    "input_order": {
      "required": ["image", "rotation"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageRotate",
    "display_name": "ImageRotate",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/transform",
    "output_node": false
  },
  "ImageFlip": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "flip_method": [["x-axis: vertically", "y-axis: horizontally"]]
      }
    },
    "input_order": {
      "required": ["image", "flip_method"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageFlip",
    "display_name": "ImageFlip",
    "description": "",
    "python_module": "comfy_extras.nodes_images",
    "category": "image/transform",
    "output_node": false
  },
  "ImageOnlyCheckpointLoader": {
    "input": {
      "required": {
        "ckpt_name": [[]]
      }
    },
    "input_order": {
      "required": ["ckpt_name"]
    },
    "output": ["MODEL", "CLIP_VISION", "VAE"],
    "output_is_list": [false, false, false],
    "output_name": ["MODEL", "CLIP_VISION", "VAE"],
    "name": "ImageOnlyCheckpointLoader",
    "display_name": "Image Only Checkpoint Loader (img2vid model)",
    "description": "",
    "python_module": "comfy_extras.nodes_video_model",
    "category": "loaders/video_models",
    "output_node": false
  },
  "SVD_img2vid_Conditioning": {
    "input": {
      "required": {
        "clip_vision": ["CLIP_VISION"],
        "init_image": ["IMAGE"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 576,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "video_frames": [
          "INT",
          {
            "default": 14,
            "min": 1,
            "max": 4096
          }
        ],
        "motion_bucket_id": [
          "INT",
          {
            "default": 127,
            "min": 1,
            "max": 1023
          }
        ],
        "fps": [
          "INT",
          {
            "default": 6,
            "min": 1,
            "max": 1024
          }
        ],
        "augmentation_level": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "clip_vision",
        "init_image",
        "vae",
        "width",
        "height",
        "video_frames",
        "motion_bucket_id",
        "fps",
        "augmentation_level"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "SVD_img2vid_Conditioning",
    "display_name": "SVD_img2vid_Conditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_video_model",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "VideoLinearCFGGuidance": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "min_cfg": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.5,
            "round": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "min_cfg"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "VideoLinearCFGGuidance",
    "display_name": "VideoLinearCFGGuidance",
    "description": "",
    "python_module": "comfy_extras.nodes_video_model",
    "category": "sampling/video_models",
    "output_node": false
  },
  "VideoTriangleCFGGuidance": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "min_cfg": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.5,
            "round": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "min_cfg"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "VideoTriangleCFGGuidance",
    "display_name": "VideoTriangleCFGGuidance",
    "description": "",
    "python_module": "comfy_extras.nodes_video_model",
    "category": "sampling/video_models",
    "output_node": false
  },
  "ImageOnlyCheckpointSave": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "clip_vision": ["CLIP_VISION"],
        "vae": ["VAE"],
        "filename_prefix": [
          "STRING",
          {
            "default": "checkpoints/ComfyUI"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["model", "clip_vision", "vae", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "ImageOnlyCheckpointSave",
    "display_name": "ImageOnlyCheckpointSave",
    "description": "",
    "python_module": "comfy_extras.nodes_video_model",
    "category": "advanced/model_merging",
    "output_node": true
  },
  "ConditioningSetAreaPercentageVideo": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "width": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "height": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "temporal": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "y": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "z": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "conditioning",
        "width",
        "height",
        "temporal",
        "x",
        "y",
        "z",
        "strength"
      ]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetAreaPercentageVideo",
    "display_name": "ConditioningSetAreaPercentageVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_video_model",
    "category": "conditioning",
    "output_node": false
  },
  "TrainLoraNode": {
    "input": {
      "required": {
        "model": [
          "MODEL",
          {
            "tooltip": "The model to train the LoRA on."
          }
        ],
        "latents": [
          "LATENT",
          {
            "tooltip": "The Latents to use for training, serve as dataset/input of the model."
          }
        ],
        "positive": [
          "CONDITIONING",
          {
            "tooltip": "The positive conditioning to use for training."
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 10000,
            "step": 1,
            "tooltip": "The batch size to use for training."
          }
        ],
        "grad_accumulation_steps": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 1024,
            "step": 1,
            "tooltip": "The number of gradient accumulation steps to use for training."
          }
        ],
        "steps": [
          "INT",
          {
            "default": 16,
            "min": 1,
            "max": 100000,
            "tooltip": "The number of steps to train the LoRA for."
          }
        ],
        "learning_rate": [
          "FLOAT",
          {
            "default": 0.0005,
            "min": 1e-7,
            "max": 1,
            "step": 0.000001,
            "tooltip": "The learning rate to use for training."
          }
        ],
        "rank": [
          "INT",
          {
            "default": 8,
            "min": 1,
            "max": 128,
            "tooltip": "The rank of the LoRA layers."
          }
        ],
        "optimizer": [
          ["AdamW", "Adam", "SGD", "RMSprop"],
          {
            "default": "AdamW",
            "tooltip": "The optimizer to use for training."
          }
        ],
        "loss_function": [
          ["MSE", "L1", "Huber", "SmoothL1"],
          {
            "default": "MSE",
            "tooltip": "The loss function to use for training."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "tooltip": "The seed to use for training (used in generator for LoRA weight initialization and noise sampling)"
          }
        ],
        "training_dtype": [
          ["bf16", "fp32"],
          {
            "default": "bf16",
            "tooltip": "The dtype to use for training."
          }
        ],
        "lora_dtype": [
          ["bf16", "fp32"],
          {
            "default": "bf16",
            "tooltip": "The dtype to use for lora."
          }
        ],
        "algorithm": [
          ["LoRA", "LoHa", "LoKr", "OFT"],
          {
            "default": "LoRA",
            "tooltip": "The algorithm to use for training."
          }
        ],
        "gradient_checkpointing": [
          "BOOLEAN",
          {
            "default": true,
            "tooltip": "Use gradient checkpointing for training."
          }
        ],
        "existing_lora": [
          ["[None]"],
          {
            "default": "[None]",
            "tooltip": "The existing LoRA to append to. Set to None for new LoRA."
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "latents",
        "positive",
        "batch_size",
        "grad_accumulation_steps",
        "steps",
        "learning_rate",
        "rank",
        "optimizer",
        "loss_function",
        "seed",
        "training_dtype",
        "lora_dtype",
        "algorithm",
        "gradient_checkpointing",
        "existing_lora"
      ]
    },
    "output": ["MODEL", "LORA_MODEL", "LOSS_MAP", "INT"],
    "output_is_list": [false, false, false, false],
    "output_name": ["model_with_lora", "lora", "loss", "steps"],
    "name": "TrainLoraNode",
    "display_name": "Train LoRA",
    "description": "",
    "python_module": "comfy_extras.nodes_train",
    "category": "training",
    "output_node": false,
    "experimental": true
  },
  "SaveLoRANode": {
    "input": {
      "required": {
        "lora": [
          "LORA_MODEL",
          {
            "tooltip": "The LoRA model to save. Do not use the model with LoRA layers."
          }
        ],
        "prefix": [
          "STRING",
          {
            "default": "loras/ComfyUI_trained_lora",
            "tooltip": "The prefix to use for the saved LoRA file."
          }
        ]
      },
      "optional": {
        "steps": [
          "INT",
          {
            "forceInput": true,
            "tooltip": "Optional: The number of steps to LoRA has been trained for, used to name the saved file."
          }
        ]
      }
    },
    "input_order": {
      "required": ["lora", "prefix"],
      "optional": ["steps"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveLoRANode",
    "display_name": "Save LoRA Weights",
    "description": "",
    "python_module": "comfy_extras.nodes_train",
    "category": "loaders",
    "output_node": true,
    "experimental": true
  },
  "LoraModelLoader": {
    "input": {
      "required": {
        "model": [
          "MODEL",
          {
            "tooltip": "The diffusion model the LoRA will be applied to."
          }
        ],
        "lora": [
          "LORA_MODEL",
          {
            "tooltip": "The LoRA model to apply to the diffusion model."
          }
        ],
        "strength_model": [
          "FLOAT",
          {
            "default": 1,
            "min": -100,
            "max": 100,
            "step": 0.01,
            "tooltip": "How strongly to modify the diffusion model. This value can be negative."
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "lora", "strength_model"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "LoraModelLoader",
    "display_name": "Load LoRA Model",
    "description": "Load Trained LoRA weights from Train LoRA node.",
    "python_module": "comfy_extras.nodes_train",
    "category": "loaders",
    "output_node": false,
    "output_tooltips": ["The modified diffusion model."],
    "experimental": true
  },
  "LoadImageSetFromFolderNode": {
    "input": {
      "required": {
        "folder": [
          ["3d"],
          {
            "tooltip": "The folder to load images from."
          }
        ]
      },
      "optional": {
        "resize_method": [
          ["None", "Stretch", "Crop", "Pad"],
          {
            "default": "None"
          }
        ]
      }
    },
    "input_order": {
      "required": ["folder"],
      "optional": ["resize_method"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "LoadImageSetFromFolderNode",
    "display_name": "Load Image Dataset from Folder",
    "description": "Loads a batch of images from a directory for training.",
    "python_module": "comfy_extras.nodes_train",
    "category": "loaders",
    "output_node": false,
    "experimental": true
  },
  "LoadImageTextSetFromFolderNode": {
    "input": {
      "required": {
        "folder": [
          ["3d"],
          {
            "tooltip": "The folder to load images from."
          }
        ],
        "clip": [
          "CLIP",
          {
            "tooltip": "The CLIP model used for encoding the text."
          }
        ]
      },
      "optional": {
        "resize_method": [
          ["None", "Stretch", "Crop", "Pad"],
          {
            "default": "None"
          }
        ],
        "width": [
          "INT",
          {
            "default": -1,
            "min": -1,
            "max": 10000,
            "step": 1,
            "tooltip": "The width to resize the images to. -1 means use the original width."
          }
        ],
        "height": [
          "INT",
          {
            "default": -1,
            "min": -1,
            "max": 10000,
            "step": 1,
            "tooltip": "The height to resize the images to. -1 means use the original height."
          }
        ]
      }
    },
    "input_order": {
      "required": ["folder", "clip"],
      "optional": ["resize_method", "width", "height"]
    },
    "output": ["IMAGE", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "CONDITIONING"],
    "name": "LoadImageTextSetFromFolderNode",
    "display_name": "Load Image and Text Dataset from Folder",
    "description": "Loads a batch of images and caption from a directory for training.",
    "python_module": "comfy_extras.nodes_train",
    "category": "loaders",
    "output_node": false,
    "experimental": true
  },
  "LossGraphNode": {
    "input": {
      "required": {
        "loss": [
          "LOSS_MAP",
          {
            "default": {}
          }
        ],
        "filename_prefix": [
          "STRING",
          {
            "default": "loss_graph"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["loss", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "LossGraphNode",
    "display_name": "Plot Loss Graph",
    "description": "Plots the loss graph and saves it to the output directory.",
    "python_module": "comfy_extras.nodes_train",
    "category": "training",
    "output_node": true,
    "experimental": true
  },
  "SelfAttentionGuidance": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "scale": [
          "FLOAT",
          {
            "default": 0.5,
            "min": -2,
            "max": 5,
            "step": 0.01
          }
        ],
        "blur_sigma": [
          "FLOAT",
          {
            "default": 2,
            "min": 0,
            "max": 10,
            "step": 0.1
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "scale", "blur_sigma"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "SelfAttentionGuidance",
    "display_name": "Self-Attention Guidance",
    "description": "",
    "python_module": "comfy_extras.nodes_sag",
    "category": "_for_testing",
    "output_node": false
  },
  "PerpNeg": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "empty_conditioning": ["CONDITIONING"],
        "neg_scale": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "empty_conditioning", "neg_scale"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "PerpNeg",
    "display_name": "Perp-Neg (DEPRECATED by PerpNegGuider)",
    "description": "",
    "python_module": "comfy_extras.nodes_perpneg",
    "category": "_for_testing",
    "output_node": false,
    "deprecated": true
  },
  "PerpNegGuider": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "empty_conditioning": ["CONDITIONING"],
        "cfg": [
          "FLOAT",
          {
            "default": 8,
            "min": 0,
            "max": 100,
            "step": 0.1,
            "round": 0.01
          }
        ],
        "neg_scale": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "positive",
        "negative",
        "empty_conditioning",
        "cfg",
        "neg_scale"
      ]
    },
    "output": ["GUIDER"],
    "output_is_list": [false],
    "output_name": ["GUIDER"],
    "name": "PerpNegGuider",
    "display_name": "PerpNegGuider",
    "description": "",
    "python_module": "comfy_extras.nodes_perpneg",
    "category": "_for_testing",
    "output_node": false
  },
  "StableZero123_Conditioning": {
    "input": {
      "required": {
        "clip_vision": ["CLIP_VISION"],
        "init_image": ["IMAGE"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 256,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 256,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ],
        "elevation": [
          "FLOAT",
          {
            "default": 0,
            "min": -180,
            "max": 180,
            "step": 0.1,
            "round": false
          }
        ],
        "azimuth": [
          "FLOAT",
          {
            "default": 0,
            "min": -180,
            "max": 180,
            "step": 0.1,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "clip_vision",
        "init_image",
        "vae",
        "width",
        "height",
        "batch_size",
        "elevation",
        "azimuth"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "StableZero123_Conditioning",
    "display_name": "StableZero123_Conditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_stable3d",
    "category": "conditioning/3d_models",
    "output_node": false
  },
  "StableZero123_Conditioning_Batched": {
    "input": {
      "required": {
        "clip_vision": ["CLIP_VISION"],
        "init_image": ["IMAGE"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 256,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 256,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ],
        "elevation": [
          "FLOAT",
          {
            "default": 0,
            "min": -180,
            "max": 180,
            "step": 0.1,
            "round": false
          }
        ],
        "azimuth": [
          "FLOAT",
          {
            "default": 0,
            "min": -180,
            "max": 180,
            "step": 0.1,
            "round": false
          }
        ],
        "elevation_batch_increment": [
          "FLOAT",
          {
            "default": 0,
            "min": -180,
            "max": 180,
            "step": 0.1,
            "round": false
          }
        ],
        "azimuth_batch_increment": [
          "FLOAT",
          {
            "default": 0,
            "min": -180,
            "max": 180,
            "step": 0.1,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "clip_vision",
        "init_image",
        "vae",
        "width",
        "height",
        "batch_size",
        "elevation",
        "azimuth",
        "elevation_batch_increment",
        "azimuth_batch_increment"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "StableZero123_Conditioning_Batched",
    "display_name": "StableZero123_Conditioning_Batched",
    "description": "",
    "python_module": "comfy_extras.nodes_stable3d",
    "category": "conditioning/3d_models",
    "output_node": false
  },
  "SV3D_Conditioning": {
    "input": {
      "required": {
        "clip_vision": ["CLIP_VISION"],
        "init_image": ["IMAGE"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 576,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 576,
            "min": 16,
            "max": 16384,
            "step": 8
          }
        ],
        "video_frames": [
          "INT",
          {
            "default": 21,
            "min": 1,
            "max": 4096
          }
        ],
        "elevation": [
          "FLOAT",
          {
            "default": 0,
            "min": -90,
            "max": 90,
            "step": 0.1,
            "round": false
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "clip_vision",
        "init_image",
        "vae",
        "width",
        "height",
        "video_frames",
        "elevation"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "SV3D_Conditioning",
    "display_name": "SV3D_Conditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_stable3d",
    "category": "conditioning/3d_models",
    "output_node": false
  },
  "SD_4XUpscale_Conditioning": {
    "input": {
      "required": {
        "images": ["IMAGE"],
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "scale_ratio": [
          "FLOAT",
          {
            "default": 4,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "noise_augmentation": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "images",
        "positive",
        "negative",
        "scale_ratio",
        "noise_augmentation"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "SD_4XUpscale_Conditioning",
    "display_name": "SD_4XUpscale_Conditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_sdupscale",
    "category": "conditioning/upscale_diffusion",
    "output_node": false
  },
  "PhotoMakerLoader": {
    "input": {
      "required": {
        "photomaker_model_name": [[]]
      }
    },
    "input_order": {
      "required": ["photomaker_model_name"]
    },
    "output": ["PHOTOMAKER"],
    "output_is_list": [false],
    "output_name": ["PHOTOMAKER"],
    "name": "PhotoMakerLoader",
    "display_name": "PhotoMakerLoader",
    "description": "",
    "python_module": "comfy_extras.nodes_photomaker",
    "category": "_for_testing/photomaker",
    "output_node": false
  },
  "PhotoMakerEncode": {
    "input": {
      "required": {
        "photomaker": ["PHOTOMAKER"],
        "image": ["IMAGE"],
        "clip": ["CLIP"],
        "text": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true,
            "default": "photograph of photomaker"
          }
        ]
      }
    },
    "input_order": {
      "required": ["photomaker", "image", "clip", "text"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "PhotoMakerEncode",
    "display_name": "PhotoMakerEncode",
    "description": "",
    "python_module": "comfy_extras.nodes_photomaker",
    "category": "_for_testing/photomaker",
    "output_node": false
  },
  "CLIPTextEncodePixArtAlpha": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 0,
            "max": 16384
          }
        ],
        "text": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "clip": ["CLIP"]
      }
    },
    "input_order": {
      "required": ["width", "height", "text", "clip"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodePixArtAlpha",
    "display_name": "CLIPTextEncodePixArtAlpha",
    "description": "Encodes text and sets the resolution conditioning for PixArt Alpha. Does not apply to PixArt Sigma.",
    "python_module": "comfy_extras.nodes_pixart",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "CLIPTextEncodeControlnet": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "conditioning": ["CONDITIONING"],
        "text": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "conditioning", "text"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeControlnet",
    "display_name": "CLIPTextEncodeControlnet",
    "description": "",
    "python_module": "comfy_extras.nodes_cond",
    "category": "_for_testing/conditioning",
    "output_node": false
  },
  "T5TokenizerOptions": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "min_padding": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 10000,
            "step": 1
          }
        ],
        "min_length": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 10000,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "min_padding", "min_length"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "T5TokenizerOptions",
    "display_name": "T5TokenizerOptions",
    "description": "",
    "python_module": "comfy_extras.nodes_cond",
    "category": "_for_testing/conditioning",
    "output_node": false
  },
  "Morphology": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "operation": [
          [
            "erode",
            "dilate",
            "open",
            "close",
            "gradient",
            "bottom_hat",
            "top_hat"
          ]
        ],
        "kernel_size": [
          "INT",
          {
            "default": 3,
            "min": 3,
            "max": 999,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "operation", "kernel_size"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "Morphology",
    "display_name": "ImageMorphology",
    "description": "",
    "python_module": "comfy_extras.nodes_morphology",
    "category": "image/postprocessing",
    "output_node": false
  },
  "ImageRGBToYUV": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["image"]
    },
    "output": ["IMAGE", "IMAGE", "IMAGE"],
    "output_is_list": [false, false, false],
    "output_name": ["Y", "U", "V"],
    "name": "ImageRGBToYUV",
    "display_name": "ImageRGBToYUV",
    "description": "",
    "python_module": "comfy_extras.nodes_morphology",
    "category": "image/batch",
    "output_node": false
  },
  "ImageYUVToRGB": {
    "input": {
      "required": {
        "Y": ["IMAGE"],
        "U": ["IMAGE"],
        "V": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["Y", "U", "V"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "ImageYUVToRGB",
    "display_name": "ImageYUVToRGB",
    "description": "",
    "python_module": "comfy_extras.nodes_morphology",
    "category": "image/batch",
    "output_node": false
  },
  "StableCascade_EmptyLatentImage": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 256,
            "max": 16384,
            "step": 8
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 256,
            "max": 16384,
            "step": 8
          }
        ],
        "compression": [
          "INT",
          {
            "default": 42,
            "min": 4,
            "max": 128,
            "step": 1
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "compression", "batch_size"]
    },
    "output": ["LATENT", "LATENT"],
    "output_is_list": [false, false],
    "output_name": ["stage_c", "stage_b"],
    "name": "StableCascade_EmptyLatentImage",
    "display_name": "StableCascade_EmptyLatentImage",
    "description": "",
    "python_module": "comfy_extras.nodes_stable_cascade",
    "category": "latent/stable_cascade",
    "output_node": false
  },
  "StableCascade_StageB_Conditioning": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "stage_c": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["conditioning", "stage_c"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "StableCascade_StageB_Conditioning",
    "display_name": "StableCascade_StageB_Conditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_stable_cascade",
    "category": "conditioning/stable_cascade",
    "output_node": false
  },
  "StableCascade_StageC_VAEEncode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "vae": ["VAE"],
        "compression": [
          "INT",
          {
            "default": 42,
            "min": 4,
            "max": 128,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "vae", "compression"]
    },
    "output": ["LATENT", "LATENT"],
    "output_is_list": [false, false],
    "output_name": ["stage_c", "stage_b"],
    "name": "StableCascade_StageC_VAEEncode",
    "display_name": "StableCascade_StageC_VAEEncode",
    "description": "",
    "python_module": "comfy_extras.nodes_stable_cascade",
    "category": "latent/stable_cascade",
    "output_node": false
  },
  "StableCascade_SuperResolutionControlnet": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "vae": ["VAE"]
      }
    },
    "input_order": {
      "required": ["image", "vae"]
    },
    "output": ["IMAGE", "LATENT", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["controlnet_input", "stage_c", "stage_b"],
    "name": "StableCascade_SuperResolutionControlnet",
    "display_name": "StableCascade_SuperResolutionControlnet",
    "description": "",
    "python_module": "comfy_extras.nodes_stable_cascade",
    "category": "_for_testing/stable_cascade",
    "output_node": false,
    "experimental": true
  },
  "DifferentialDiffusion": {
    "input": {
      "required": {
        "model": ["MODEL"]
      }
    },
    "input_order": {
      "required": ["model"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "DifferentialDiffusion",
    "display_name": "Differential Diffusion",
    "description": "",
    "python_module": "comfy_extras.nodes_differential_diffusion",
    "category": "_for_testing",
    "output_node": false
  },
  "InstructPixToPixConditioning": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "pixels": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["positive", "negative", "vae", "pixels"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "InstructPixToPixConditioning",
    "display_name": "InstructPixToPixConditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_ip2p",
    "category": "conditioning/instructpix2pix",
    "output_node": false
  },
  "ModelMergeSD1": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "time_embed.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "label_emb.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "out.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "time_embed.",
        "label_emb.",
        "input_blocks.0.",
        "input_blocks.1.",
        "input_blocks.2.",
        "input_blocks.3.",
        "input_blocks.4.",
        "input_blocks.5.",
        "input_blocks.6.",
        "input_blocks.7.",
        "input_blocks.8.",
        "input_blocks.9.",
        "input_blocks.10.",
        "input_blocks.11.",
        "middle_block.0.",
        "middle_block.1.",
        "middle_block.2.",
        "output_blocks.0.",
        "output_blocks.1.",
        "output_blocks.2.",
        "output_blocks.3.",
        "output_blocks.4.",
        "output_blocks.5.",
        "output_blocks.6.",
        "output_blocks.7.",
        "output_blocks.8.",
        "output_blocks.9.",
        "output_blocks.10.",
        "output_blocks.11.",
        "out."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeSD1",
    "display_name": "ModelMergeSD1",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeSD2": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "time_embed.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "label_emb.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "out.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "time_embed.",
        "label_emb.",
        "input_blocks.0.",
        "input_blocks.1.",
        "input_blocks.2.",
        "input_blocks.3.",
        "input_blocks.4.",
        "input_blocks.5.",
        "input_blocks.6.",
        "input_blocks.7.",
        "input_blocks.8.",
        "input_blocks.9.",
        "input_blocks.10.",
        "input_blocks.11.",
        "middle_block.0.",
        "middle_block.1.",
        "middle_block.2.",
        "output_blocks.0.",
        "output_blocks.1.",
        "output_blocks.2.",
        "output_blocks.3.",
        "output_blocks.4.",
        "output_blocks.5.",
        "output_blocks.6.",
        "output_blocks.7.",
        "output_blocks.8.",
        "output_blocks.9.",
        "output_blocks.10.",
        "output_blocks.11.",
        "out."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeSD2",
    "display_name": "ModelMergeSD2",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeSDXL": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "time_embed.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "label_emb.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.0": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.1": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.2": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.3": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.4": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.5": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.6": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.7": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "input_blocks.8": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.0": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.1": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "middle_block.2": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.0": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.1": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.2": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.3": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.4": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.5": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.6": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.7": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "output_blocks.8": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "out.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "time_embed.",
        "label_emb.",
        "input_blocks.0",
        "input_blocks.1",
        "input_blocks.2",
        "input_blocks.3",
        "input_blocks.4",
        "input_blocks.5",
        "input_blocks.6",
        "input_blocks.7",
        "input_blocks.8",
        "middle_block.0",
        "middle_block.1",
        "middle_block.2",
        "output_blocks.0",
        "output_blocks.1",
        "output_blocks.2",
        "output_blocks.3",
        "output_blocks.4",
        "output_blocks.5",
        "output_blocks.6",
        "output_blocks.7",
        "output_blocks.8",
        "out."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeSDXL",
    "display_name": "ModelMergeSDXL",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeSD3_2B": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "pos_embed.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "context_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "y_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "pos_embed.",
        "x_embedder.",
        "context_embedder.",
        "y_embedder.",
        "t_embedder.",
        "joint_blocks.0.",
        "joint_blocks.1.",
        "joint_blocks.2.",
        "joint_blocks.3.",
        "joint_blocks.4.",
        "joint_blocks.5.",
        "joint_blocks.6.",
        "joint_blocks.7.",
        "joint_blocks.8.",
        "joint_blocks.9.",
        "joint_blocks.10.",
        "joint_blocks.11.",
        "joint_blocks.12.",
        "joint_blocks.13.",
        "joint_blocks.14.",
        "joint_blocks.15.",
        "joint_blocks.16.",
        "joint_blocks.17.",
        "joint_blocks.18.",
        "joint_blocks.19.",
        "joint_blocks.20.",
        "joint_blocks.21.",
        "joint_blocks.22.",
        "joint_blocks.23.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeSD3_2B",
    "display_name": "ModelMergeSD3_2B",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeAuraflow": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "init_x_linear.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "positional_encoding": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "cond_seq_linear.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "register_tokens": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_layers.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_layers.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_layers.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_layers.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.28.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.29.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.30.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_layers.31.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "modF.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_linear.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "init_x_linear.",
        "positional_encoding",
        "cond_seq_linear.",
        "register_tokens",
        "t_embedder.",
        "double_layers.0.",
        "double_layers.1.",
        "double_layers.2.",
        "double_layers.3.",
        "single_layers.0.",
        "single_layers.1.",
        "single_layers.2.",
        "single_layers.3.",
        "single_layers.4.",
        "single_layers.5.",
        "single_layers.6.",
        "single_layers.7.",
        "single_layers.8.",
        "single_layers.9.",
        "single_layers.10.",
        "single_layers.11.",
        "single_layers.12.",
        "single_layers.13.",
        "single_layers.14.",
        "single_layers.15.",
        "single_layers.16.",
        "single_layers.17.",
        "single_layers.18.",
        "single_layers.19.",
        "single_layers.20.",
        "single_layers.21.",
        "single_layers.22.",
        "single_layers.23.",
        "single_layers.24.",
        "single_layers.25.",
        "single_layers.26.",
        "single_layers.27.",
        "single_layers.28.",
        "single_layers.29.",
        "single_layers.30.",
        "single_layers.31.",
        "modF.",
        "final_linear."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeAuraflow",
    "display_name": "ModelMergeAuraflow",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeFlux1": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "img_in.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "time_in.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "guidance_in": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "vector_in.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "txt_in.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "double_blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.28.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.29.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.30.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.31.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.32.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.33.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.34.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.35.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.36.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "single_blocks.37.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "img_in.",
        "time_in.",
        "guidance_in",
        "vector_in.",
        "txt_in.",
        "double_blocks.0.",
        "double_blocks.1.",
        "double_blocks.2.",
        "double_blocks.3.",
        "double_blocks.4.",
        "double_blocks.5.",
        "double_blocks.6.",
        "double_blocks.7.",
        "double_blocks.8.",
        "double_blocks.9.",
        "double_blocks.10.",
        "double_blocks.11.",
        "double_blocks.12.",
        "double_blocks.13.",
        "double_blocks.14.",
        "double_blocks.15.",
        "double_blocks.16.",
        "double_blocks.17.",
        "double_blocks.18.",
        "single_blocks.0.",
        "single_blocks.1.",
        "single_blocks.2.",
        "single_blocks.3.",
        "single_blocks.4.",
        "single_blocks.5.",
        "single_blocks.6.",
        "single_blocks.7.",
        "single_blocks.8.",
        "single_blocks.9.",
        "single_blocks.10.",
        "single_blocks.11.",
        "single_blocks.12.",
        "single_blocks.13.",
        "single_blocks.14.",
        "single_blocks.15.",
        "single_blocks.16.",
        "single_blocks.17.",
        "single_blocks.18.",
        "single_blocks.19.",
        "single_blocks.20.",
        "single_blocks.21.",
        "single_blocks.22.",
        "single_blocks.23.",
        "single_blocks.24.",
        "single_blocks.25.",
        "single_blocks.26.",
        "single_blocks.27.",
        "single_blocks.28.",
        "single_blocks.29.",
        "single_blocks.30.",
        "single_blocks.31.",
        "single_blocks.32.",
        "single_blocks.33.",
        "single_blocks.34.",
        "single_blocks.35.",
        "single_blocks.36.",
        "single_blocks.37.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeFlux1",
    "display_name": "ModelMergeFlux1",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeSD35_Large": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "pos_embed.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "context_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "y_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.28.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.29.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.30.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.31.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.32.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.33.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.34.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.35.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.36.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "joint_blocks.37.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "pos_embed.",
        "x_embedder.",
        "context_embedder.",
        "y_embedder.",
        "t_embedder.",
        "joint_blocks.0.",
        "joint_blocks.1.",
        "joint_blocks.2.",
        "joint_blocks.3.",
        "joint_blocks.4.",
        "joint_blocks.5.",
        "joint_blocks.6.",
        "joint_blocks.7.",
        "joint_blocks.8.",
        "joint_blocks.9.",
        "joint_blocks.10.",
        "joint_blocks.11.",
        "joint_blocks.12.",
        "joint_blocks.13.",
        "joint_blocks.14.",
        "joint_blocks.15.",
        "joint_blocks.16.",
        "joint_blocks.17.",
        "joint_blocks.18.",
        "joint_blocks.19.",
        "joint_blocks.20.",
        "joint_blocks.21.",
        "joint_blocks.22.",
        "joint_blocks.23.",
        "joint_blocks.24.",
        "joint_blocks.25.",
        "joint_blocks.26.",
        "joint_blocks.27.",
        "joint_blocks.28.",
        "joint_blocks.29.",
        "joint_blocks.30.",
        "joint_blocks.31.",
        "joint_blocks.32.",
        "joint_blocks.33.",
        "joint_blocks.34.",
        "joint_blocks.35.",
        "joint_blocks.36.",
        "joint_blocks.37.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeSD35_Large",
    "display_name": "ModelMergeSD35_Large",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeMochiPreview": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "pos_frequencies.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t5_y_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t5_yproj.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.28.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.29.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.30.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.31.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.32.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.33.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.34.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.35.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.36.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.37.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.38.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.39.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.40.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.41.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.42.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.43.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.44.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.45.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.46.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.47.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "pos_frequencies.",
        "t_embedder.",
        "t5_y_embedder.",
        "t5_yproj.",
        "blocks.0.",
        "blocks.1.",
        "blocks.2.",
        "blocks.3.",
        "blocks.4.",
        "blocks.5.",
        "blocks.6.",
        "blocks.7.",
        "blocks.8.",
        "blocks.9.",
        "blocks.10.",
        "blocks.11.",
        "blocks.12.",
        "blocks.13.",
        "blocks.14.",
        "blocks.15.",
        "blocks.16.",
        "blocks.17.",
        "blocks.18.",
        "blocks.19.",
        "blocks.20.",
        "blocks.21.",
        "blocks.22.",
        "blocks.23.",
        "blocks.24.",
        "blocks.25.",
        "blocks.26.",
        "blocks.27.",
        "blocks.28.",
        "blocks.29.",
        "blocks.30.",
        "blocks.31.",
        "blocks.32.",
        "blocks.33.",
        "blocks.34.",
        "blocks.35.",
        "blocks.36.",
        "blocks.37.",
        "blocks.38.",
        "blocks.39.",
        "blocks.40.",
        "blocks.41.",
        "blocks.42.",
        "blocks.43.",
        "blocks.44.",
        "blocks.45.",
        "blocks.46.",
        "blocks.47.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeMochiPreview",
    "display_name": "ModelMergeMochiPreview",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeLTXV": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "patchify_proj.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "adaln_single.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "caption_projection.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "transformer_blocks.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "scale_shift_table": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "proj_out.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "patchify_proj.",
        "adaln_single.",
        "caption_projection.",
        "transformer_blocks.0.",
        "transformer_blocks.1.",
        "transformer_blocks.2.",
        "transformer_blocks.3.",
        "transformer_blocks.4.",
        "transformer_blocks.5.",
        "transformer_blocks.6.",
        "transformer_blocks.7.",
        "transformer_blocks.8.",
        "transformer_blocks.9.",
        "transformer_blocks.10.",
        "transformer_blocks.11.",
        "transformer_blocks.12.",
        "transformer_blocks.13.",
        "transformer_blocks.14.",
        "transformer_blocks.15.",
        "transformer_blocks.16.",
        "transformer_blocks.17.",
        "transformer_blocks.18.",
        "transformer_blocks.19.",
        "transformer_blocks.20.",
        "transformer_blocks.21.",
        "transformer_blocks.22.",
        "transformer_blocks.23.",
        "transformer_blocks.24.",
        "transformer_blocks.25.",
        "transformer_blocks.26.",
        "transformer_blocks.27.",
        "scale_shift_table",
        "proj_out."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeLTXV",
    "display_name": "ModelMergeLTXV",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeCosmos7B": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "pos_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "extra_pos_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "affline_norm.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "pos_embedder.",
        "extra_pos_embedder.",
        "x_embedder.",
        "t_embedder.",
        "affline_norm.",
        "blocks.block0.",
        "blocks.block1.",
        "blocks.block2.",
        "blocks.block3.",
        "blocks.block4.",
        "blocks.block5.",
        "blocks.block6.",
        "blocks.block7.",
        "blocks.block8.",
        "blocks.block9.",
        "blocks.block10.",
        "blocks.block11.",
        "blocks.block12.",
        "blocks.block13.",
        "blocks.block14.",
        "blocks.block15.",
        "blocks.block16.",
        "blocks.block17.",
        "blocks.block18.",
        "blocks.block19.",
        "blocks.block20.",
        "blocks.block21.",
        "blocks.block22.",
        "blocks.block23.",
        "blocks.block24.",
        "blocks.block25.",
        "blocks.block26.",
        "blocks.block27.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeCosmos7B",
    "display_name": "ModelMergeCosmos7B",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeCosmos14B": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "pos_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "extra_pos_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "affline_norm.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block28.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block29.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block30.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block31.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block32.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block33.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block34.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.block35.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "pos_embedder.",
        "extra_pos_embedder.",
        "x_embedder.",
        "t_embedder.",
        "affline_norm.",
        "blocks.block0.",
        "blocks.block1.",
        "blocks.block2.",
        "blocks.block3.",
        "blocks.block4.",
        "blocks.block5.",
        "blocks.block6.",
        "blocks.block7.",
        "blocks.block8.",
        "blocks.block9.",
        "blocks.block10.",
        "blocks.block11.",
        "blocks.block12.",
        "blocks.block13.",
        "blocks.block14.",
        "blocks.block15.",
        "blocks.block16.",
        "blocks.block17.",
        "blocks.block18.",
        "blocks.block19.",
        "blocks.block20.",
        "blocks.block21.",
        "blocks.block22.",
        "blocks.block23.",
        "blocks.block24.",
        "blocks.block25.",
        "blocks.block26.",
        "blocks.block27.",
        "blocks.block28.",
        "blocks.block29.",
        "blocks.block30.",
        "blocks.block31.",
        "blocks.block32.",
        "blocks.block33.",
        "blocks.block34.",
        "blocks.block35.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeCosmos14B",
    "display_name": "ModelMergeCosmos14B",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeWAN2_1": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "patch_embedding.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "time_embedding.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "time_projection.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "text_embedding.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "img_emb.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.28.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.29.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.30.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.31.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.32.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.33.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.34.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.35.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.36.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.37.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.38.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.39.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "head.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "patch_embedding.",
        "time_embedding.",
        "time_projection.",
        "text_embedding.",
        "img_emb.",
        "blocks.0.",
        "blocks.1.",
        "blocks.2.",
        "blocks.3.",
        "blocks.4.",
        "blocks.5.",
        "blocks.6.",
        "blocks.7.",
        "blocks.8.",
        "blocks.9.",
        "blocks.10.",
        "blocks.11.",
        "blocks.12.",
        "blocks.13.",
        "blocks.14.",
        "blocks.15.",
        "blocks.16.",
        "blocks.17.",
        "blocks.18.",
        "blocks.19.",
        "blocks.20.",
        "blocks.21.",
        "blocks.22.",
        "blocks.23.",
        "blocks.24.",
        "blocks.25.",
        "blocks.26.",
        "blocks.27.",
        "blocks.28.",
        "blocks.29.",
        "blocks.30.",
        "blocks.31.",
        "blocks.32.",
        "blocks.33.",
        "blocks.34.",
        "blocks.35.",
        "blocks.36.",
        "blocks.37.",
        "blocks.38.",
        "blocks.39.",
        "head."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeWAN2_1",
    "display_name": "ModelMergeWAN2_1",
    "description": "1.3B model has 30 blocks, 14B model has 40 blocks. Image to video model has the extra img_emb.",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeCosmosPredict2_2B": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "pos_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedding_norm.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "pos_embedder.",
        "x_embedder.",
        "t_embedder.",
        "t_embedding_norm.",
        "blocks.0.",
        "blocks.1.",
        "blocks.2.",
        "blocks.3.",
        "blocks.4.",
        "blocks.5.",
        "blocks.6.",
        "blocks.7.",
        "blocks.8.",
        "blocks.9.",
        "blocks.10.",
        "blocks.11.",
        "blocks.12.",
        "blocks.13.",
        "blocks.14.",
        "blocks.15.",
        "blocks.16.",
        "blocks.17.",
        "blocks.18.",
        "blocks.19.",
        "blocks.20.",
        "blocks.21.",
        "blocks.22.",
        "blocks.23.",
        "blocks.24.",
        "blocks.25.",
        "blocks.26.",
        "blocks.27.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeCosmosPredict2_2B",
    "display_name": "ModelMergeCosmosPredict2_2B",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "ModelMergeCosmosPredict2_14B": {
    "input": {
      "required": {
        "model1": ["MODEL"],
        "model2": ["MODEL"],
        "pos_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "x_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedder.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "t_embedding_norm.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.0.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.1.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.2.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.3.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.4.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.5.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.6.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.7.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.8.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.9.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.10.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.11.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.12.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.13.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.14.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.15.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.16.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.17.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.18.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.19.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.20.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.21.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.22.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.23.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.24.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.25.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.26.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.27.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.28.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.29.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.30.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.31.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.32.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.33.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.34.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "blocks.35.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "final_layer.": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model1",
        "model2",
        "pos_embedder.",
        "x_embedder.",
        "t_embedder.",
        "t_embedding_norm.",
        "blocks.0.",
        "blocks.1.",
        "blocks.2.",
        "blocks.3.",
        "blocks.4.",
        "blocks.5.",
        "blocks.6.",
        "blocks.7.",
        "blocks.8.",
        "blocks.9.",
        "blocks.10.",
        "blocks.11.",
        "blocks.12.",
        "blocks.13.",
        "blocks.14.",
        "blocks.15.",
        "blocks.16.",
        "blocks.17.",
        "blocks.18.",
        "blocks.19.",
        "blocks.20.",
        "blocks.21.",
        "blocks.22.",
        "blocks.23.",
        "blocks.24.",
        "blocks.25.",
        "blocks.26.",
        "blocks.27.",
        "blocks.28.",
        "blocks.29.",
        "blocks.30.",
        "blocks.31.",
        "blocks.32.",
        "blocks.33.",
        "blocks.34.",
        "blocks.35.",
        "final_layer."
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelMergeCosmosPredict2_14B",
    "display_name": "ModelMergeCosmosPredict2_14B",
    "description": "",
    "python_module": "comfy_extras.nodes_model_merging_model_specific",
    "category": "advanced/model_merging/model_specific",
    "output_node": false
  },
  "PerturbedAttentionGuidance": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "scale": [
          "FLOAT",
          {
            "default": 3,
            "min": 0,
            "max": 100,
            "step": 0.01,
            "round": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "scale"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "PerturbedAttentionGuidance",
    "display_name": "PerturbedAttentionGuidance",
    "description": "",
    "python_module": "comfy_extras.nodes_pag",
    "category": "model_patches/unet",
    "output_node": false
  },
  "AlignYourStepsScheduler": {
    "input": {
      "required": {
        "model_type": [["SD1", "SDXL", "SVD"]],
        "steps": [
          "INT",
          {
            "default": 10,
            "min": 1,
            "max": 10000
          }
        ],
        "denoise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model_type", "steps", "denoise"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "AlignYourStepsScheduler",
    "display_name": "AlignYourStepsScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_align_your_steps",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "UNetSelfAttentionMultiply": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "q": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "k": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "v": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "out": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "q", "k", "v", "out"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "UNetSelfAttentionMultiply",
    "display_name": "UNetSelfAttentionMultiply",
    "description": "",
    "python_module": "comfy_extras.nodes_attention_multiply",
    "category": "_for_testing/attention_experiments",
    "output_node": false
  },
  "UNetCrossAttentionMultiply": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "q": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "k": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "v": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "out": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "q", "k", "v", "out"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "UNetCrossAttentionMultiply",
    "display_name": "UNetCrossAttentionMultiply",
    "description": "",
    "python_module": "comfy_extras.nodes_attention_multiply",
    "category": "_for_testing/attention_experiments",
    "output_node": false
  },
  "CLIPAttentionMultiply": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "q": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "k": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "v": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "out": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "q", "k", "v", "out"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "CLIPAttentionMultiply",
    "display_name": "CLIPAttentionMultiply",
    "description": "",
    "python_module": "comfy_extras.nodes_attention_multiply",
    "category": "_for_testing/attention_experiments",
    "output_node": false
  },
  "UNetTemporalAttentionMultiply": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "self_structural": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "self_temporal": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "cross_structural": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "cross_temporal": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "self_structural",
        "self_temporal",
        "cross_structural",
        "cross_temporal"
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "UNetTemporalAttentionMultiply",
    "display_name": "UNetTemporalAttentionMultiply",
    "description": "",
    "python_module": "comfy_extras.nodes_attention_multiply",
    "category": "_for_testing/attention_experiments",
    "output_node": false
  },
  "SamplerLCMUpscale": {
    "input": {
      "required": {
        "scale_ratio": [
          "FLOAT",
          {
            "default": 1,
            "min": 0.1,
            "max": 20,
            "step": 0.01
          }
        ],
        "scale_steps": [
          "INT",
          {
            "default": -1,
            "min": -1,
            "max": 1000,
            "step": 1
          }
        ],
        "upscale_method": [
          ["bislerp", "nearest-exact", "bilinear", "area", "bicubic"]
        ]
      }
    },
    "input_order": {
      "required": ["scale_ratio", "scale_steps", "upscale_method"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerLCMUpscale",
    "display_name": "SamplerLCMUpscale",
    "description": "",
    "python_module": "comfy_extras.nodes_advanced_samplers",
    "category": "sampling/custom_sampling/samplers",
    "output_node": false
  },
  "SamplerEulerCFGpp": {
    "input": {
      "required": {
        "version": [["regular", "alternative"]]
      }
    },
    "input_order": {
      "required": ["version"]
    },
    "output": ["SAMPLER"],
    "output_is_list": [false],
    "output_name": ["SAMPLER"],
    "name": "SamplerEulerCFGpp",
    "display_name": "SamplerEulerCFG++",
    "description": "",
    "python_module": "comfy_extras.nodes_advanced_samplers",
    "category": "_for_testing",
    "output_node": false
  },
  "WebcamCapture": {
    "input": {
      "required": {
        "image": ["WEBCAM", {}],
        "width": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 16384,
            "step": 1
          }
        ],
        "capture_on_queue": [
          "BOOLEAN",
          {
            "default": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "width", "height", "capture_on_queue"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "WebcamCapture",
    "display_name": "Webcam Capture",
    "description": "",
    "python_module": "comfy_extras.nodes_webcam",
    "category": "image",
    "output_node": false
  },
  "EmptyLatentAudio": {
    "input": {
      "required": {
        "seconds": [
          "FLOAT",
          {
            "default": 47.6,
            "min": 1,
            "max": 1000,
            "step": 0.1
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096,
            "tooltip": "The number of latent images in the batch."
          }
        ]
      }
    },
    "input_order": {
      "required": ["seconds", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyLatentAudio",
    "display_name": "Empty Latent Audio",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "latent/audio",
    "output_node": false
  },
  "VAEEncodeAudio": {
    "input": {
      "required": {
        "audio": ["AUDIO"],
        "vae": ["VAE"]
      }
    },
    "input_order": {
      "required": ["audio", "vae"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "VAEEncodeAudio",
    "display_name": "VAE Encode Audio",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "latent/audio",
    "output_node": false
  },
  "VAEDecodeAudio": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "vae": ["VAE"]
      }
    },
    "input_order": {
      "required": ["samples", "vae"]
    },
    "output": ["AUDIO"],
    "output_is_list": [false],
    "output_name": ["AUDIO"],
    "name": "VAEDecodeAudio",
    "display_name": "VAE Decode Audio",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "latent/audio",
    "output_node": false
  },
  "SaveAudio": {
    "input": {
      "required": {
        "audio": ["AUDIO"],
        "filename_prefix": [
          "STRING",
          {
            "default": "audio/ComfyUI"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["audio", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveAudio",
    "display_name": "Save Audio (FLAC)",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "audio",
    "output_node": true
  },
  "SaveAudioMP3": {
    "input": {
      "required": {
        "audio": ["AUDIO"],
        "filename_prefix": [
          "STRING",
          {
            "default": "audio/ComfyUI"
          }
        ],
        "quality": [
          ["V0", "128k", "320k"],
          {
            "default": "V0"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["audio", "filename_prefix", "quality"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveAudioMP3",
    "display_name": "Save Audio (MP3)",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "audio",
    "output_node": true
  },
  "SaveAudioOpus": {
    "input": {
      "required": {
        "audio": ["AUDIO"],
        "filename_prefix": [
          "STRING",
          {
            "default": "audio/ComfyUI"
          }
        ],
        "quality": [
          ["64k", "96k", "128k", "192k", "320k"],
          {
            "default": "128k"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["audio", "filename_prefix", "quality"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveAudioOpus",
    "display_name": "Save Audio (Opus)",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "audio",
    "output_node": true
  },
  "LoadAudio": {
    "input": {
      "required": {
        "audio": [
          [],
          {
            "audio_upload": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["audio"]
    },
    "output": ["AUDIO"],
    "output_is_list": [false],
    "output_name": ["AUDIO"],
    "name": "LoadAudio",
    "display_name": "Load Audio",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "audio",
    "output_node": false
  },
  "PreviewAudio": {
    "input": {
      "required": {
        "audio": ["AUDIO"]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["audio"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "PreviewAudio",
    "display_name": "Preview Audio",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "audio",
    "output_node": true
  },
  "ConditioningStableAudio": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "seconds_start": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1000,
            "step": 0.1
          }
        ],
        "seconds_total": [
          "FLOAT",
          {
            "default": 47,
            "min": 0,
            "max": 1000,
            "step": 0.1
          }
        ]
      }
    },
    "input_order": {
      "required": ["positive", "negative", "seconds_start", "seconds_total"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "ConditioningStableAudio",
    "display_name": "ConditioningStableAudio",
    "description": "",
    "python_module": "comfy_extras.nodes_audio",
    "category": "conditioning",
    "output_node": false
  },
  "TripleCLIPLoader": {
    "input": {
      "required": {
        "clip_name1": [[]],
        "clip_name2": [[]],
        "clip_name3": [[]]
      }
    },
    "input_order": {
      "required": ["clip_name1", "clip_name2", "clip_name3"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "TripleCLIPLoader",
    "display_name": "TripleCLIPLoader",
    "description": "[Recipes]\n\nsd3: clip-l, clip-g, t5",
    "python_module": "comfy_extras.nodes_sd3",
    "category": "advanced/loaders",
    "output_node": false
  },
  "EmptySD3LatentImage": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptySD3LatentImage",
    "display_name": "EmptySD3LatentImage",
    "description": "",
    "python_module": "comfy_extras.nodes_sd3",
    "category": "latent/sd3",
    "output_node": false
  },
  "CLIPTextEncodeSD3": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "clip_l": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "clip_g": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "t5xxl": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "empty_padding": [["none", "empty_prompt"]]
      }
    },
    "input_order": {
      "required": ["clip", "clip_l", "clip_g", "t5xxl", "empty_padding"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeSD3",
    "display_name": "CLIPTextEncodeSD3",
    "description": "",
    "python_module": "comfy_extras.nodes_sd3",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "ControlNetApplySD3": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "control_net": ["CONTROL_NET"],
        "vae": ["VAE"],
        "image": ["IMAGE"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "control_net",
        "vae",
        "image",
        "strength",
        "start_percent",
        "end_percent"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "ControlNetApplySD3",
    "display_name": "Apply Controlnet with VAE",
    "description": "",
    "python_module": "comfy_extras.nodes_sd3",
    "category": "conditioning/controlnet",
    "output_node": false,
    "deprecated": true
  },
  "SkipLayerGuidanceSD3": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "layers": [
          "STRING",
          {
            "default": "7, 8, 9",
            "multiline": false
          }
        ],
        "scale": [
          "FLOAT",
          {
            "default": 3,
            "min": 0,
            "max": 10,
            "step": 0.1
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0.01,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 0.15,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "layers", "scale", "start_percent", "end_percent"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "SkipLayerGuidanceSD3",
    "display_name": "SkipLayerGuidanceSD3",
    "description": "Generic version of SkipLayerGuidance node that can be used on every DiT model.",
    "python_module": "comfy_extras.nodes_sd3",
    "category": "advanced/guidance",
    "output_node": false,
    "experimental": true
  },
  "GITSScheduler": {
    "input": {
      "required": {
        "coeff": [
          "FLOAT",
          {
            "default": 1.2,
            "min": 0.8,
            "max": 1.5,
            "step": 0.05
          }
        ],
        "steps": [
          "INT",
          {
            "default": 10,
            "min": 2,
            "max": 1000
          }
        ],
        "denoise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["coeff", "steps", "denoise"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "GITSScheduler",
    "display_name": "GITSScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_gits",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "SetUnionControlNetType": {
    "input": {
      "required": {
        "control_net": ["CONTROL_NET"],
        "type": [
          [
            "auto",
            "openpose",
            "depth",
            "hed/pidi/scribble/ted",
            "canny/lineart/anime_lineart/mlsd",
            "normal",
            "segment",
            "tile",
            "repaint"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["control_net", "type"]
    },
    "output": ["CONTROL_NET"],
    "output_is_list": [false],
    "output_name": ["CONTROL_NET"],
    "name": "SetUnionControlNetType",
    "display_name": "SetUnionControlNetType",
    "description": "",
    "python_module": "comfy_extras.nodes_controlnet",
    "category": "conditioning/controlnet",
    "output_node": false
  },
  "ControlNetInpaintingAliMamaApply": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "control_net": ["CONTROL_NET"],
        "vae": ["VAE"],
        "image": ["IMAGE"],
        "mask": ["MASK"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "control_net",
        "vae",
        "image",
        "mask",
        "strength",
        "start_percent",
        "end_percent"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "ControlNetInpaintingAliMamaApply",
    "display_name": "ControlNetInpaintingAliMamaApply",
    "description": "",
    "python_module": "comfy_extras.nodes_controlnet",
    "category": "conditioning/controlnet",
    "output_node": false
  },
  "CLIPTextEncodeHunyuanDiT": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "bert": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "mt5xl": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "bert", "mt5xl"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeHunyuanDiT",
    "display_name": "CLIPTextEncodeHunyuanDiT",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "TextEncodeHunyuanVideo_ImageToVideo": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "clip_vision_output": ["CLIP_VISION_OUTPUT"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "image_interleave": [
          "INT",
          {
            "default": 2,
            "min": 1,
            "max": 512,
            "tooltip": "How much the image influences things vs the text prompt. Higher number means more influence from the text prompt."
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "clip_vision_output", "prompt", "image_interleave"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "TextEncodeHunyuanVideo_ImageToVideo",
    "display_name": "TextEncodeHunyuanVideo_ImageToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "EmptyHunyuanLatentVideo": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 848,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 25,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "length", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyHunyuanLatentVideo",
    "display_name": "EmptyHunyuanLatentVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan",
    "category": "latent/video",
    "output_node": false
  },
  "HunyuanImageToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 848,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 53,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ],
        "guidance_type": [["v1 (concat)", "v2 (replace)", "custom"]]
      },
      "optional": {
        "start_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "vae",
        "width",
        "height",
        "length",
        "batch_size",
        "guidance_type"
      ],
      "optional": ["start_image"]
    },
    "output": ["CONDITIONING", "LATENT"],
    "output_is_list": [false, false],
    "output_name": ["positive", "latent"],
    "name": "HunyuanImageToVideo",
    "display_name": "HunyuanImageToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "CLIPTextEncodeFlux": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "clip_l": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "t5xxl": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "guidance": [
          "FLOAT",
          {
            "default": 3.5,
            "min": 0,
            "max": 100,
            "step": 0.1
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "clip_l", "t5xxl", "guidance"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeFlux",
    "display_name": "CLIPTextEncodeFlux",
    "description": "",
    "python_module": "comfy_extras.nodes_flux",
    "category": "advanced/conditioning/flux",
    "output_node": false
  },
  "FluxGuidance": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"],
        "guidance": [
          "FLOAT",
          {
            "default": 3.5,
            "min": 0,
            "max": 100,
            "step": 0.1
          }
        ]
      }
    },
    "input_order": {
      "required": ["conditioning", "guidance"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "FluxGuidance",
    "display_name": "FluxGuidance",
    "description": "",
    "python_module": "comfy_extras.nodes_flux",
    "category": "advanced/conditioning/flux",
    "output_node": false
  },
  "FluxDisableGuidance": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"]
      }
    },
    "input_order": {
      "required": ["conditioning"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "FluxDisableGuidance",
    "display_name": "FluxDisableGuidance",
    "description": "This node completely disables the guidance embed on Flux and Flux like models",
    "python_module": "comfy_extras.nodes_flux",
    "category": "advanced/conditioning/flux",
    "output_node": false
  },
  "FluxKontextImageScale": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["image"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxKontextImageScale",
    "display_name": "FluxKontextImageScale",
    "description": "This node resizes the image to one that is more optimal for flux kontext.",
    "python_module": "comfy_extras.nodes_flux",
    "category": "advanced/conditioning/flux",
    "output_node": false
  },
  "LoraSave": {
    "input": {
      "required": {
        "filename_prefix": [
          "STRING",
          {
            "default": "loras/ComfyUI_extracted_lora"
          }
        ],
        "rank": [
          "INT",
          {
            "default": 8,
            "min": 1,
            "max": 4096,
            "step": 1
          }
        ],
        "lora_type": [["standard", "full_diff"]],
        "bias_diff": [
          "BOOLEAN",
          {
            "default": true
          }
        ]
      },
      "optional": {
        "model_diff": [
          "MODEL",
          {
            "tooltip": "The ModelSubtract output to be converted to a lora."
          }
        ],
        "text_encoder_diff": [
          "CLIP",
          {
            "tooltip": "The CLIPSubtract output to be converted to a lora."
          }
        ]
      }
    },
    "input_order": {
      "required": ["filename_prefix", "rank", "lora_type", "bias_diff"],
      "optional": ["model_diff", "text_encoder_diff"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "LoraSave",
    "display_name": "Extract and Save Lora",
    "description": "",
    "python_module": "comfy_extras.nodes_lora_extract",
    "category": "_for_testing",
    "output_node": true
  },
  "TorchCompileModel": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "backend": [["inductor", "cudagraphs"]]
      }
    },
    "input_order": {
      "required": ["model", "backend"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "TorchCompileModel",
    "display_name": "TorchCompileModel",
    "description": "",
    "python_module": "comfy_extras.nodes_torch_compile",
    "category": "_for_testing",
    "output_node": false,
    "experimental": true
  },
  "EmptyMochiLatentVideo": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 848,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 25,
            "min": 7,
            "max": 16384,
            "step": 6
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "length", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyMochiLatentVideo",
    "display_name": "EmptyMochiLatentVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_mochi",
    "category": "latent/video",
    "output_node": false
  },
  "SkipLayerGuidanceDiT": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "double_layers": [
          "STRING",
          {
            "default": "7, 8, 9",
            "multiline": false
          }
        ],
        "single_layers": [
          "STRING",
          {
            "default": "7, 8, 9",
            "multiline": false
          }
        ],
        "scale": [
          "FLOAT",
          {
            "default": 3,
            "min": 0,
            "max": 10,
            "step": 0.1
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0.01,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 0.15,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "rescaling_scale": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "double_layers",
        "single_layers",
        "scale",
        "start_percent",
        "end_percent",
        "rescaling_scale"
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "SkipLayerGuidanceDiT",
    "display_name": "SkipLayerGuidanceDiT",
    "description": "Generic version of SkipLayerGuidance node that can be used on every DiT model.",
    "python_module": "comfy_extras.nodes_slg",
    "category": "advanced/guidance",
    "output_node": false,
    "experimental": true
  },
  "SkipLayerGuidanceDiTSimple": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "double_layers": [
          "STRING",
          {
            "default": "7, 8, 9",
            "multiline": false
          }
        ],
        "single_layers": [
          "STRING",
          {
            "default": "7, 8, 9",
            "multiline": false
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "model",
        "double_layers",
        "single_layers",
        "start_percent",
        "end_percent"
      ]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "SkipLayerGuidanceDiTSimple",
    "display_name": "SkipLayerGuidanceDiTSimple",
    "description": "Simple version of the SkipLayerGuidanceDiT node that only modifies the uncond pass.",
    "python_module": "comfy_extras.nodes_slg",
    "category": "advanced/guidance",
    "output_node": false,
    "experimental": true
  },
  "Mahiro": {
    "input": {
      "required": {
        "model": ["MODEL"]
      }
    },
    "input_order": {
      "required": ["model"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["patched_model"],
    "name": "Mahiro",
    "display_name": "Mahiro is so cute that she deserves a better guidance function!! (。・ω・。)",
    "description": "Modify the guidance to scale more on the 'direction' of the positive prompt rather than the difference between the negative prompt.",
    "python_module": "comfy_extras.nodes_mahiro",
    "category": "_for_testing",
    "output_node": false
  },
  "EmptyLTXVLatentVideo": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 768,
            "min": 64,
            "max": 16384,
            "step": 32
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 64,
            "max": 16384,
            "step": 32
          }
        ],
        "length": [
          "INT",
          {
            "default": 97,
            "min": 1,
            "max": 16384,
            "step": 8
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "length", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyLTXVLatentVideo",
    "display_name": "EmptyLTXVLatentVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "latent/video/ltxv",
    "output_node": false
  },
  "LTXVImgToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "image": ["IMAGE"],
        "width": [
          "INT",
          {
            "default": 768,
            "min": 64,
            "max": 16384,
            "step": 32
          }
        ],
        "height": [
          "INT",
          {
            "default": 512,
            "min": 64,
            "max": 16384,
            "step": 32
          }
        ],
        "length": [
          "INT",
          {
            "default": 97,
            "min": 9,
            "max": 16384,
            "step": 8
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "image",
        "width",
        "height",
        "length",
        "batch_size",
        "strength"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "LTXVImgToVideo",
    "display_name": "LTXVImgToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "ModelSamplingLTXV": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "max_shift": [
          "FLOAT",
          {
            "default": 2.05,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ],
        "base_shift": [
          "FLOAT",
          {
            "default": 0.95,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      },
      "optional": {
        "latent": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["model", "max_shift", "base_shift"],
      "optional": ["latent"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "ModelSamplingLTXV",
    "display_name": "ModelSamplingLTXV",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "advanced/model",
    "output_node": false
  },
  "LTXVConditioning": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "frame_rate": [
          "FLOAT",
          {
            "default": 25,
            "min": 0,
            "max": 1000,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["positive", "negative", "frame_rate"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "LTXVConditioning",
    "display_name": "LTXVConditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "LTXVScheduler": {
    "input": {
      "required": {
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000
          }
        ],
        "max_shift": [
          "FLOAT",
          {
            "default": 2.05,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ],
        "base_shift": [
          "FLOAT",
          {
            "default": 0.95,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ],
        "stretch": [
          "BOOLEAN",
          {
            "default": true,
            "tooltip": "Stretch the sigmas to be in the range [terminal, 1]."
          }
        ],
        "terminal": [
          "FLOAT",
          {
            "default": 0.1,
            "min": 0,
            "max": 0.99,
            "step": 0.01,
            "tooltip": "The terminal value of the sigmas after stretching."
          }
        ]
      },
      "optional": {
        "latent": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["steps", "max_shift", "base_shift", "stretch", "terminal"],
      "optional": ["latent"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "LTXVScheduler",
    "display_name": "LTXVScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "LTXVAddGuide": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "latent": ["LATENT"],
        "image": [
          "IMAGE",
          {
            "tooltip": "Image or video to condition the latent video on. Must be 8*n + 1 frames.If the video is not 8*n + 1 frames, it will be cropped to the nearest 8*n + 1 frames."
          }
        ],
        "frame_idx": [
          "INT",
          {
            "default": 0,
            "min": -9999,
            "max": 9999,
            "tooltip": "Frame index to start the conditioning at. For single-frame images or videos with 1-8 frames, any frame_idx value is acceptable. For videos with 9+ frames, frame_idx must be divisible by 8, otherwise it will be rounded down to the nearest multiple of 8. Negative values are counted from the end of the video."
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "latent",
        "image",
        "frame_idx",
        "strength"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "LTXVAddGuide",
    "display_name": "LTXVAddGuide",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "LTXVPreprocess": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "img_compression": [
          "INT",
          {
            "default": 35,
            "min": 0,
            "max": 100,
            "tooltip": "Amount of compression to apply on image."
          }
        ]
      }
    },
    "input_order": {
      "required": ["image", "img_compression"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["output_image"],
    "name": "LTXVPreprocess",
    "display_name": "LTXVPreprocess",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "image",
    "output_node": false
  },
  "LTXVCropGuides": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "latent": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["positive", "negative", "latent"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "LTXVCropGuides",
    "display_name": "LTXVCropGuides",
    "description": "",
    "python_module": "comfy_extras.nodes_lt",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "CreateHookLora": {
    "input": {
      "required": {
        "lora_name": [[]],
        "strength_model": [
          "FLOAT",
          {
            "default": 1,
            "min": -20,
            "max": 20,
            "step": 0.01
          }
        ],
        "strength_clip": [
          "FLOAT",
          {
            "default": 1,
            "min": -20,
            "max": 20,
            "step": 0.01
          }
        ]
      },
      "optional": {
        "prev_hooks": ["HOOKS"]
      }
    },
    "input_order": {
      "required": ["lora_name", "strength_model", "strength_clip"],
      "optional": ["prev_hooks"]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "CreateHookLora",
    "display_name": "Create Hook LoRA",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/create",
    "output_node": false,
    "experimental": true
  },
  "CreateHookLoraModelOnly": {
    "input": {
      "required": {
        "lora_name": [[]],
        "strength_model": [
          "FLOAT",
          {
            "default": 1,
            "min": -20,
            "max": 20,
            "step": 0.01
          }
        ]
      },
      "optional": {
        "prev_hooks": ["HOOKS"]
      }
    },
    "input_order": {
      "required": ["lora_name", "strength_model"],
      "optional": ["prev_hooks"]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "CreateHookLoraModelOnly",
    "display_name": "Create Hook LoRA (MO)",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/create",
    "output_node": false,
    "experimental": true
  },
  "CreateHookModelAsLora": {
    "input": {
      "required": {
        "ckpt_name": [[]],
        "strength_model": [
          "FLOAT",
          {
            "default": 1,
            "min": -20,
            "max": 20,
            "step": 0.01
          }
        ],
        "strength_clip": [
          "FLOAT",
          {
            "default": 1,
            "min": -20,
            "max": 20,
            "step": 0.01
          }
        ]
      },
      "optional": {
        "prev_hooks": ["HOOKS"]
      }
    },
    "input_order": {
      "required": ["ckpt_name", "strength_model", "strength_clip"],
      "optional": ["prev_hooks"]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "CreateHookModelAsLora",
    "display_name": "Create Hook Model as LoRA",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/create",
    "output_node": false,
    "experimental": true
  },
  "CreateHookModelAsLoraModelOnly": {
    "input": {
      "required": {
        "ckpt_name": [[]],
        "strength_model": [
          "FLOAT",
          {
            "default": 1,
            "min": -20,
            "max": 20,
            "step": 0.01
          }
        ]
      },
      "optional": {
        "prev_hooks": ["HOOKS"]
      }
    },
    "input_order": {
      "required": ["ckpt_name", "strength_model"],
      "optional": ["prev_hooks"]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "CreateHookModelAsLoraModelOnly",
    "display_name": "Create Hook Model as LoRA (MO)",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/create",
    "output_node": false,
    "experimental": true
  },
  "SetHookKeyframes": {
    "input": {
      "required": {
        "hooks": ["HOOKS"]
      },
      "optional": {
        "hook_kf": ["HOOK_KEYFRAMES"]
      }
    },
    "input_order": {
      "required": ["hooks"],
      "optional": ["hook_kf"]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "SetHookKeyframes",
    "display_name": "Set Hook Keyframes",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/scheduling",
    "output_node": false,
    "experimental": true
  },
  "CreateHookKeyframe": {
    "input": {
      "required": {
        "strength_mult": [
          "FLOAT",
          {
            "default": 1,
            "min": -20,
            "max": 20,
            "step": 0.01
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      },
      "optional": {
        "prev_hook_kf": ["HOOK_KEYFRAMES"]
      }
    },
    "input_order": {
      "required": ["strength_mult", "start_percent"],
      "optional": ["prev_hook_kf"]
    },
    "output": ["HOOK_KEYFRAMES"],
    "output_is_list": [false],
    "output_name": ["HOOK_KF"],
    "name": "CreateHookKeyframe",
    "display_name": "Create Hook Keyframe",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/scheduling",
    "output_node": false,
    "experimental": true
  },
  "CreateHookKeyframesInterpolated": {
    "input": {
      "required": {
        "strength_start": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.001
          }
        ],
        "strength_end": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.001
          }
        ],
        "interpolation": [["linear", "ease_in", "ease_out", "ease_in_out"]],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "keyframes_count": [
          "INT",
          {
            "default": 5,
            "min": 2,
            "max": 100,
            "step": 1
          }
        ],
        "print_keyframes": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "optional": {
        "prev_hook_kf": ["HOOK_KEYFRAMES"]
      }
    },
    "input_order": {
      "required": [
        "strength_start",
        "strength_end",
        "interpolation",
        "start_percent",
        "end_percent",
        "keyframes_count",
        "print_keyframes"
      ],
      "optional": ["prev_hook_kf"]
    },
    "output": ["HOOK_KEYFRAMES"],
    "output_is_list": [false],
    "output_name": ["HOOK_KF"],
    "name": "CreateHookKeyframesInterpolated",
    "display_name": "Create Hook Keyframes Interp.",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/scheduling",
    "output_node": false,
    "experimental": true
  },
  "CreateHookKeyframesFromFloats": {
    "input": {
      "required": {
        "floats_strength": [
          "FLOATS",
          {
            "default": -1,
            "min": -1,
            "step": 0.001,
            "forceInput": true
          }
        ],
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "print_keyframes": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "optional": {
        "prev_hook_kf": ["HOOK_KEYFRAMES"]
      }
    },
    "input_order": {
      "required": [
        "floats_strength",
        "start_percent",
        "end_percent",
        "print_keyframes"
      ],
      "optional": ["prev_hook_kf"]
    },
    "output": ["HOOK_KEYFRAMES"],
    "output_is_list": [false],
    "output_name": ["HOOK_KF"],
    "name": "CreateHookKeyframesFromFloats",
    "display_name": "Create Hook Keyframes From Floats",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/scheduling",
    "output_node": false,
    "experimental": true
  },
  "CombineHooks2": {
    "input": {
      "required": {},
      "optional": {
        "hooks_A": ["HOOKS"],
        "hooks_B": ["HOOKS"]
      }
    },
    "input_order": {
      "required": [],
      "optional": ["hooks_A", "hooks_B"]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "CombineHooks2",
    "display_name": "Combine Hooks [2]",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/combine",
    "output_node": false,
    "experimental": true
  },
  "CombineHooks4": {
    "input": {
      "required": {},
      "optional": {
        "hooks_A": ["HOOKS"],
        "hooks_B": ["HOOKS"],
        "hooks_C": ["HOOKS"],
        "hooks_D": ["HOOKS"]
      }
    },
    "input_order": {
      "required": [],
      "optional": ["hooks_A", "hooks_B", "hooks_C", "hooks_D"]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "CombineHooks4",
    "display_name": "Combine Hooks [4]",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/combine",
    "output_node": false,
    "experimental": true
  },
  "CombineHooks8": {
    "input": {
      "required": {},
      "optional": {
        "hooks_A": ["HOOKS"],
        "hooks_B": ["HOOKS"],
        "hooks_C": ["HOOKS"],
        "hooks_D": ["HOOKS"],
        "hooks_E": ["HOOKS"],
        "hooks_F": ["HOOKS"],
        "hooks_G": ["HOOKS"],
        "hooks_H": ["HOOKS"]
      }
    },
    "input_order": {
      "required": [],
      "optional": [
        "hooks_A",
        "hooks_B",
        "hooks_C",
        "hooks_D",
        "hooks_E",
        "hooks_F",
        "hooks_G",
        "hooks_H"
      ]
    },
    "output": ["HOOKS"],
    "output_is_list": [false],
    "output_name": ["HOOKS"],
    "name": "CombineHooks8",
    "display_name": "Combine Hooks [8]",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/combine",
    "output_node": false,
    "experimental": true
  },
  "ConditioningSetProperties": {
    "input": {
      "required": {
        "cond_NEW": ["CONDITIONING"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "set_cond_area": [["default", "mask bounds"]]
      },
      "optional": {
        "mask": ["MASK"],
        "hooks": ["HOOKS"],
        "timesteps": ["TIMESTEPS_RANGE"]
      }
    },
    "input_order": {
      "required": ["cond_NEW", "strength", "set_cond_area"],
      "optional": ["mask", "hooks", "timesteps"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetProperties",
    "display_name": "Cond Set Props",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/cond single",
    "output_node": false,
    "experimental": true
  },
  "ConditioningSetPropertiesAndCombine": {
    "input": {
      "required": {
        "cond": ["CONDITIONING"],
        "cond_NEW": ["CONDITIONING"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "set_cond_area": [["default", "mask bounds"]]
      },
      "optional": {
        "mask": ["MASK"],
        "hooks": ["HOOKS"],
        "timesteps": ["TIMESTEPS_RANGE"]
      }
    },
    "input_order": {
      "required": ["cond", "cond_NEW", "strength", "set_cond_area"],
      "optional": ["mask", "hooks", "timesteps"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetPropertiesAndCombine",
    "display_name": "Cond Set Props Combine",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/cond single",
    "output_node": false,
    "experimental": true
  },
  "PairConditioningSetProperties": {
    "input": {
      "required": {
        "positive_NEW": ["CONDITIONING"],
        "negative_NEW": ["CONDITIONING"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "set_cond_area": [["default", "mask bounds"]]
      },
      "optional": {
        "mask": ["MASK"],
        "hooks": ["HOOKS"],
        "timesteps": ["TIMESTEPS_RANGE"]
      }
    },
    "input_order": {
      "required": ["positive_NEW", "negative_NEW", "strength", "set_cond_area"],
      "optional": ["mask", "hooks", "timesteps"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "PairConditioningSetProperties",
    "display_name": "Cond Pair Set Props",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/cond pair",
    "output_node": false,
    "experimental": true
  },
  "PairConditioningSetPropertiesAndCombine": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "positive_NEW": ["CONDITIONING"],
        "negative_NEW": ["CONDITIONING"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ],
        "set_cond_area": [["default", "mask bounds"]]
      },
      "optional": {
        "mask": ["MASK"],
        "hooks": ["HOOKS"],
        "timesteps": ["TIMESTEPS_RANGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "positive_NEW",
        "negative_NEW",
        "strength",
        "set_cond_area"
      ],
      "optional": ["mask", "hooks", "timesteps"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "PairConditioningSetPropertiesAndCombine",
    "display_name": "Cond Pair Set Props Combine",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/cond pair",
    "output_node": false,
    "experimental": true
  },
  "ConditioningSetDefaultCombine": {
    "input": {
      "required": {
        "cond": ["CONDITIONING"],
        "cond_DEFAULT": ["CONDITIONING"]
      },
      "optional": {
        "hooks": ["HOOKS"]
      }
    },
    "input_order": {
      "required": ["cond", "cond_DEFAULT"],
      "optional": ["hooks"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ConditioningSetDefaultCombine",
    "display_name": "Cond Set Default Combine",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/cond single",
    "output_node": false,
    "experimental": true
  },
  "PairConditioningSetDefaultCombine": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "positive_DEFAULT": ["CONDITIONING"],
        "negative_DEFAULT": ["CONDITIONING"]
      },
      "optional": {
        "hooks": ["HOOKS"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "positive_DEFAULT",
        "negative_DEFAULT"
      ],
      "optional": ["hooks"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "PairConditioningSetDefaultCombine",
    "display_name": "Cond Pair Set Default Combine",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/cond pair",
    "output_node": false,
    "experimental": true
  },
  "PairConditioningCombine": {
    "input": {
      "required": {
        "positive_A": ["CONDITIONING"],
        "negative_A": ["CONDITIONING"],
        "positive_B": ["CONDITIONING"],
        "negative_B": ["CONDITIONING"]
      }
    },
    "input_order": {
      "required": ["positive_A", "negative_A", "positive_B", "negative_B"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "PairConditioningCombine",
    "display_name": "Cond Pair Combine",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/cond pair",
    "output_node": false,
    "experimental": true
  },
  "SetClipHooks": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "apply_to_conds": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "schedule_clip": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "optional": {
        "hooks": ["HOOKS"]
      }
    },
    "input_order": {
      "required": ["clip", "apply_to_conds", "schedule_clip"],
      "optional": ["hooks"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "SetClipHooks",
    "display_name": "Set CLIP Hooks",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks/clip",
    "output_node": false,
    "experimental": true
  },
  "ConditioningTimestepsRange": {
    "input": {
      "required": {
        "start_percent": [
          "FLOAT",
          {
            "default": 0,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ],
        "end_percent": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": ["start_percent", "end_percent"]
    },
    "output": ["TIMESTEPS_RANGE", "TIMESTEPS_RANGE", "TIMESTEPS_RANGE"],
    "output_is_list": [false, false, false],
    "output_name": ["TIMESTEPS_RANGE", "BEFORE_RANGE", "AFTER_RANGE"],
    "name": "ConditioningTimestepsRange",
    "display_name": "Timesteps Range",
    "description": "",
    "python_module": "comfy_extras.nodes_hooks",
    "category": "advanced/hooks",
    "output_node": false,
    "experimental": true
  },
  "Load3D": {
    "input": {
      "required": {
        "model_file": [
          [],
          {
            "file_upload": true
          }
        ],
        "image": ["LOAD_3D", {}],
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 1,
            "max": 4096,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 1,
            "max": 4096,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["model_file", "image", "width", "height"]
    },
    "output": [
      "IMAGE",
      "MASK",
      "STRING",
      "IMAGE",
      "IMAGE",
      "LOAD3D_CAMERA",
      "VIDEO"
    ],
    "output_is_list": [false, false, false, false, false, false, false],
    "output_name": [
      "image",
      "mask",
      "mesh_path",
      "normal",
      "lineart",
      "camera_info",
      "recording_video"
    ],
    "name": "Load3D",
    "display_name": "Load 3D",
    "description": "",
    "python_module": "comfy_extras.nodes_load_3d",
    "category": "3d",
    "output_node": false,
    "experimental": true
  },
  "Load3DAnimation": {
    "input": {
      "required": {
        "model_file": [
          [],
          {
            "file_upload": true
          }
        ],
        "image": ["LOAD_3D_ANIMATION", {}],
        "width": [
          "INT",
          {
            "default": 1024,
            "min": 1,
            "max": 4096,
            "step": 1
          }
        ],
        "height": [
          "INT",
          {
            "default": 1024,
            "min": 1,
            "max": 4096,
            "step": 1
          }
        ]
      }
    },
    "input_order": {
      "required": ["model_file", "image", "width", "height"]
    },
    "output": ["IMAGE", "MASK", "STRING", "IMAGE", "LOAD3D_CAMERA", "VIDEO"],
    "output_is_list": [false, false, false, false, false, false],
    "output_name": [
      "image",
      "mask",
      "mesh_path",
      "normal",
      "camera_info",
      "recording_video"
    ],
    "name": "Load3DAnimation",
    "display_name": "Load 3D - Animation",
    "description": "",
    "python_module": "comfy_extras.nodes_load_3d",
    "category": "3d",
    "output_node": false,
    "experimental": true
  },
  "Preview3D": {
    "input": {
      "required": {
        "model_file": [
          "STRING",
          {
            "default": "",
            "multiline": false
          }
        ]
      },
      "optional": {
        "camera_info": ["LOAD3D_CAMERA", {}]
      }
    },
    "input_order": {
      "required": ["model_file"],
      "optional": ["camera_info"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "Preview3D",
    "display_name": "Preview 3D",
    "description": "",
    "python_module": "comfy_extras.nodes_load_3d",
    "category": "3d",
    "output_node": true,
    "experimental": true
  },
  "Preview3DAnimation": {
    "input": {
      "required": {
        "model_file": [
          "STRING",
          {
            "default": "",
            "multiline": false
          }
        ]
      },
      "optional": {
        "camera_info": ["LOAD3D_CAMERA", {}]
      }
    },
    "input_order": {
      "required": ["model_file"],
      "optional": ["camera_info"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "Preview3DAnimation",
    "display_name": "Preview 3D - Animation",
    "description": "",
    "python_module": "comfy_extras.nodes_load_3d",
    "category": "3d",
    "output_node": true,
    "experimental": true
  },
  "EmptyCosmosLatentVideo": {
    "input": {
      "required": {
        "width": [
          "INT",
          {
            "default": 1280,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 704,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 121,
            "min": 1,
            "max": 16384,
            "step": 8
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      }
    },
    "input_order": {
      "required": ["width", "height", "length", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyCosmosLatentVideo",
    "display_name": "EmptyCosmosLatentVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_cosmos",
    "category": "latent/video",
    "output_node": false
  },
  "CosmosImageToVideoLatent": {
    "input": {
      "required": {
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 1280,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 704,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 121,
            "min": 1,
            "max": 16384,
            "step": 8
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "start_image": ["IMAGE"],
        "end_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["vae", "width", "height", "length", "batch_size"],
      "optional": ["start_image", "end_image"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "CosmosImageToVideoLatent",
    "display_name": "CosmosImageToVideoLatent",
    "description": "",
    "python_module": "comfy_extras.nodes_cosmos",
    "category": "conditioning/inpaint",
    "output_node": false
  },
  "CosmosPredict2ImageToVideoLatent": {
    "input": {
      "required": {
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 848,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 93,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "start_image": ["IMAGE"],
        "end_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["vae", "width", "height", "length", "batch_size"],
      "optional": ["start_image", "end_image"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "CosmosPredict2ImageToVideoLatent",
    "display_name": "CosmosPredict2ImageToVideoLatent",
    "description": "",
    "python_module": "comfy_extras.nodes_cosmos",
    "category": "conditioning/inpaint",
    "output_node": false
  },
  "SaveWEBM": {
    "input": {
      "required": {
        "images": ["IMAGE"],
        "filename_prefix": [
          "STRING",
          {
            "default": "ComfyUI"
          }
        ],
        "codec": [["vp9", "av1"]],
        "fps": [
          "FLOAT",
          {
            "default": 24,
            "min": 0.01,
            "max": 1000,
            "step": 0.01
          }
        ],
        "crf": [
          "FLOAT",
          {
            "default": 32,
            "min": 0,
            "max": 63,
            "step": 1,
            "tooltip": "Higher crf means lower quality with a smaller file size, lower crf means higher quality higher filesize."
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["images", "filename_prefix", "codec", "fps", "crf"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveWEBM",
    "display_name": "SaveWEBM",
    "description": "",
    "python_module": "comfy_extras.nodes_video",
    "category": "image/video",
    "output_node": true,
    "experimental": true
  },
  "SaveVideo": {
    "input": {
      "required": {
        "video": [
          "VIDEO",
          {
            "tooltip": "The video to save."
          }
        ],
        "filename_prefix": [
          "STRING",
          {
            "default": "video/ComfyUI",
            "tooltip": "The prefix for the file to save. This may include formatting information such as %date:yyyy-MM-dd% or %Empty Latent Image.width% to include values from nodes."
          }
        ],
        "format": [
          ["auto", "mp4"],
          {
            "default": "auto",
            "tooltip": "The format to save the video as."
          }
        ],
        "codec": [
          ["auto", "h264"],
          {
            "default": "auto",
            "tooltip": "The codec to use for the video."
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["video", "filename_prefix", "format", "codec"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveVideo",
    "display_name": "Save Video",
    "description": "Saves the input images to your ComfyUI output directory.",
    "python_module": "comfy_extras.nodes_video",
    "category": "image/video",
    "output_node": true
  },
  "CreateVideo": {
    "input": {
      "required": {
        "images": [
          "IMAGE",
          {
            "tooltip": "The images to create a video from."
          }
        ],
        "fps": [
          "FLOAT",
          {
            "default": 30,
            "min": 1,
            "max": 120,
            "step": 1
          }
        ]
      },
      "optional": {
        "audio": [
          "AUDIO",
          {
            "tooltip": "The audio to add to the video."
          }
        ]
      }
    },
    "input_order": {
      "required": ["images", "fps"],
      "optional": ["audio"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "CreateVideo",
    "display_name": "Create Video",
    "description": "Create a video from images.",
    "python_module": "comfy_extras.nodes_video",
    "category": "image/video",
    "output_node": false
  },
  "GetVideoComponents": {
    "input": {
      "required": {
        "video": [
          "VIDEO",
          {
            "tooltip": "The video to extract components from."
          }
        ]
      }
    },
    "input_order": {
      "required": ["video"]
    },
    "output": ["IMAGE", "AUDIO", "FLOAT"],
    "output_is_list": [false, false, false],
    "output_name": ["images", "audio", "fps"],
    "name": "GetVideoComponents",
    "display_name": "Get Video Components",
    "description": "Extracts all components from a video: frames, audio, and framerate.",
    "python_module": "comfy_extras.nodes_video",
    "category": "image/video",
    "output_node": false
  },
  "LoadVideo": {
    "input": {
      "required": {
        "file": [
          [],
          {
            "video_upload": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["file"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "LoadVideo",
    "display_name": "Load Video",
    "description": "",
    "python_module": "comfy_extras.nodes_video",
    "category": "image/video",
    "output_node": false
  },
  "CLIPTextEncodeLumina2": {
    "input": {
      "required": {
        "system_prompt": [
          ["superior", "alignment"],
          {
            "tooltip": "Lumina2 provide two types of system prompts:Superior: You are an assistant designed to generate superior images with the superior degree of image-text alignment based on textual prompts or user prompts. Alignment: You are an assistant designed to generate high-quality images with the highest degree of image-text alignment based on textual prompts."
          }
        ],
        "user_prompt": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true,
            "tooltip": "The text to be encoded."
          }
        ],
        "clip": [
          "CLIP",
          {
            "tooltip": "The CLIP model used for encoding the text."
          }
        ]
      }
    },
    "input_order": {
      "required": ["system_prompt", "user_prompt", "clip"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeLumina2",
    "display_name": "CLIP Text Encode for Lumina2",
    "description": "Encodes a system prompt and a user prompt using a CLIP model into an embedding that can be used to guide the diffusion model towards generating specific images.",
    "python_module": "comfy_extras.nodes_lumina2",
    "category": "conditioning",
    "output_node": false,
    "output_tooltips": [
      "A conditioning containing the embedded text used to guide the diffusion model."
    ]
  },
  "RenormCFG": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "cfg_trunc": [
          "FLOAT",
          {
            "default": 100,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ],
        "renorm_cfg": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "cfg_trunc", "renorm_cfg"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "RenormCFG",
    "display_name": "RenormCFG",
    "description": "",
    "python_module": "comfy_extras.nodes_lumina2",
    "category": "advanced/model",
    "output_node": false
  },
  "WanTrackToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "tracks": [
          "STRING",
          {
            "multiline": true,
            "default": "[]"
          }
        ],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ],
        "temperature": [
          "FLOAT",
          {
            "default": 220,
            "min": 1,
            "max": 1000,
            "step": 0.1
          }
        ],
        "topk": [
          "INT",
          {
            "default": 2,
            "min": 1,
            "max": 10
          }
        ],
        "start_image": ["IMAGE"]
      },
      "optional": {
        "clip_vision_output": ["CLIP_VISION_OUTPUT"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "tracks",
        "width",
        "height",
        "length",
        "batch_size",
        "temperature",
        "topk",
        "start_image"
      ],
      "optional": ["clip_vision_output"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "WanTrackToVideo",
    "display_name": "WanTrackToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "WanImageToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "clip_vision_output": ["CLIP_VISION_OUTPUT"],
        "start_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "width",
        "height",
        "length",
        "batch_size"
      ],
      "optional": ["clip_vision_output", "start_image"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "WanImageToVideo",
    "display_name": "WanImageToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "WanFunControlToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "clip_vision_output": ["CLIP_VISION_OUTPUT"],
        "start_image": ["IMAGE"],
        "control_video": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "width",
        "height",
        "length",
        "batch_size"
      ],
      "optional": ["clip_vision_output", "start_image", "control_video"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "WanFunControlToVideo",
    "display_name": "WanFunControlToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "WanFunInpaintToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "clip_vision_output": ["CLIP_VISION_OUTPUT"],
        "start_image": ["IMAGE"],
        "end_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "width",
        "height",
        "length",
        "batch_size"
      ],
      "optional": ["clip_vision_output", "start_image", "end_image"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "WanFunInpaintToVideo",
    "display_name": "WanFunInpaintToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "WanFirstLastFrameToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "clip_vision_start_image": ["CLIP_VISION_OUTPUT"],
        "clip_vision_end_image": ["CLIP_VISION_OUTPUT"],
        "start_image": ["IMAGE"],
        "end_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "width",
        "height",
        "length",
        "batch_size"
      ],
      "optional": [
        "clip_vision_start_image",
        "clip_vision_end_image",
        "start_image",
        "end_image"
      ]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "WanFirstLastFrameToVideo",
    "display_name": "WanFirstLastFrameToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "WanVaceToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1000,
            "step": 0.01
          }
        ]
      },
      "optional": {
        "control_video": ["IMAGE"],
        "control_masks": ["MASK"],
        "reference_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "width",
        "height",
        "length",
        "batch_size",
        "strength"
      ],
      "optional": ["control_video", "control_masks", "reference_image"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT", "INT"],
    "output_is_list": [false, false, false, false],
    "output_name": ["positive", "negative", "latent", "trim_latent"],
    "name": "WanVaceToVideo",
    "display_name": "WanVaceToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false,
    "experimental": true
  },
  "TrimVideoLatent": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "trim_amount": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 99999
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "trim_amount"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "TrimVideoLatent",
    "display_name": "TrimVideoLatent",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "latent/video",
    "output_node": false,
    "experimental": true
  },
  "WanCameraImageToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "clip_vision_output": ["CLIP_VISION_OUTPUT"],
        "start_image": ["IMAGE"],
        "camera_conditions": ["WAN_CAMERA_EMBEDDING"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "width",
        "height",
        "length",
        "batch_size"
      ],
      "optional": ["clip_vision_output", "start_image", "camera_conditions"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false],
    "output_name": ["positive", "negative", "latent"],
    "name": "WanCameraImageToVideo",
    "display_name": "WanCameraImageToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "WanPhantomSubjectToVideo": {
    "input": {
      "required": {
        "positive": ["CONDITIONING"],
        "negative": ["CONDITIONING"],
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "images": ["IMAGE"]
      }
    },
    "input_order": {
      "required": [
        "positive",
        "negative",
        "vae",
        "width",
        "height",
        "length",
        "batch_size"
      ],
      "optional": ["images"]
    },
    "output": ["CONDITIONING", "CONDITIONING", "CONDITIONING", "LATENT"],
    "output_is_list": [false, false, false, false],
    "output_name": ["positive", "negative_text", "negative_img_text", "latent"],
    "name": "WanPhantomSubjectToVideo",
    "display_name": "WanPhantomSubjectToVideo",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "Wan22ImageToVideoLatent": {
    "input": {
      "required": {
        "vae": ["VAE"],
        "width": [
          "INT",
          {
            "default": 1280,
            "min": 32,
            "max": 16384,
            "step": 32
          }
        ],
        "height": [
          "INT",
          {
            "default": 704,
            "min": 32,
            "max": 16384,
            "step": 32
          }
        ],
        "length": [
          "INT",
          {
            "default": 49,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096
          }
        ]
      },
      "optional": {
        "start_image": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["vae", "width", "height", "length", "batch_size"],
      "optional": ["start_image"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "Wan22ImageToVideoLatent",
    "display_name": "Wan22ImageToVideoLatent",
    "description": "",
    "python_module": "comfy_extras.nodes_wan",
    "category": "conditioning/inpaint",
    "output_node": false
  },
  "LotusConditioning": {
    "input": {
      "required": {}
    },
    "input_order": {
      "required": []
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["conditioning"],
    "name": "LotusConditioning",
    "display_name": "LotusConditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_lotus",
    "category": "conditioning/lotus",
    "output_node": false
  },
  "EmptyLatentHunyuan3Dv2": {
    "input": {
      "required": {
        "resolution": [
          "INT",
          {
            "default": 3072,
            "min": 1,
            "max": 8192
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096,
            "tooltip": "The number of latent images in the batch."
          }
        ]
      }
    },
    "input_order": {
      "required": ["resolution", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyLatentHunyuan3Dv2",
    "display_name": "EmptyLatentHunyuan3Dv2",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan3d",
    "category": "latent/3d",
    "output_node": false
  },
  "Hunyuan3Dv2Conditioning": {
    "input": {
      "required": {
        "clip_vision_output": ["CLIP_VISION_OUTPUT"]
      }
    },
    "input_order": {
      "required": ["clip_vision_output"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "Hunyuan3Dv2Conditioning",
    "display_name": "Hunyuan3Dv2Conditioning",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan3d",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "Hunyuan3Dv2ConditioningMultiView": {
    "input": {
      "required": {},
      "optional": {
        "front": ["CLIP_VISION_OUTPUT"],
        "left": ["CLIP_VISION_OUTPUT"],
        "back": ["CLIP_VISION_OUTPUT"],
        "right": ["CLIP_VISION_OUTPUT"]
      }
    },
    "input_order": {
      "required": [],
      "optional": ["front", "left", "back", "right"]
    },
    "output": ["CONDITIONING", "CONDITIONING"],
    "output_is_list": [false, false],
    "output_name": ["positive", "negative"],
    "name": "Hunyuan3Dv2ConditioningMultiView",
    "display_name": "Hunyuan3Dv2ConditioningMultiView",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan3d",
    "category": "conditioning/video_models",
    "output_node": false
  },
  "VAEDecodeHunyuan3D": {
    "input": {
      "required": {
        "samples": ["LATENT"],
        "vae": ["VAE"],
        "num_chunks": [
          "INT",
          {
            "default": 8000,
            "min": 1000,
            "max": 500000
          }
        ],
        "octree_resolution": [
          "INT",
          {
            "default": 256,
            "min": 16,
            "max": 512
          }
        ]
      }
    },
    "input_order": {
      "required": ["samples", "vae", "num_chunks", "octree_resolution"]
    },
    "output": ["VOXEL"],
    "output_is_list": [false],
    "output_name": ["VOXEL"],
    "name": "VAEDecodeHunyuan3D",
    "display_name": "VAEDecodeHunyuan3D",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan3d",
    "category": "latent/3d",
    "output_node": false
  },
  "VoxelToMeshBasic": {
    "input": {
      "required": {
        "voxel": ["VOXEL"],
        "threshold": [
          "FLOAT",
          {
            "default": 0.6,
            "min": -1,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["voxel", "threshold"]
    },
    "output": ["MESH"],
    "output_is_list": [false],
    "output_name": ["MESH"],
    "name": "VoxelToMeshBasic",
    "display_name": "VoxelToMeshBasic",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan3d",
    "category": "3d",
    "output_node": false
  },
  "VoxelToMesh": {
    "input": {
      "required": {
        "voxel": ["VOXEL"],
        "algorithm": [["surface net", "basic"]],
        "threshold": [
          "FLOAT",
          {
            "default": 0.6,
            "min": -1,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["voxel", "algorithm", "threshold"]
    },
    "output": ["MESH"],
    "output_is_list": [false],
    "output_name": ["MESH"],
    "name": "VoxelToMesh",
    "display_name": "VoxelToMesh",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan3d",
    "category": "3d",
    "output_node": false
  },
  "SaveGLB": {
    "input": {
      "required": {
        "mesh": ["MESH"],
        "filename_prefix": [
          "STRING",
          {
            "default": "mesh/ComfyUI"
          }
        ]
      },
      "hidden": {
        "prompt": "PROMPT",
        "extra_pnginfo": "EXTRA_PNGINFO"
      }
    },
    "input_order": {
      "required": ["mesh", "filename_prefix"],
      "hidden": ["prompt", "extra_pnginfo"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveGLB",
    "display_name": "SaveGLB",
    "description": "",
    "python_module": "comfy_extras.nodes_hunyuan3d",
    "category": "3d",
    "output_node": true
  },
  "PrimitiveString": {
    "input": {
      "required": {
        "value": ["STRING", {}]
      }
    },
    "input_order": {
      "required": ["value"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "PrimitiveString",
    "display_name": "String",
    "description": "",
    "python_module": "comfy_extras.nodes_primitive",
    "category": "utils/primitive",
    "output_node": false
  },
  "PrimitiveStringMultiline": {
    "input": {
      "required": {
        "value": [
          "STRING",
          {
            "multiline": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["value"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "PrimitiveStringMultiline",
    "display_name": "String (Multiline)",
    "description": "",
    "python_module": "comfy_extras.nodes_primitive",
    "category": "utils/primitive",
    "output_node": false
  },
  "PrimitiveInt": {
    "input": {
      "required": {
        "value": [
          "INT",
          {
            "min": -9223372036854776000,
            "max": 9223372036854776000,
            "control_after_generate": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["value"]
    },
    "output": ["INT"],
    "output_is_list": [false],
    "output_name": ["INT"],
    "name": "PrimitiveInt",
    "display_name": "Int",
    "description": "",
    "python_module": "comfy_extras.nodes_primitive",
    "category": "utils/primitive",
    "output_node": false
  },
  "PrimitiveFloat": {
    "input": {
      "required": {
        "value": [
          "FLOAT",
          {
            "min": -9223372036854776000,
            "max": 9223372036854776000
          }
        ]
      }
    },
    "input_order": {
      "required": ["value"]
    },
    "output": ["FLOAT"],
    "output_is_list": [false],
    "output_name": ["FLOAT"],
    "name": "PrimitiveFloat",
    "display_name": "Float",
    "description": "",
    "python_module": "comfy_extras.nodes_primitive",
    "category": "utils/primitive",
    "output_node": false
  },
  "PrimitiveBoolean": {
    "input": {
      "required": {
        "value": ["BOOLEAN", {}]
      }
    },
    "input_order": {
      "required": ["value"]
    },
    "output": ["BOOLEAN"],
    "output_is_list": [false],
    "output_name": ["BOOLEAN"],
    "name": "PrimitiveBoolean",
    "display_name": "Boolean",
    "description": "",
    "python_module": "comfy_extras.nodes_primitive",
    "category": "utils/primitive",
    "output_node": false
  },
  "CFGZeroStar": {
    "input": {
      "required": {
        "model": ["MODEL"]
      }
    },
    "input_order": {
      "required": ["model"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["patched_model"],
    "name": "CFGZeroStar",
    "display_name": "CFGZeroStar",
    "description": "",
    "python_module": "comfy_extras.nodes_cfg",
    "category": "advanced/guidance",
    "output_node": false
  },
  "CFGNorm": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 100,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "strength"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["patched_model"],
    "name": "CFGNorm",
    "display_name": "CFGNorm",
    "description": "",
    "python_module": "comfy_extras.nodes_cfg",
    "category": "advanced/guidance",
    "output_node": false,
    "experimental": true
  },
  "OptimalStepsScheduler": {
    "input": {
      "required": {
        "model_type": [["FLUX", "Wan", "Chroma"]],
        "steps": [
          "INT",
          {
            "default": 20,
            "min": 3,
            "max": 1000
          }
        ],
        "denoise": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["model_type", "steps", "denoise"]
    },
    "output": ["SIGMAS"],
    "output_is_list": [false],
    "output_name": ["SIGMAS"],
    "name": "OptimalStepsScheduler",
    "display_name": "OptimalStepsScheduler",
    "description": "",
    "python_module": "comfy_extras.nodes_optimalsteps",
    "category": "sampling/custom_sampling/schedulers",
    "output_node": false
  },
  "QuadrupleCLIPLoader": {
    "input": {
      "required": {
        "clip_name1": [[]],
        "clip_name2": [[]],
        "clip_name3": [[]],
        "clip_name4": [[]]
      }
    },
    "input_order": {
      "required": ["clip_name1", "clip_name2", "clip_name3", "clip_name4"]
    },
    "output": ["CLIP"],
    "output_is_list": [false],
    "output_name": ["CLIP"],
    "name": "QuadrupleCLIPLoader",
    "display_name": "QuadrupleCLIPLoader",
    "description": "[Recipes]\n\nhidream: long clip-l, long clip-g, t5xxl, llama_8b_3.1_instruct",
    "python_module": "comfy_extras.nodes_hidream",
    "category": "advanced/loaders",
    "output_node": false
  },
  "CLIPTextEncodeHiDream": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "clip_l": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "clip_g": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "t5xxl": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "llama": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "clip_l", "clip_g", "t5xxl", "llama"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "CLIPTextEncodeHiDream",
    "display_name": "CLIPTextEncodeHiDream",
    "description": "",
    "python_module": "comfy_extras.nodes_hidream",
    "category": "advanced/conditioning",
    "output_node": false
  },
  "FreSca": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "scale_low": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01,
            "tooltip": "Scaling factor for low-frequency components"
          }
        ],
        "scale_high": [
          "FLOAT",
          {
            "default": 1.25,
            "min": 0,
            "max": 10,
            "step": 0.01,
            "tooltip": "Scaling factor for high-frequency components"
          }
        ],
        "freq_cutoff": [
          "INT",
          {
            "default": 20,
            "min": 1,
            "max": 10000,
            "step": 1,
            "tooltip": "Number of frequency indices around center to consider as low-frequency"
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "scale_low", "scale_high", "freq_cutoff"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "FreSca",
    "display_name": "FreSca",
    "description": "Applies frequency-dependent scaling to the guidance",
    "python_module": "comfy_extras.nodes_fresca",
    "category": "_for_testing",
    "output_node": false
  },
  "APG": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "eta": [
          "FLOAT",
          {
            "default": 1,
            "min": -10,
            "max": 10,
            "step": 0.01,
            "tooltip": "Controls the scale of the parallel guidance vector. Default CFG behavior at a setting of 1."
          }
        ],
        "norm_threshold": [
          "FLOAT",
          {
            "default": 5,
            "min": 0,
            "max": 50,
            "step": 0.1,
            "tooltip": "Normalize guidance vector to this value, normalization disable at a setting of 0."
          }
        ],
        "momentum": [
          "FLOAT",
          {
            "default": 0,
            "min": -5,
            "max": 1,
            "step": 0.01,
            "tooltip": "Controls a running average of guidance during diffusion, disabled at a setting of 0."
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "eta", "norm_threshold", "momentum"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "APG",
    "display_name": "Adaptive Projected Guidance",
    "description": "",
    "python_module": "comfy_extras.nodes_apg",
    "category": "sampling/custom_sampling",
    "output_node": false
  },
  "PreviewAny": {
    "input": {
      "required": {
        "source": ["*", {}]
      }
    },
    "input_order": {
      "required": ["source"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "PreviewAny",
    "display_name": "Preview Any",
    "description": "",
    "python_module": "comfy_extras.nodes_preview_any",
    "category": "utils",
    "output_node": true
  },
  "TextEncodeAceStepAudio": {
    "input": {
      "required": {
        "clip": ["CLIP"],
        "tags": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "lyrics": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "lyrics_strength": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["clip", "tags", "lyrics", "lyrics_strength"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "TextEncodeAceStepAudio",
    "display_name": "TextEncodeAceStepAudio",
    "description": "",
    "python_module": "comfy_extras.nodes_ace",
    "category": "conditioning",
    "output_node": false
  },
  "EmptyAceStepLatentAudio": {
    "input": {
      "required": {
        "seconds": [
          "FLOAT",
          {
            "default": 120,
            "min": 1,
            "max": 1000,
            "step": 0.1
          }
        ],
        "batch_size": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 4096,
            "tooltip": "The number of latent images in the batch."
          }
        ]
      }
    },
    "input_order": {
      "required": ["seconds", "batch_size"]
    },
    "output": ["LATENT"],
    "output_is_list": [false],
    "output_name": ["LATENT"],
    "name": "EmptyAceStepLatentAudio",
    "display_name": "EmptyAceStepLatentAudio",
    "description": "",
    "python_module": "comfy_extras.nodes_ace",
    "category": "latent/audio",
    "output_node": false
  },
  "StringConcatenate": {
    "input": {
      "required": {
        "string_a": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "string_b": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "delimiter": [
          "STRING",
          {
            "multiline": false,
            "default": ""
          }
        ]
      }
    },
    "input_order": {
      "required": ["string_a", "string_b", "delimiter"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "StringConcatenate",
    "display_name": "Concatenate",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "StringSubstring": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "start": ["INT", {}],
        "end": ["INT", {}]
      }
    },
    "input_order": {
      "required": ["string", "start", "end"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "StringSubstring",
    "display_name": "Substring",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "StringLength": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["string"]
    },
    "output": ["INT"],
    "output_is_list": [false],
    "output_name": ["length"],
    "name": "StringLength",
    "display_name": "Length",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "CaseConverter": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "mode": [
          "COMBO",
          {
            "options": ["UPPERCASE", "lowercase", "Capitalize", "Title Case"]
          }
        ]
      }
    },
    "input_order": {
      "required": ["string", "mode"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "CaseConverter",
    "display_name": "Case Converter",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "StringTrim": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "mode": [
          "COMBO",
          {
            "options": ["Both", "Left", "Right"]
          }
        ]
      }
    },
    "input_order": {
      "required": ["string", "mode"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "StringTrim",
    "display_name": "Trim",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "StringReplace": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "find": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "replace": [
          "STRING",
          {
            "multiline": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["string", "find", "replace"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "StringReplace",
    "display_name": "Replace",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "StringContains": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "substring": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "case_sensitive": [
          "BOOLEAN",
          {
            "default": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["string", "substring", "case_sensitive"]
    },
    "output": ["BOOLEAN"],
    "output_is_list": [false],
    "output_name": ["contains"],
    "name": "StringContains",
    "display_name": "Contains",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "StringCompare": {
    "input": {
      "required": {
        "string_a": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "string_b": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "mode": [
          "COMBO",
          {
            "options": ["Starts With", "Ends With", "Equal"]
          }
        ],
        "case_sensitive": [
          "BOOLEAN",
          {
            "default": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["string_a", "string_b", "mode", "case_sensitive"]
    },
    "output": ["BOOLEAN"],
    "output_is_list": [false],
    "output_name": ["BOOLEAN"],
    "name": "StringCompare",
    "display_name": "Compare",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "RegexMatch": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "regex_pattern": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "case_insensitive": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "multiline": [
          "BOOLEAN",
          {
            "default": false
          }
        ],
        "dotall": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "string",
        "regex_pattern",
        "case_insensitive",
        "multiline",
        "dotall"
      ]
    },
    "output": ["BOOLEAN"],
    "output_is_list": [false],
    "output_name": ["matches"],
    "name": "RegexMatch",
    "display_name": "Regex Match",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "RegexExtract": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "regex_pattern": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "mode": [
          "COMBO",
          {
            "options": [
              "First Match",
              "All Matches",
              "First Group",
              "All Groups"
            ]
          }
        ],
        "case_insensitive": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "multiline": [
          "BOOLEAN",
          {
            "default": false
          }
        ],
        "dotall": [
          "BOOLEAN",
          {
            "default": false
          }
        ],
        "group_index": [
          "INT",
          {
            "default": 1,
            "min": 0,
            "max": 100
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "string",
        "regex_pattern",
        "mode",
        "case_insensitive",
        "multiline",
        "dotall",
        "group_index"
      ]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "RegexExtract",
    "display_name": "Regex Extract",
    "description": "",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "RegexReplace": {
    "input": {
      "required": {
        "string": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "regex_pattern": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "replace": [
          "STRING",
          {
            "multiline": true
          }
        ]
      },
      "optional": {
        "case_insensitive": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "multiline": [
          "BOOLEAN",
          {
            "default": false
          }
        ],
        "dotall": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "When enabled, the dot (.) character will match any character including newline characters. When disabled, dots won't match newlines."
          }
        ],
        "count": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 100,
            "tooltip": "Maximum number of replacements to make. Set to 0 to replace all occurrences (default). Set to 1 to replace only the first match, 2 for the first two matches, etc."
          }
        ]
      }
    },
    "input_order": {
      "required": ["string", "regex_pattern", "replace"],
      "optional": ["case_insensitive", "multiline", "dotall", "count"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "RegexReplace",
    "display_name": "Regex Replace",
    "description": "Find and replace text using regex patterns.",
    "python_module": "comfy_extras.nodes_string",
    "category": "utils/string",
    "output_node": false
  },
  "WanCameraEmbedding": {
    "input": {
      "required": {
        "camera_pose": [
          [
            "Static",
            "Pan Up",
            "Pan Down",
            "Pan Left",
            "Pan Right",
            "Zoom In",
            "Zoom Out",
            "Anti Clockwise (ACW)",
            "ClockWise (CW)"
          ],
          {
            "default": "Static"
          }
        ],
        "width": [
          "INT",
          {
            "default": 832,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "height": [
          "INT",
          {
            "default": 480,
            "min": 16,
            "max": 16384,
            "step": 16
          }
        ],
        "length": [
          "INT",
          {
            "default": 81,
            "min": 1,
            "max": 16384,
            "step": 4
          }
        ]
      },
      "optional": {
        "speed": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.1
          }
        ],
        "fx": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 1e-9
          }
        ],
        "fy": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 1e-9
          }
        ],
        "cx": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ],
        "cy": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01
          }
        ]
      }
    },
    "input_order": {
      "required": ["camera_pose", "width", "height", "length"],
      "optional": ["speed", "fx", "fy", "cx", "cy"]
    },
    "output": ["WAN_CAMERA_EMBEDDING", "INT", "INT", "INT"],
    "output_is_list": [false, false, false, false],
    "output_name": ["camera_embedding", "width", "height", "length"],
    "name": "WanCameraEmbedding",
    "display_name": "WanCameraEmbedding",
    "description": "",
    "python_module": "comfy_extras.nodes_camera_trajectory",
    "category": "camera",
    "output_node": false
  },
  "ReferenceLatent": {
    "input": {
      "required": {
        "conditioning": ["CONDITIONING"]
      },
      "optional": {
        "latent": ["LATENT"]
      }
    },
    "input_order": {
      "required": ["conditioning"],
      "optional": ["latent"]
    },
    "output": ["CONDITIONING"],
    "output_is_list": [false],
    "output_name": ["CONDITIONING"],
    "name": "ReferenceLatent",
    "display_name": "ReferenceLatent",
    "description": "This node sets the guiding latent for an edit model. If the model supports it you can chain multiple to set multiple reference images.",
    "python_module": "comfy_extras.nodes_edit_model",
    "category": "advanced/conditioning/edit_models",
    "output_node": false
  },
  "TCFG": {
    "input": {
      "required": {
        "model": ["MODEL", {}]
      }
    },
    "input_order": {
      "required": ["model"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["patched_model"],
    "name": "TCFG",
    "display_name": "Tangential Damping CFG",
    "description": "TCFG – Tangential Damping CFG (2503.18137)\n\nRefine the uncond (negative) to align with the cond (positive) for improving quality.",
    "python_module": "comfy_extras.nodes_tcfg",
    "category": "advanced/guidance",
    "output_node": false
  },
  "IdeogramV1": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "turbo": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to use turbo mode (faster generation, potentially lower quality)"
          }
        ]
      },
      "optional": {
        "aspect_ratio": [
          "COMBO",
          {
            "options": [
              "1:1",
              "4:3",
              "3:4",
              "16:9",
              "9:16",
              "2:1",
              "1:2",
              "3:2",
              "2:3",
              "4:5",
              "5:4"
            ],
            "default": "1:1",
            "tooltip": "The aspect ratio for image generation."
          }
        ],
        "magic_prompt_option": [
          "COMBO",
          {
            "options": ["AUTO", "ON", "OFF"],
            "default": "AUTO",
            "tooltip": "Determine if MagicPrompt should be used in generation"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "step": 1,
            "control_after_generate": true,
            "display": "number"
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Description of what to exclude from the image"
          }
        ],
        "num_images": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 8,
            "step": 1,
            "display": "number"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "turbo"],
      "optional": [
        "aspect_ratio",
        "magic_prompt_option",
        "seed",
        "negative_prompt",
        "num_images"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "IdeogramV1",
    "display_name": "Ideogram V1",
    "description": "Generates images using the Ideogram V1 model.",
    "python_module": "comfy_api_nodes.nodes_ideogram",
    "category": "api node/image/Ideogram",
    "output_node": false,
    "api_node": true
  },
  "IdeogramV2": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "turbo": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to use turbo mode (faster generation, potentially lower quality)"
          }
        ]
      },
      "optional": {
        "aspect_ratio": [
          "COMBO",
          {
            "options": [
              "1:1",
              "4:3",
              "3:4",
              "16:9",
              "9:16",
              "2:1",
              "1:2",
              "3:2",
              "2:3",
              "4:5",
              "5:4"
            ],
            "default": "1:1",
            "tooltip": "The aspect ratio for image generation. Ignored if resolution is not set to AUTO."
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": [
              "Auto",
              "512 x 1536",
              "576 x 1408",
              "576 x 1472",
              "576 x 1536",
              "640 x 1024",
              "640 x 1344",
              "640 x 1408",
              "640 x 1472",
              "640 x 1536",
              "704 x 1152",
              "704 x 1216",
              "704 x 1280",
              "704 x 1344",
              "704 x 1408",
              "704 x 1472",
              "720 x 1280",
              "736 x 1312",
              "768 x 1024",
              "768 x 1088",
              "768 x 1152",
              "768 x 1216",
              "768 x 1232",
              "768 x 1280",
              "768 x 1344",
              "832 x 960",
              "832 x 1024",
              "832 x 1088",
              "832 x 1152",
              "832 x 1216",
              "832 x 1248",
              "864 x 1152",
              "896 x 960",
              "896 x 1024",
              "896 x 1088",
              "896 x 1120",
              "896 x 1152",
              "960 x 832",
              "960 x 896",
              "960 x 1024",
              "960 x 1088",
              "1024 x 640",
              "1024 x 768",
              "1024 x 832",
              "1024 x 896",
              "1024 x 960",
              "1024 x 1024",
              "1088 x 768",
              "1088 x 832",
              "1088 x 896",
              "1088 x 960",
              "1120 x 896",
              "1152 x 704",
              "1152 x 768",
              "1152 x 832",
              "1152 x 864",
              "1152 x 896",
              "1216 x 704",
              "1216 x 768",
              "1216 x 832",
              "1232 x 768",
              "1248 x 832",
              "1280 x 704",
              "1280 x 720",
              "1280 x 768",
              "1280 x 800",
              "1312 x 736",
              "1344 x 640",
              "1344 x 704",
              "1344 x 768",
              "1408 x 576",
              "1408 x 640",
              "1408 x 704",
              "1472 x 576",
              "1472 x 640",
              "1472 x 704",
              "1536 x 512",
              "1536 x 576",
              "1536 x 640"
            ],
            "default": "Auto",
            "tooltip": "The resolution for image generation. If not set to AUTO, this overrides the aspect_ratio setting."
          }
        ],
        "magic_prompt_option": [
          "COMBO",
          {
            "options": ["AUTO", "ON", "OFF"],
            "default": "AUTO",
            "tooltip": "Determine if MagicPrompt should be used in generation"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "step": 1,
            "control_after_generate": true,
            "display": "number"
          }
        ],
        "style_type": [
          "COMBO",
          {
            "options": [
              "AUTO",
              "GENERAL",
              "REALISTIC",
              "DESIGN",
              "RENDER_3D",
              "ANIME"
            ],
            "default": "NONE",
            "tooltip": "Style type for generation (V2 only)"
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Description of what to exclude from the image"
          }
        ],
        "num_images": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 8,
            "step": 1,
            "display": "number"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "turbo"],
      "optional": [
        "aspect_ratio",
        "resolution",
        "magic_prompt_option",
        "seed",
        "style_type",
        "negative_prompt",
        "num_images"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "IdeogramV2",
    "display_name": "Ideogram V2",
    "description": "Generates images using the Ideogram V2 model.",
    "python_module": "comfy_api_nodes.nodes_ideogram",
    "category": "api node/image/Ideogram",
    "output_node": false,
    "api_node": true
  },
  "IdeogramV3": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation or editing"
          }
        ]
      },
      "optional": {
        "image": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Optional reference image for image editing."
          }
        ],
        "mask": [
          "MASK",
          {
            "default": null,
            "tooltip": "Optional mask for inpainting (white areas will be replaced)"
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": [
              "1:3",
              "3:1",
              "1:2",
              "2:1",
              "9:16",
              "16:9",
              "10:16",
              "16:10",
              "2:3",
              "3:2",
              "3:4",
              "4:3",
              "4:5",
              "5:4",
              "1:1"
            ],
            "default": "1:1",
            "tooltip": "The aspect ratio for image generation. Ignored if resolution is not set to Auto."
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": [
              "Auto",
              "512x1536",
              "576x1408",
              "576x1472",
              "576x1536",
              "640x1344",
              "640x1408",
              "640x1472",
              "640x1536",
              "704x1152",
              "704x1216",
              "704x1280",
              "704x1344",
              "704x1408",
              "704x1472",
              "736x1312",
              "768x1088",
              "768x1216",
              "768x1280",
              "768x1344",
              "800x1280",
              "832x960",
              "832x1024",
              "832x1088",
              "832x1152",
              "832x1216",
              "832x1248",
              "864x1152",
              "896x960",
              "896x1024",
              "896x1088",
              "896x1120",
              "896x1152",
              "960x832",
              "960x896",
              "960x1024",
              "960x1088",
              "1024x832",
              "1024x896",
              "1024x960",
              "1024x1024",
              "1088x768",
              "1088x832",
              "1088x896",
              "1088x960",
              "1120x896",
              "1152x704",
              "1152x832",
              "1152x864",
              "1152x896",
              "1216x704",
              "1216x768",
              "1216x832",
              "1248x832",
              "1280x704",
              "1280x768",
              "1280x800",
              "1312x736",
              "1344x640",
              "1344x704",
              "1344x768",
              "1408x576",
              "1408x640",
              "1408x704",
              "1472x576",
              "1472x640",
              "1472x704",
              "1536x512",
              "1536x576",
              "1536x640"
            ],
            "default": "Auto",
            "tooltip": "The resolution for image generation. If not set to Auto, this overrides the aspect_ratio setting."
          }
        ],
        "magic_prompt_option": [
          "COMBO",
          {
            "options": ["AUTO", "ON", "OFF"],
            "default": "AUTO",
            "tooltip": "Determine if MagicPrompt should be used in generation"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "step": 1,
            "control_after_generate": true,
            "display": "number"
          }
        ],
        "num_images": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 8,
            "step": 1,
            "display": "number"
          }
        ],
        "rendering_speed": [
          "COMBO",
          {
            "options": ["BALANCED", "TURBO", "QUALITY"],
            "default": "BALANCED",
            "tooltip": "Controls the trade-off between generation speed and quality"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt"],
      "optional": [
        "image",
        "mask",
        "aspect_ratio",
        "resolution",
        "magic_prompt_option",
        "seed",
        "num_images",
        "rendering_speed"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "IdeogramV3",
    "display_name": "Ideogram V3",
    "description": "Generates images using the Ideogram V3 model. Supports both regular image generation from text prompts and image editing with mask.",
    "python_module": "comfy_api_nodes.nodes_ideogram",
    "category": "api node/image/Ideogram",
    "output_node": false,
    "api_node": true
  },
  "OpenAIDalle2": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text prompt for DALL·E"
          }
        ]
      },
      "optional": {
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "step": 1,
            "display": "number",
            "control_after_generate": true,
            "tooltip": "not implemented yet in backend"
          }
        ],
        "size": [
          "COMBO",
          {
            "options": ["256x256", "512x512", "1024x1024"],
            "default": "1024x1024",
            "tooltip": "Image size"
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 8,
            "step": 1,
            "display": "number",
            "tooltip": "How many images to generate"
          }
        ],
        "image": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Optional reference image for image editing."
          }
        ],
        "mask": [
          "MASK",
          {
            "default": null,
            "tooltip": "Optional mask for inpainting (white areas will be replaced)"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt"],
      "optional": ["seed", "size", "n", "image", "mask"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "OpenAIDalle2",
    "display_name": "OpenAI DALL·E 2",
    "description": "Generates images synchronously via OpenAI's DALL·E 2 endpoint.",
    "python_module": "comfy_api_nodes.nodes_openai",
    "category": "api node/image/OpenAI",
    "output_node": false,
    "api_node": true
  },
  "OpenAIDalle3": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text prompt for DALL·E"
          }
        ]
      },
      "optional": {
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "step": 1,
            "display": "number",
            "control_after_generate": true,
            "tooltip": "not implemented yet in backend"
          }
        ],
        "quality": [
          "COMBO",
          {
            "options": ["standard", "hd"],
            "default": "standard",
            "tooltip": "Image quality"
          }
        ],
        "style": [
          "COMBO",
          {
            "options": ["natural", "vivid"],
            "default": "natural",
            "tooltip": "Vivid causes the model to lean towards generating hyper-real and dramatic images. Natural causes the model to produce more natural, less hyper-real looking images."
          }
        ],
        "size": [
          "COMBO",
          {
            "options": ["1024x1024", "1024x1792", "1792x1024"],
            "default": "1024x1024",
            "tooltip": "Image size"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt"],
      "optional": ["seed", "quality", "style", "size"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "OpenAIDalle3",
    "display_name": "OpenAI DALL·E 3",
    "description": "Generates images synchronously via OpenAI's DALL·E 3 endpoint.",
    "python_module": "comfy_api_nodes.nodes_openai",
    "category": "api node/image/OpenAI",
    "output_node": false,
    "api_node": true
  },
  "OpenAIGPTImage1": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text prompt for GPT Image 1"
          }
        ]
      },
      "optional": {
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "step": 1,
            "display": "number",
            "control_after_generate": true,
            "tooltip": "not implemented yet in backend"
          }
        ],
        "quality": [
          "COMBO",
          {
            "options": ["low", "medium", "high"],
            "default": "low",
            "tooltip": "Image quality, affects cost and generation time."
          }
        ],
        "background": [
          "COMBO",
          {
            "options": ["opaque", "transparent"],
            "default": "opaque",
            "tooltip": "Return image with or without background"
          }
        ],
        "size": [
          "COMBO",
          {
            "options": ["auto", "1024x1024", "1024x1536", "1536x1024"],
            "default": "auto",
            "tooltip": "Image size"
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 8,
            "step": 1,
            "display": "number",
            "tooltip": "How many images to generate"
          }
        ],
        "image": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Optional reference image for image editing."
          }
        ],
        "mask": [
          "MASK",
          {
            "default": null,
            "tooltip": "Optional mask for inpainting (white areas will be replaced)"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt"],
      "optional": [
        "seed",
        "quality",
        "background",
        "size",
        "n",
        "image",
        "mask"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "OpenAIGPTImage1",
    "display_name": "OpenAI GPT Image 1",
    "description": "Generates images synchronously via OpenAI's GPT Image 1 endpoint.",
    "python_module": "comfy_api_nodes.nodes_openai",
    "category": "api node/image/OpenAI",
    "output_node": false,
    "api_node": true
  },
  "OpenAIChatNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text inputs to the model, used to generate a response."
          }
        ],
        "persist_context": [
          "BOOLEAN",
          {
            "default": true,
            "tooltip": "Persist chat context between calls (multi-turn conversation)"
          }
        ],
        "model": [
          "COMBO",
          {
            "options": [
              "o4-mini",
              "o1",
              "o3",
              "o1-pro",
              "gpt-4o",
              "gpt-4.1",
              "gpt-4.1-mini",
              "gpt-4.1-nano"
            ],
            "default": null,
            "tooltip": "The model used to generate the response"
          }
        ]
      },
      "optional": {
        "images": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Optional image(s) to use as context for the model. To include multiple images, you can use the Batch Images node."
          }
        ],
        "files": [
          "OPENAI_INPUT_FILES",
          {
            "default": null,
            "tooltip": "Optional file(s) to use as context for the model. Accepts inputs from the OpenAI Chat Input Files node."
          }
        ],
        "advanced_options": [
          "OPENAI_CHAT_CONFIG",
          {
            "default": null,
            "tooltip": "Optional configuration for the model. Accepts inputs from the OpenAI Chat Advanced Options node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "persist_context", "model"],
      "optional": ["images", "files", "advanced_options"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "OpenAIChatNode",
    "display_name": "OpenAI Chat",
    "description": "Generate text responses from an OpenAI model.",
    "python_module": "comfy_api_nodes.nodes_openai",
    "category": "api node/text/OpenAI",
    "output_node": false,
    "api_node": true
  },
  "OpenAIInputFiles": {
    "input": {
      "required": {
        "file": [
          "COMBO",
          {
            "tooltip": "Input files to include as context for the model. Only accepts text (.txt) and PDF (.pdf) files for now.",
            "options": [],
            "default": null
          }
        ]
      },
      "optional": {
        "OPENAI_INPUT_FILES": [
          "OPENAI_INPUT_FILES",
          {
            "tooltip": "An optional additional file(s) to batch together with the file loaded from this node. Allows chaining of input files so that a single message can include multiple input files.",
            "default": null
          }
        ]
      }
    },
    "input_order": {
      "required": ["file"],
      "optional": ["OPENAI_INPUT_FILES"]
    },
    "output": ["OPENAI_INPUT_FILES"],
    "output_is_list": [false],
    "output_name": ["OPENAI_INPUT_FILES"],
    "name": "OpenAIInputFiles",
    "display_name": "OpenAI Chat Input Files",
    "description": "Loads and prepares input files (text, pdf, etc.) to include as inputs for the OpenAI Chat Node. The files will be read by the OpenAI model when generating a response. 🛈 TIP: Can be chained together with other OpenAI Input File nodes.",
    "python_module": "comfy_api_nodes.nodes_openai",
    "category": "api node/text/OpenAI",
    "output_node": false
  },
  "OpenAIChatConfig": {
    "input": {
      "required": {
        "truncation": [
          "COMBO",
          {
            "options": ["auto", "disabled"],
            "default": "auto",
            "tooltip": "The truncation strategy to use for the model response. auto: If the context of this response and previous ones exceeds the model's context window size, the model will truncate the response to fit the context window by dropping input items in the middle of the conversation.disabled: If a model response will exceed the context window size for a model, the request will fail with a 400 error"
          }
        ]
      },
      "optional": {
        "max_output_tokens": [
          "INT",
          {
            "default": 4096,
            "tooltip": "An upper bound for the number of tokens that can be generated for a response, including visible output tokens",
            "min": 16,
            "max": 16384
          }
        ],
        "instructions": [
          "STRING",
          {
            "default": null,
            "tooltip": "Instructions for the model on how to generate the response",
            "multiline": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["truncation"],
      "optional": ["max_output_tokens", "instructions"]
    },
    "output": ["OPENAI_CHAT_CONFIG"],
    "output_is_list": [false],
    "output_name": ["OPENAI_CHAT_CONFIG"],
    "name": "OpenAIChatConfig",
    "display_name": "OpenAI Chat Advanced Options",
    "description": "Allows specifying advanced configuration options for the OpenAI Chat Nodes.",
    "python_module": "comfy_api_nodes.nodes_openai",
    "category": "api node/text/OpenAI",
    "output_node": false
  },
  "MinimaxTextToVideoNode": {
    "input": {
      "required": {
        "prompt_text": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text prompt to guide the video generation"
          }
        ],
        "model": [
          ["T2V-01", "T2V-01-Director"],
          {
            "default": "T2V-01",
            "tooltip": "Model to use for video generation"
          }
        ]
      },
      "optional": {
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt_text", "model"],
      "optional": ["seed"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "MinimaxTextToVideoNode",
    "display_name": "MiniMax Text to Video",
    "description": "Generates videos from prompts using MiniMax's API",
    "python_module": "comfy_api_nodes.nodes_minimax",
    "category": "api node/video/MiniMax",
    "output_node": true,
    "api_node": true
  },
  "MinimaxImageToVideoNode": {
    "input": {
      "required": {
        "image": [
          "IMAGE",
          {
            "tooltip": "Image to use as first frame of video generation"
          }
        ],
        "prompt_text": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text prompt to guide the video generation"
          }
        ],
        "model": [
          ["I2V-01-Director", "I2V-01", "I2V-01-live"],
          {
            "default": "I2V-01",
            "tooltip": "Model to use for video generation"
          }
        ]
      },
      "optional": {
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["image", "prompt_text", "model"],
      "optional": ["seed"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "MinimaxImageToVideoNode",
    "display_name": "MiniMax Image to Video",
    "description": "Generates videos from an image and prompts using MiniMax's API",
    "python_module": "comfy_api_nodes.nodes_minimax",
    "category": "api node/video/MiniMax",
    "output_node": true,
    "api_node": true
  },
  "VeoVideoGenerationNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text description of the video"
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": ["16:9", "9:16"],
            "default": "16:9",
            "tooltip": "Aspect ratio of the output video"
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Negative text prompt to guide what to avoid in the video"
          }
        ],
        "duration_seconds": [
          "INT",
          {
            "default": 5,
            "min": 5,
            "max": 8,
            "step": 1,
            "display": "number",
            "tooltip": "Duration of the output video in seconds"
          }
        ],
        "enhance_prompt": [
          "BOOLEAN",
          {
            "default": true,
            "tooltip": "Whether to enhance the prompt with AI assistance"
          }
        ],
        "person_generation": [
          "COMBO",
          {
            "options": ["ALLOW", "BLOCK"],
            "default": "ALLOW",
            "tooltip": "Whether to allow generating people in the video"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 4294967295,
            "step": 1,
            "display": "number",
            "control_after_generate": true,
            "tooltip": "Seed for video generation (0 for random)"
          }
        ],
        "image": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Optional reference image to guide video generation"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "aspect_ratio"],
      "optional": [
        "negative_prompt",
        "duration_seconds",
        "enhance_prompt",
        "person_generation",
        "seed",
        "image"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "VeoVideoGenerationNode",
    "display_name": "Google Veo2 Video Generation",
    "description": "Generates videos from text prompts using Google's Veo API",
    "python_module": "comfy_api_nodes.nodes_veo2",
    "category": "api node/video/Veo",
    "output_node": false,
    "api_node": true
  },
  "KlingCameraControls": {
    "input": {
      "required": {
        "camera_control_type": [
          "COMBO",
          {
            "options": [
              "simple",
              "down_back",
              "forward_up",
              "right_turn_forward",
              "left_turn_forward"
            ],
            "default": null
          }
        ],
        "horizontal_movement": [
          "FLOAT",
          {
            "default": 0,
            "min": -10,
            "max": 10,
            "step": 0.25,
            "display": "slider",
            "tooltip": "Controls camera's movement along horizontal axis (x-axis). Negative indicates left, positive indicates right"
          }
        ],
        "vertical_movement": [
          "FLOAT",
          {
            "default": 0,
            "min": -10,
            "max": 10,
            "step": 0.25,
            "display": "slider",
            "tooltip": "Controls camera's movement along vertical axis (y-axis). Negative indicates downward, positive indicates upward."
          }
        ],
        "pan": [
          "FLOAT",
          {
            "default": 0.5,
            "min": -10,
            "max": 10,
            "step": 0.25,
            "display": "slider",
            "tooltip": "Controls camera's rotation in vertical plane (x-axis). Negative indicates downward rotation, positive indicates upward rotation."
          }
        ],
        "tilt": [
          "FLOAT",
          {
            "default": 0,
            "min": -10,
            "max": 10,
            "step": 0.25,
            "display": "slider",
            "tooltip": "Controls camera's rotation in horizontal plane (y-axis). Negative indicates left rotation, positive indicates right rotation."
          }
        ],
        "roll": [
          "FLOAT",
          {
            "default": 0,
            "min": -10,
            "max": 10,
            "step": 0.25,
            "display": "slider",
            "tooltip": "Controls camera's rolling amount (z-axis). Negative indicates counterclockwise, positive indicates clockwise."
          }
        ],
        "zoom": [
          "FLOAT",
          {
            "default": 0,
            "min": -10,
            "max": 10,
            "step": 0.25,
            "display": "slider",
            "tooltip": "Controls change in camera's focal length. Negative indicates narrower field of view, positive indicates wider field of view."
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "camera_control_type",
        "horizontal_movement",
        "vertical_movement",
        "pan",
        "tilt",
        "roll",
        "zoom"
      ]
    },
    "output": ["CAMERA_CONTROL"],
    "output_is_list": [false],
    "output_name": ["camera_control"],
    "name": "KlingCameraControls",
    "display_name": "Kling Camera Controls",
    "description": "Allows specifying configuration options for Kling Camera Controls and motion control effects.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": false
  },
  "KlingTextToVideoNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Positive text prompt",
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Negative text prompt",
            "multiline": true
          }
        ],
        "cfg_scale": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": ["16:9", "9:16", "1:1"],
            "default": "16:9"
          }
        ],
        "mode": [
          [
            "standard mode / 5s duration / kling-v1",
            "standard mode / 10s duration / kling-v1",
            "pro mode / 5s duration / kling-v1",
            "pro mode / 10s duration / kling-v1",
            "standard mode / 5s duration / kling-v1-6",
            "standard mode / 10s duration / kling-v1-6",
            "pro mode / 5s duration / kling-v2-master",
            "pro mode / 10s duration / kling-v2-master",
            "standard mode / 5s duration / kling-v2-master",
            "standard mode / 10s duration / kling-v2-master"
          ],
          {
            "default": "standard mode / 5s duration / kling-v1-6",
            "tooltip": "The configuration to use for the video generation following the format: mode / duration / model_name."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "negative_prompt",
        "cfg_scale",
        "aspect_ratio",
        "mode"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingTextToVideoNode",
    "display_name": "Kling Text to Video",
    "description": "Kling Text to Video Node",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingImage2VideoNode": {
    "input": {
      "required": {
        "start_frame": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "The reference image used to generate the video."
          }
        ],
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Positive text prompt",
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Negative text prompt",
            "multiline": true
          }
        ],
        "model_name": [
          "COMBO",
          {
            "options": [
              "kling-v1",
              "kling-v1-5",
              "kling-v1-6",
              "kling-v2-master"
            ],
            "default": "kling-v2-master"
          }
        ],
        "cfg_scale": [
          "FLOAT",
          {
            "default": 0.8,
            "min": 0,
            "max": 1
          }
        ],
        "mode": [
          "COMBO",
          {
            "options": ["std", "pro"],
            "default": "std"
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": ["16:9", "9:16", "1:1"],
            "default": "16:9"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": ["5", "10"],
            "default": "5"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "start_frame",
        "prompt",
        "negative_prompt",
        "model_name",
        "cfg_scale",
        "mode",
        "aspect_ratio",
        "duration"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingImage2VideoNode",
    "display_name": "Kling Image to Video",
    "description": "Kling Image to Video Node",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingCameraControlI2VNode": {
    "input": {
      "required": {
        "start_frame": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Reference Image - URL or Base64 encoded string, cannot exceed 10MB, resolution not less than 300*300px, aspect ratio between 1:2.5 ~ 2.5:1. Base64 should not include data:image prefix."
          }
        ],
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Positive text prompt",
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Negative text prompt",
            "multiline": true
          }
        ],
        "cfg_scale": [
          "FLOAT",
          {
            "default": 0.75,
            "min": 0,
            "max": 1
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": ["16:9", "9:16", "1:1"],
            "default": "16:9"
          }
        ],
        "camera_control": [
          "CAMERA_CONTROL",
          {
            "tooltip": "Can be created using the Kling Camera Controls node. Controls the camera movement and motion during the video generation."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "start_frame",
        "prompt",
        "negative_prompt",
        "cfg_scale",
        "aspect_ratio",
        "camera_control"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingCameraControlI2VNode",
    "display_name": "Kling Image to Video (Camera Control)",
    "description": "Transform still images into cinematic videos with professional camera movements that simulate real-world cinematography. Control virtual camera actions including zoom, rotation, pan, tilt, and first-person view, while maintaining focus on your original image.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingCameraControlT2VNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Positive text prompt",
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Negative text prompt",
            "multiline": true
          }
        ],
        "cfg_scale": [
          "FLOAT",
          {
            "default": 0.75,
            "min": 0,
            "max": 1
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": ["16:9", "9:16", "1:1"],
            "default": "16:9"
          }
        ],
        "camera_control": [
          "CAMERA_CONTROL",
          {
            "tooltip": "Can be created using the Kling Camera Controls node. Controls the camera movement and motion during the video generation."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "negative_prompt",
        "cfg_scale",
        "aspect_ratio",
        "camera_control"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingCameraControlT2VNode",
    "display_name": "Kling Text to Video (Camera Control)",
    "description": "Transform text into cinematic videos with professional camera movements that simulate real-world cinematography. Control virtual camera actions including zoom, rotation, pan, tilt, and first-person view, while maintaining focus on your original text.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingStartEndFrameNode": {
    "input": {
      "required": {
        "start_frame": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Reference Image - URL or Base64 encoded string, cannot exceed 10MB, resolution not less than 300*300px, aspect ratio between 1:2.5 ~ 2.5:1. Base64 should not include data:image prefix."
          }
        ],
        "end_frame": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Reference Image - End frame control. URL or Base64 encoded string, cannot exceed 10MB, resolution not less than 300*300px. Base64 should not include data:image prefix."
          }
        ],
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Positive text prompt",
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Negative text prompt",
            "multiline": true
          }
        ],
        "cfg_scale": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": ["16:9", "9:16", "1:1"],
            "default": "16:9"
          }
        ],
        "mode": [
          [
            "standard mode / 5s duration / kling-v1",
            "pro mode / 5s duration / kling-v1",
            "pro mode / 5s duration / kling-v1-5",
            "pro mode / 10s duration / kling-v1-5",
            "pro mode / 5s duration / kling-v1-6",
            "pro mode / 10s duration / kling-v1-6"
          ],
          {
            "default": "pro mode / 5s duration / kling-v1-5",
            "tooltip": "The configuration to use for the video generation following the format: mode / duration / model_name."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "start_frame",
        "end_frame",
        "prompt",
        "negative_prompt",
        "cfg_scale",
        "aspect_ratio",
        "mode"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingStartEndFrameNode",
    "display_name": "Kling Start-End Frame to Video",
    "description": "Generate a video sequence that transitions between your provided start and end images. The node creates all frames in between, producing a smooth transformation from the first frame to the last.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingVideoExtendNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Positive text prompt for guiding the video extension",
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Negative text prompt for elements to avoid in the extended video",
            "multiline": true
          }
        ],
        "cfg_scale": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1
          }
        ],
        "video_id": [
          "STRING",
          {
            "default": null,
            "tooltip": "The ID of the video to be extended. Supports videos generated by text-to-video, image-to-video, and previous video extension operations. Cannot exceed 3 minutes total duration after extension.",
            "forceInput": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "negative_prompt", "cfg_scale", "video_id"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingVideoExtendNode",
    "display_name": "Kling Video Extend",
    "description": "Kling Video Extend Node. Extend videos made by other Kling nodes. The video_id is created by using other Kling Nodes.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingLipSyncAudioToVideoNode": {
    "input": {
      "required": {
        "video": ["VIDEO", {}],
        "audio": ["AUDIO", {}],
        "voice_language": [
          "COMBO",
          {
            "options": ["zh", "en"],
            "default": "en"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["video", "audio", "voice_language"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingLipSyncAudioToVideoNode",
    "display_name": "Kling Lip Sync Video with Audio",
    "description": "Kling Lip Sync Audio to Video Node. Syncs mouth movements in a video file to the audio content of an audio file. When using, ensure that the audio contains clearly distinguishable vocals and that the video contains a distinct face. The audio file should not be larger than 5MB. The video file should not be larger than 100MB, should have height/width between 720px and 1920px, and should be between 2s and 10s in length.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingLipSyncTextToVideoNode": {
    "input": {
      "required": {
        "video": ["VIDEO", {}],
        "text": [
          "STRING",
          {
            "default": null,
            "tooltip": "Text Content for Lip-Sync Video Generation. Required when mode is text2video. Maximum length is 120 characters.",
            "multiline": true
          }
        ],
        "voice": [
          [
            "Melody",
            "Sunny",
            "Sage",
            "Ace",
            "Blossom",
            "Peppy",
            "Dove",
            "Shine",
            "Anchor",
            "Lyric",
            "Tender",
            "Siren",
            "Zippy",
            "Bud",
            "Sprite",
            "Candy",
            "Beacon",
            "Rock",
            "Titan",
            "Grace",
            "Helen",
            "Lore",
            "Crag",
            "Prattle",
            "Hearth",
            "The Reader",
            "Commercial Lady",
            "阳光少年",
            "懂事小弟",
            "运动少年",
            "青春少女",
            "温柔小妹",
            "元气少女",
            "阳光男生",
            "幽默小哥",
            "文艺小哥",
            "甜美邻家",
            "温柔姐姐",
            "职场女青",
            "活泼男童",
            "俏皮女童",
            "稳重老爸",
            "温柔妈妈",
            "严肃上司",
            "优雅贵妇",
            "慈祥爷爷",
            "唠叨爷爷",
            "唠叨奶奶",
            "和蔼奶奶",
            "东北老铁",
            "重庆小伙",
            "四川妹子",
            "潮汕大叔",
            "台湾男生",
            "西安掌柜",
            "天津姐姐",
            "新闻播报男",
            "译制片男",
            "撒娇女友",
            "刀片烟嗓",
            "乖巧正太"
          ],
          {
            "default": "Melody"
          }
        ],
        "voice_speed": [
          "FLOAT",
          {
            "default": 1,
            "tooltip": "Speech Rate. Valid range: 0.8~2.0, accurate to one decimal place.",
            "min": 0.8,
            "max": 2,
            "slider": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["video", "text", "voice", "voice_speed"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingLipSyncTextToVideoNode",
    "display_name": "Kling Lip Sync Video with Text",
    "description": "Kling Lip Sync Text to Video Node. Syncs mouth movements in a video file to a text prompt. The video file should not be larger than 100MB, should have height/width between 720px and 1920px, and should be between 2s and 10s in length.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingVirtualTryOnNode": {
    "input": {
      "required": {
        "human_image": ["IMAGE", {}],
        "cloth_image": ["IMAGE", {}],
        "model_name": [
          "COMBO",
          {
            "options": [
              "kolors-virtual-try-on-v1",
              "kolors-virtual-try-on-v1-5"
            ],
            "default": "kolors-virtual-try-on-v1"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["human_image", "cloth_image", "model_name"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "KlingVirtualTryOnNode",
    "display_name": "Kling Virtual Try On",
    "description": "Kling Virtual Try On Node. Input a human image and a cloth image to try on the cloth on the human. You can merge multiple clothing item pictures into one image with a white background.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/image/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingImageGenerationNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "tooltip": "Positive text prompt",
            "multiline": true,
            "max_length": 500
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Negative text prompt",
            "multiline": true
          }
        ],
        "image_type": [
          "COMBO",
          {
            "options": ["subject", "face"],
            "default": null
          }
        ],
        "image_fidelity": [
          "FLOAT",
          {
            "default": 0.5,
            "tooltip": "Reference intensity for user-uploaded images",
            "min": 0,
            "max": 1,
            "slider": true,
            "step": 0.01
          }
        ],
        "human_fidelity": [
          "FLOAT",
          {
            "default": 0.45,
            "tooltip": "Subject reference similarity",
            "min": 0,
            "max": 1,
            "slider": true,
            "step": 0.01
          }
        ],
        "model_name": [
          "COMBO",
          {
            "options": ["kling-v1", "kling-v1-5", "kling-v2"],
            "default": "kling-v1"
          }
        ],
        "aspect_ratio": [
          "COMBO",
          {
            "options": [
              "16:9",
              "9:16",
              "1:1",
              "4:3",
              "3:4",
              "3:2",
              "2:3",
              "21:9"
            ],
            "default": "16:9"
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "tooltip": "Number of generated images",
            "min": 1,
            "max": 9
          }
        ]
      },
      "optional": {
        "image": ["IMAGE", {}]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "negative_prompt",
        "image_type",
        "image_fidelity",
        "human_fidelity",
        "model_name",
        "aspect_ratio",
        "n"
      ],
      "optional": ["image"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "KlingImageGenerationNode",
    "display_name": "Kling Image Generation",
    "description": "Kling Image Generation Node. Generate an image from a text prompt with an optional reference image.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/image/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingSingleImageVideoEffectNode": {
    "input": {
      "required": {
        "image": [
          "IMAGE",
          {
            "tooltip": " Reference Image. URL or Base64 encoded string (without data:image prefix). File size cannot exceed 10MB, resolution not less than 300*300px, aspect ratio between 1:2.5 ~ 2.5:1"
          }
        ],
        "effect_scene": [
          "COMBO",
          {
            "options": [
              "bloombloom",
              "dizzydizzy",
              "fuzzyfuzzy",
              "squish",
              "expansion"
            ]
          }
        ],
        "model_name": [
          "COMBO",
          {
            "options": ["kling-v1-6"]
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": ["5", "10"]
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["image", "effect_scene", "model_name", "duration"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING", "STRING"],
    "output_is_list": [false, false, false],
    "output_name": ["VIDEO", "video_id", "duration"],
    "name": "KlingSingleImageVideoEffectNode",
    "display_name": "Kling Video Effects",
    "description": "Achieve different special effects when generating a video based on the effect_scene.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "KlingDualCharacterVideoEffectNode": {
    "input": {
      "required": {
        "image_left": [
          "IMAGE",
          {
            "tooltip": "Left side image"
          }
        ],
        "image_right": [
          "IMAGE",
          {
            "tooltip": "Right side image"
          }
        ],
        "effect_scene": [
          "COMBO",
          {
            "options": ["hug", "kiss", "heart_gesture"]
          }
        ],
        "model_name": [
          "COMBO",
          {
            "options": ["kling-v1", "kling-v1-5", "kling-v1-6"],
            "default": "kling-v1"
          }
        ],
        "mode": [
          "COMBO",
          {
            "options": ["std", "pro"],
            "default": "std"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": ["5", "10"]
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "image_left",
        "image_right",
        "effect_scene",
        "model_name",
        "mode",
        "duration"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO", "STRING"],
    "output_is_list": [false, false],
    "output_name": ["VIDEO", "duration"],
    "name": "KlingDualCharacterVideoEffectNode",
    "display_name": "Kling Dual Character Video Effects",
    "description": "Achieve different special effects when generating a video based on the effect_scene. First image will be positioned on left side, second on right side of the composite.",
    "python_module": "comfy_api_nodes.nodes_kling",
    "category": "api node/video/Kling",
    "output_node": false,
    "api_node": true
  },
  "FluxProUltraImageNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "prompt_upsampling": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to perform upsampling on the prompt. If active, automatically modifies the prompt for more creative generation, but results are nondeterministic (same seed will not produce exactly the same result)."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ],
        "aspect_ratio": [
          "STRING",
          {
            "default": "16:9",
            "tooltip": "Aspect ratio of image; must be between 1:4 and 4:1."
          }
        ],
        "raw": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "When True, generate less processed, more natural-looking images."
          }
        ]
      },
      "optional": {
        "image_prompt": ["IMAGE"],
        "image_prompt_strength": [
          "FLOAT",
          {
            "default": 0.1,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "tooltip": "Blend between the prompt and the image prompt."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "prompt_upsampling",
        "seed",
        "aspect_ratio",
        "raw"
      ],
      "optional": ["image_prompt", "image_prompt_strength"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxProUltraImageNode",
    "display_name": "Flux 1.1 [pro] Ultra Image",
    "description": "Generates images using Flux Pro 1.1 Ultra via api based on prompt and resolution.",
    "python_module": "comfy_api_nodes.nodes_bfl",
    "category": "api node/image/BFL",
    "output_node": false,
    "api_node": true
  },
  "FluxKontextProImageNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation - specify what and how to edit."
          }
        ],
        "aspect_ratio": [
          "STRING",
          {
            "default": "16:9",
            "tooltip": "Aspect ratio of image; must be between 1:4 and 4:1."
          }
        ],
        "guidance": [
          "FLOAT",
          {
            "default": 3,
            "min": 0.1,
            "max": 99,
            "step": 0.1,
            "tooltip": "Guidance strength for the image generation process"
          }
        ],
        "steps": [
          "INT",
          {
            "default": 50,
            "min": 1,
            "max": 150,
            "tooltip": "Number of steps for the image generation process"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 1234,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ],
        "prompt_upsampling": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to perform upsampling on the prompt. If active, automatically modifies the prompt for more creative generation, but results are nondeterministic (same seed will not produce exactly the same result)."
          }
        ]
      },
      "optional": {
        "input_image": ["IMAGE"]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "aspect_ratio",
        "guidance",
        "steps",
        "seed",
        "prompt_upsampling"
      ],
      "optional": ["input_image"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxKontextProImageNode",
    "display_name": "Flux.1 Kontext [pro] Image",
    "description": "Edits images using Flux.1 Kontext [pro] via api based on prompt and aspect ratio.",
    "python_module": "comfy_api_nodes.nodes_bfl",
    "category": "api node/image/BFL",
    "output_node": false,
    "api_node": true
  },
  "FluxKontextMaxImageNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation - specify what and how to edit."
          }
        ],
        "aspect_ratio": [
          "STRING",
          {
            "default": "16:9",
            "tooltip": "Aspect ratio of image; must be between 1:4 and 4:1."
          }
        ],
        "guidance": [
          "FLOAT",
          {
            "default": 3,
            "min": 0.1,
            "max": 99,
            "step": 0.1,
            "tooltip": "Guidance strength for the image generation process"
          }
        ],
        "steps": [
          "INT",
          {
            "default": 50,
            "min": 1,
            "max": 150,
            "tooltip": "Number of steps for the image generation process"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 1234,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ],
        "prompt_upsampling": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to perform upsampling on the prompt. If active, automatically modifies the prompt for more creative generation, but results are nondeterministic (same seed will not produce exactly the same result)."
          }
        ]
      },
      "optional": {
        "input_image": ["IMAGE"]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "aspect_ratio",
        "guidance",
        "steps",
        "seed",
        "prompt_upsampling"
      ],
      "optional": ["input_image"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxKontextMaxImageNode",
    "display_name": "Flux.1 Kontext [max] Image",
    "description": "Edits images using Flux.1 Kontext [max] via api based on prompt and aspect ratio.",
    "python_module": "comfy_api_nodes.nodes_bfl",
    "category": "api node/image/BFL",
    "output_node": false,
    "api_node": true
  },
  "FluxProExpandNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "prompt_upsampling": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to perform upsampling on the prompt. If active, automatically modifies the prompt for more creative generation, but results are nondeterministic (same seed will not produce exactly the same result)."
          }
        ],
        "top": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2048,
            "tooltip": "Number of pixels to expand at the top of the image"
          }
        ],
        "bottom": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2048,
            "tooltip": "Number of pixels to expand at the bottom of the image"
          }
        ],
        "left": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2048,
            "tooltip": "Number of pixels to expand at the left side of the image"
          }
        ],
        "right": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2048,
            "tooltip": "Number of pixels to expand at the right side of the image"
          }
        ],
        "guidance": [
          "FLOAT",
          {
            "default": 60,
            "min": 1.5,
            "max": 100,
            "tooltip": "Guidance strength for the image generation process"
          }
        ],
        "steps": [
          "INT",
          {
            "default": 50,
            "min": 15,
            "max": 50,
            "tooltip": "Number of steps for the image generation process"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "image",
        "prompt",
        "prompt_upsampling",
        "top",
        "bottom",
        "left",
        "right",
        "guidance",
        "steps",
        "seed"
      ],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxProExpandNode",
    "display_name": "Flux.1 Expand Image",
    "description": "Outpaints image based on prompt.",
    "python_module": "comfy_api_nodes.nodes_bfl",
    "category": "api node/image/BFL",
    "output_node": false,
    "api_node": true
  },
  "FluxProFillNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "mask": ["MASK"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "prompt_upsampling": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to perform upsampling on the prompt. If active, automatically modifies the prompt for more creative generation, but results are nondeterministic (same seed will not produce exactly the same result)."
          }
        ],
        "guidance": [
          "FLOAT",
          {
            "default": 60,
            "min": 1.5,
            "max": 100,
            "tooltip": "Guidance strength for the image generation process"
          }
        ],
        "steps": [
          "INT",
          {
            "default": 50,
            "min": 15,
            "max": 50,
            "tooltip": "Number of steps for the image generation process"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "image",
        "mask",
        "prompt",
        "prompt_upsampling",
        "guidance",
        "steps",
        "seed"
      ],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxProFillNode",
    "display_name": "Flux.1 Fill Image",
    "description": "Inpaints image based on mask and prompt.",
    "python_module": "comfy_api_nodes.nodes_bfl",
    "category": "api node/image/BFL",
    "output_node": false,
    "api_node": true
  },
  "FluxProCannyNode": {
    "input": {
      "required": {
        "control_image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "prompt_upsampling": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to perform upsampling on the prompt. If active, automatically modifies the prompt for more creative generation, but results are nondeterministic (same seed will not produce exactly the same result)."
          }
        ],
        "canny_low_threshold": [
          "FLOAT",
          {
            "default": 0.1,
            "min": 0.01,
            "max": 0.99,
            "step": 0.01,
            "tooltip": "Low threshold for Canny edge detection; ignored if skip_processing is True"
          }
        ],
        "canny_high_threshold": [
          "FLOAT",
          {
            "default": 0.4,
            "min": 0.01,
            "max": 0.99,
            "step": 0.01,
            "tooltip": "High threshold for Canny edge detection; ignored if skip_processing is True"
          }
        ],
        "skip_preprocessing": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to skip preprocessing; set to True if control_image already is canny-fied, False if it is a raw image."
          }
        ],
        "guidance": [
          "FLOAT",
          {
            "default": 30,
            "min": 1,
            "max": 100,
            "tooltip": "Guidance strength for the image generation process"
          }
        ],
        "steps": [
          "INT",
          {
            "default": 50,
            "min": 15,
            "max": 50,
            "tooltip": "Number of steps for the image generation process"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "control_image",
        "prompt",
        "prompt_upsampling",
        "canny_low_threshold",
        "canny_high_threshold",
        "skip_preprocessing",
        "guidance",
        "steps",
        "seed"
      ],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxProCannyNode",
    "display_name": "Flux.1 Canny Control Image",
    "description": "Generate image using a control image (canny).",
    "python_module": "comfy_api_nodes.nodes_bfl",
    "category": "api node/image/BFL",
    "output_node": false,
    "api_node": true
  },
  "FluxProDepthNode": {
    "input": {
      "required": {
        "control_image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "prompt_upsampling": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to perform upsampling on the prompt. If active, automatically modifies the prompt for more creative generation, but results are nondeterministic (same seed will not produce exactly the same result)."
          }
        ],
        "skip_preprocessing": [
          "BOOLEAN",
          {
            "default": false,
            "tooltip": "Whether to skip preprocessing; set to True if control_image already is depth-ified, False if it is a raw image."
          }
        ],
        "guidance": [
          "FLOAT",
          {
            "default": 15,
            "min": 1,
            "max": 100,
            "tooltip": "Guidance strength for the image generation process"
          }
        ],
        "steps": [
          "INT",
          {
            "default": 50,
            "min": 15,
            "max": 50,
            "tooltip": "Number of steps for the image generation process"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "control_image",
        "prompt",
        "prompt_upsampling",
        "skip_preprocessing",
        "guidance",
        "steps",
        "seed"
      ],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "FluxProDepthNode",
    "display_name": "Flux.1 Depth Control Image",
    "description": "Generate image using a control image (depth).",
    "python_module": "comfy_api_nodes.nodes_bfl",
    "category": "api node/image/BFL",
    "output_node": false,
    "api_node": true
  },
  "LumaImageNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "model": [["photon-1", "photon-flash-1"]],
        "aspect_ratio": [
          ["1:1", "16:9", "9:16", "4:3", "3:4", "21:9", "9:21"],
          {
            "default": "16:9"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ],
        "style_image_weight": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "tooltip": "Weight of style image. Ignored if no style_image provided."
          }
        ]
      },
      "optional": {
        "image_luma_ref": [
          "LUMA_REF",
          {
            "tooltip": "Luma Reference node connection to influence generation with input images; up to 4 images can be considered."
          }
        ],
        "style_image": [
          "IMAGE",
          {
            "tooltip": "Style reference image; only 1 image will be used."
          }
        ],
        "character_image": [
          "IMAGE",
          {
            "tooltip": "Character reference images; can be a batch of multiple, up to 4 images can be considered."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "model",
        "aspect_ratio",
        "seed",
        "style_image_weight"
      ],
      "optional": ["image_luma_ref", "style_image", "character_image"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "LumaImageNode",
    "display_name": "Luma Text to Image",
    "description": "Generates images synchronously based on prompt and aspect ratio.",
    "python_module": "comfy_api_nodes.nodes_luma",
    "category": "api node/image/Luma",
    "output_node": false,
    "api_node": true
  },
  "LumaImageModifyNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation"
          }
        ],
        "image_weight": [
          "FLOAT",
          {
            "default": 0.1,
            "min": 0,
            "max": 0.98,
            "step": 0.01,
            "tooltip": "Weight of the image; the closer to 1.0, the less the image will be modified."
          }
        ],
        "model": [["photon-1", "photon-flash-1"]],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["image", "prompt", "image_weight", "model", "seed"],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "LumaImageModifyNode",
    "display_name": "Luma Image to Image",
    "description": "Modifies images synchronously based on prompt and aspect ratio.",
    "python_module": "comfy_api_nodes.nodes_luma",
    "category": "api node/image/Luma",
    "output_node": false,
    "api_node": true
  },
  "LumaVideoNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the video generation"
          }
        ],
        "model": [["ray-2", "ray-flash-2", "ray-1-6"]],
        "aspect_ratio": [
          ["1:1", "16:9", "9:16", "4:3", "3:4", "21:9", "9:21"],
          {
            "default": "16:9"
          }
        ],
        "resolution": [
          ["540p", "720p", "1080p", "4k"],
          {
            "default": "540p"
          }
        ],
        "duration": [["5s", "9s"]],
        "loop": [
          "BOOLEAN",
          {
            "default": false
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {
        "luma_concepts": [
          "LUMA_CONCEPTS",
          {
            "tooltip": "Optional Camera Concepts to dictate camera motion via the Luma Concepts node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "model",
        "aspect_ratio",
        "resolution",
        "duration",
        "loop",
        "seed"
      ],
      "optional": ["luma_concepts"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "LumaVideoNode",
    "display_name": "Luma Text to Video",
    "description": "Generates videos synchronously based on prompt and output_size.",
    "python_module": "comfy_api_nodes.nodes_luma",
    "category": "api node/video/Luma",
    "output_node": false,
    "api_node": true
  },
  "LumaImageToVideoNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the video generation"
          }
        ],
        "model": [["ray-2", "ray-flash-2", "ray-1-6"]],
        "resolution": [
          ["540p", "720p", "1080p", "4k"],
          {
            "default": "540p"
          }
        ],
        "duration": [["5s", "9s"]],
        "loop": [
          "BOOLEAN",
          {
            "default": false
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {
        "first_image": [
          "IMAGE",
          {
            "tooltip": "First frame of generated video."
          }
        ],
        "last_image": [
          "IMAGE",
          {
            "tooltip": "Last frame of generated video."
          }
        ],
        "luma_concepts": [
          "LUMA_CONCEPTS",
          {
            "tooltip": "Optional Camera Concepts to dictate camera motion via the Luma Concepts node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "model", "resolution", "duration", "loop", "seed"],
      "optional": ["first_image", "last_image", "luma_concepts"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "LumaImageToVideoNode",
    "display_name": "Luma Image to Video",
    "description": "Generates videos synchronously based on prompt, input images, and output_size.",
    "python_module": "comfy_api_nodes.nodes_luma",
    "category": "api node/video/Luma",
    "output_node": false,
    "api_node": true
  },
  "LumaReferenceNode": {
    "input": {
      "required": {
        "image": [
          "IMAGE",
          {
            "tooltip": "Image to use as reference."
          }
        ],
        "weight": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "tooltip": "Weight of image reference."
          }
        ]
      },
      "optional": {
        "luma_ref": ["LUMA_REF"]
      }
    },
    "input_order": {
      "required": ["image", "weight"],
      "optional": ["luma_ref"]
    },
    "output": ["LUMA_REF"],
    "output_is_list": [false],
    "output_name": ["luma_ref"],
    "name": "LumaReferenceNode",
    "display_name": "Luma Reference",
    "description": "Holds an image and weight for use with Luma Generate Image node.",
    "python_module": "comfy_api_nodes.nodes_luma",
    "category": "api node/image/Luma",
    "output_node": false
  },
  "LumaConceptsNode": {
    "input": {
      "required": {
        "concept1": [
          [
            "None",
            "truck_left",
            "pan_right",
            "pedestal_down",
            "low_angle",
            "pedestal_up",
            "selfie",
            "pan_left",
            "roll_right",
            "zoom_in",
            "over_the_shoulder",
            "orbit_right",
            "orbit_left",
            "static",
            "tiny_planet",
            "high_angle",
            "bolt_cam",
            "dolly_zoom",
            "overhead",
            "zoom_out",
            "handheld",
            "roll_left",
            "pov",
            "aerial_drone",
            "push_in",
            "crane_down",
            "truck_right",
            "tilt_down",
            "elevator_doors",
            "tilt_up",
            "ground_level",
            "pull_out",
            "aerial",
            "crane_up",
            "eye_level"
          ]
        ],
        "concept2": [
          [
            "None",
            "truck_left",
            "pan_right",
            "pedestal_down",
            "low_angle",
            "pedestal_up",
            "selfie",
            "pan_left",
            "roll_right",
            "zoom_in",
            "over_the_shoulder",
            "orbit_right",
            "orbit_left",
            "static",
            "tiny_planet",
            "high_angle",
            "bolt_cam",
            "dolly_zoom",
            "overhead",
            "zoom_out",
            "handheld",
            "roll_left",
            "pov",
            "aerial_drone",
            "push_in",
            "crane_down",
            "truck_right",
            "tilt_down",
            "elevator_doors",
            "tilt_up",
            "ground_level",
            "pull_out",
            "aerial",
            "crane_up",
            "eye_level"
          ]
        ],
        "concept3": [
          [
            "None",
            "truck_left",
            "pan_right",
            "pedestal_down",
            "low_angle",
            "pedestal_up",
            "selfie",
            "pan_left",
            "roll_right",
            "zoom_in",
            "over_the_shoulder",
            "orbit_right",
            "orbit_left",
            "static",
            "tiny_planet",
            "high_angle",
            "bolt_cam",
            "dolly_zoom",
            "overhead",
            "zoom_out",
            "handheld",
            "roll_left",
            "pov",
            "aerial_drone",
            "push_in",
            "crane_down",
            "truck_right",
            "tilt_down",
            "elevator_doors",
            "tilt_up",
            "ground_level",
            "pull_out",
            "aerial",
            "crane_up",
            "eye_level"
          ]
        ],
        "concept4": [
          [
            "None",
            "truck_left",
            "pan_right",
            "pedestal_down",
            "low_angle",
            "pedestal_up",
            "selfie",
            "pan_left",
            "roll_right",
            "zoom_in",
            "over_the_shoulder",
            "orbit_right",
            "orbit_left",
            "static",
            "tiny_planet",
            "high_angle",
            "bolt_cam",
            "dolly_zoom",
            "overhead",
            "zoom_out",
            "handheld",
            "roll_left",
            "pov",
            "aerial_drone",
            "push_in",
            "crane_down",
            "truck_right",
            "tilt_down",
            "elevator_doors",
            "tilt_up",
            "ground_level",
            "pull_out",
            "aerial",
            "crane_up",
            "eye_level"
          ]
        ]
      },
      "optional": {
        "luma_concepts": [
          "LUMA_CONCEPTS",
          {
            "tooltip": "Optional Camera Concepts to add to the ones chosen here."
          }
        ]
      }
    },
    "input_order": {
      "required": ["concept1", "concept2", "concept3", "concept4"],
      "optional": ["luma_concepts"]
    },
    "output": ["LUMA_CONCEPTS"],
    "output_is_list": [false],
    "output_name": ["luma_concepts"],
    "name": "LumaConceptsNode",
    "display_name": "Luma Concepts",
    "description": "Holds one or more Camera Concepts for use with Luma Text to Video and Luma Image to Video nodes.",
    "python_module": "comfy_api_nodes.nodes_luma",
    "category": "api node/video/Luma",
    "output_node": false
  },
  "RecraftTextToImageNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation."
          }
        ],
        "size": [
          [
            "1024x1024",
            "1365x1024",
            "1024x1365",
            "1536x1024",
            "1024x1536",
            "1820x1024",
            "1024x1820",
            "1024x2048",
            "2048x1024",
            "1434x1024",
            "1024x1434",
            "1024x1280",
            "1280x1024",
            "1024x1707",
            "1707x1024"
          ],
          {
            "default": "1024x1024",
            "tooltip": "The size of the generated image."
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 6,
            "tooltip": "The number of images to generate."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {
        "recraft_style": ["RECRAFT_V3_STYLE"],
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ],
        "recraft_controls": [
          "RECRAFT_CONTROLS",
          {
            "tooltip": "Optional additional controls over the generation via the Recraft Controls node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "size", "n", "seed"],
      "optional": ["recraft_style", "negative_prompt", "recraft_controls"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RecraftTextToImageNode",
    "display_name": "Recraft Text to Image",
    "description": "Generates images synchronously based on prompt and resolution.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftImageToImageNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation."
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 6,
            "tooltip": "The number of images to generate."
          }
        ],
        "strength": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "tooltip": "Defines the difference with the original image, should lie in [0, 1], where 0 means almost identical, and 1 means miserable similarity."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {
        "recraft_style": ["RECRAFT_V3_STYLE"],
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ],
        "recraft_controls": [
          "RECRAFT_CONTROLS",
          {
            "tooltip": "Optional additional controls over the generation via the Recraft Controls node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image", "prompt", "n", "strength", "seed"],
      "optional": ["recraft_style", "negative_prompt", "recraft_controls"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RecraftImageToImageNode",
    "display_name": "Recraft Image to Image",
    "description": "Modify image based on prompt and strength.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftImageInpaintingNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "mask": ["MASK"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation."
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 6,
            "tooltip": "The number of images to generate."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {
        "recraft_style": ["RECRAFT_V3_STYLE"],
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image", "mask", "prompt", "n", "seed"],
      "optional": ["recraft_style", "negative_prompt"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RecraftImageInpaintingNode",
    "display_name": "Recraft Image Inpainting",
    "description": "Modify image based on prompt and mask.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftTextToVectorNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation."
          }
        ],
        "substyle": [
          [
            "None",
            "bold_stroke",
            "chemistry",
            "colored_stencil",
            "contour_pop_art",
            "cosmics",
            "cutout",
            "depressive",
            "editorial",
            "emotional_flat",
            "engraving",
            "infographical",
            "line_art",
            "line_circuit",
            "linocut",
            "marker_outline",
            "mosaic",
            "naivector",
            "roundish_flat",
            "seamless",
            "segmented_colors",
            "sharp_contrast",
            "thin",
            "vector_photo",
            "vivid_shapes"
          ]
        ],
        "size": [
          [
            "1024x1024",
            "1365x1024",
            "1024x1365",
            "1536x1024",
            "1024x1536",
            "1820x1024",
            "1024x1820",
            "1024x2048",
            "2048x1024",
            "1434x1024",
            "1024x1434",
            "1024x1280",
            "1280x1024",
            "1024x1707",
            "1707x1024"
          ],
          {
            "default": "1024x1024",
            "tooltip": "The size of the generated image."
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 6,
            "tooltip": "The number of images to generate."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ],
        "recraft_controls": [
          "RECRAFT_CONTROLS",
          {
            "tooltip": "Optional additional controls over the generation via the Recraft Controls node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "substyle", "size", "n", "seed"],
      "optional": ["negative_prompt", "recraft_controls"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["SVG"],
    "output_is_list": [false],
    "output_name": ["SVG"],
    "name": "RecraftTextToVectorNode",
    "display_name": "Recraft Text to Vector",
    "description": "Generates SVG synchronously based on prompt and resolution.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftVectorizeImageNode": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image"],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["SVG"],
    "output_is_list": [false],
    "output_name": ["SVG"],
    "name": "RecraftVectorizeImageNode",
    "display_name": "Recraft Vectorize Image",
    "description": "Generates SVG synchronously from an input image.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftRemoveBackgroundNode": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image"],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE", "MASK"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "MASK"],
    "name": "RecraftRemoveBackgroundNode",
    "display_name": "Recraft Remove Background",
    "description": "Remove background from image, and return processed image and mask.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftReplaceBackgroundNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the image generation."
          }
        ],
        "n": [
          "INT",
          {
            "default": 1,
            "min": 1,
            "max": 6,
            "tooltip": "The number of images to generate."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "Seed to determine if node should re-run; actual results are nondeterministic regardless of seed."
          }
        ]
      },
      "optional": {
        "recraft_style": ["RECRAFT_V3_STYLE"],
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image", "prompt", "n", "seed"],
      "optional": ["recraft_style", "negative_prompt"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RecraftReplaceBackgroundNode",
    "display_name": "Recraft Replace Background",
    "description": "Replace background on image, based on provided prompt.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftCrispUpscaleNode": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image"],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RecraftCrispUpscaleNode",
    "display_name": "Recraft Crisp Upscale Image",
    "description": "Upscale image synchronously.\nEnhances a given raster image using ‘crisp upscale’ tool, increasing image resolution, making the image sharper and cleaner.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftCreativeUpscaleNode": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image"],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RecraftCreativeUpscaleNode",
    "display_name": "Recraft Creative Upscale Image",
    "description": "Upscale image synchronously.\nEnhances a given raster image using ‘creative upscale’ tool, boosting resolution with a focus on refining small details and faces.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false,
    "api_node": true
  },
  "RecraftStyleV3RealisticImage": {
    "input": {
      "required": {
        "substyle": [
          [
            "None",
            "b_and_w",
            "enterprise",
            "evening_light",
            "faded_nostalgia",
            "forest_life",
            "hard_flash",
            "hdr",
            "motion_blur",
            "mystic_naturalism",
            "natural_light",
            "natural_tones",
            "organic_calm",
            "real_life_glow",
            "retro_realism",
            "retro_snapshot",
            "studio_portrait",
            "urban_drama",
            "village_realism",
            "warm_folk"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["substyle"]
    },
    "output": ["RECRAFT_V3_STYLE"],
    "output_is_list": [false],
    "output_name": ["recraft_style"],
    "name": "RecraftStyleV3RealisticImage",
    "display_name": "Recraft Style - Realistic Image",
    "description": "Select realistic_image style and optional substyle.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false
  },
  "RecraftStyleV3DigitalIllustration": {
    "input": {
      "required": {
        "substyle": [
          [
            "None",
            "2d_art_poster",
            "2d_art_poster_2",
            "antiquarian",
            "bold_fantasy",
            "child_book",
            "child_books",
            "cover",
            "crosshatch",
            "digital_engraving",
            "engraving_color",
            "expressionism",
            "freehand_details",
            "grain",
            "grain_20",
            "graphic_intensity",
            "hand_drawn",
            "hand_drawn_outline",
            "handmade_3d",
            "hard_comics",
            "infantile_sketch",
            "long_shadow",
            "modern_folk",
            "multicolor",
            "neon_calm",
            "noir",
            "nostalgic_pastel",
            "outline_details",
            "pastel_gradient",
            "pastel_sketch",
            "pixel_art",
            "plastic",
            "pop_art",
            "pop_renaissance",
            "seamless",
            "street_art",
            "tablet_sketch",
            "urban_glow",
            "urban_sketching",
            "vanilla_dreams",
            "young_adult_book",
            "young_adult_book_2"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["substyle"]
    },
    "output": ["RECRAFT_V3_STYLE"],
    "output_is_list": [false],
    "output_name": ["recraft_style"],
    "name": "RecraftStyleV3DigitalIllustration",
    "display_name": "Recraft Style - Digital Illustration",
    "description": "Select realistic_image style and optional substyle.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false
  },
  "RecraftStyleV3LogoRaster": {
    "input": {
      "required": {
        "substyle": [
          [
            "emblem_graffiti",
            "emblem_pop_art",
            "emblem_punk",
            "emblem_stamp",
            "emblem_vintage"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["substyle"]
    },
    "output": ["RECRAFT_V3_STYLE"],
    "output_is_list": [false],
    "output_name": ["recraft_style"],
    "name": "RecraftStyleV3LogoRaster",
    "display_name": "Recraft Style - Logo Raster",
    "description": "Select realistic_image style and optional substyle.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false
  },
  "RecraftStyleV3InfiniteStyleLibrary": {
    "input": {
      "required": {
        "style_id": [
          "STRING",
          {
            "default": "",
            "tooltip": "UUID of style from Infinite Style Library."
          }
        ]
      }
    },
    "input_order": {
      "required": ["style_id"]
    },
    "output": ["RECRAFT_V3_STYLE"],
    "output_is_list": [false],
    "output_name": ["recraft_style"],
    "name": "RecraftStyleV3InfiniteStyleLibrary",
    "display_name": "Recraft Style - Infinite Style Library",
    "description": "Select style based on preexisting UUID from Recraft's Infinite Style Library.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false
  },
  "RecraftColorRGB": {
    "input": {
      "required": {
        "r": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 255,
            "tooltip": "Red value of color."
          }
        ],
        "g": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 255,
            "tooltip": "Green value of color."
          }
        ],
        "b": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 255,
            "tooltip": "Blue value of color."
          }
        ]
      },
      "optional": {
        "recraft_color": ["RECRAFT_COLOR"]
      }
    },
    "input_order": {
      "required": ["r", "g", "b"],
      "optional": ["recraft_color"]
    },
    "output": ["RECRAFT_COLOR"],
    "output_is_list": [false],
    "output_name": ["recraft_color"],
    "name": "RecraftColorRGB",
    "display_name": "Recraft Color RGB",
    "description": "Create Recraft Color by choosing specific RGB values.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false
  },
  "RecraftControls": {
    "input": {
      "required": {},
      "optional": {
        "colors": ["RECRAFT_COLOR"],
        "background_color": ["RECRAFT_COLOR"]
      }
    },
    "input_order": {
      "required": [],
      "optional": ["colors", "background_color"]
    },
    "output": ["RECRAFT_CONTROLS"],
    "output_is_list": [false],
    "output_name": ["recraft_controls"],
    "name": "RecraftControls",
    "display_name": "Recraft Controls",
    "description": "Create Recraft Controls for customizing Recraft generation.",
    "python_module": "comfy_api_nodes.nodes_recraft",
    "category": "api node/image/Recraft",
    "output_node": false
  },
  "PixverseTextToVideoNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the video generation"
          }
        ],
        "aspect_ratio": [["16:9", "4:3", "1:1", "3:4", "9:16"]],
        "quality": [
          ["360p", "540p", "720p", "1080p"],
          {
            "default": "540p"
          }
        ],
        "duration_seconds": [[5, 8]],
        "motion_mode": [["normal", "fast"]],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "control_after_generate": true,
            "tooltip": "Seed for video generation."
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ],
        "pixverse_template": [
          "PIXVERSE_TEMPLATE",
          {
            "tooltip": "An optional template to influence style of generation, created by the PixVerse Template node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "aspect_ratio",
        "quality",
        "duration_seconds",
        "motion_mode",
        "seed"
      ],
      "optional": ["negative_prompt", "pixverse_template"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "PixverseTextToVideoNode",
    "display_name": "PixVerse Text to Video",
    "description": "Generates videos based on prompt and output_size.",
    "python_module": "comfy_api_nodes.nodes_pixverse",
    "category": "api node/video/PixVerse",
    "output_node": false,
    "api_node": true
  },
  "PixverseImageToVideoNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the video generation"
          }
        ],
        "quality": [
          ["360p", "540p", "720p", "1080p"],
          {
            "default": "540p"
          }
        ],
        "duration_seconds": [[5, 8]],
        "motion_mode": [["normal", "fast"]],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "control_after_generate": true,
            "tooltip": "Seed for video generation."
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ],
        "pixverse_template": [
          "PIXVERSE_TEMPLATE",
          {
            "tooltip": "An optional template to influence style of generation, created by the PixVerse Template node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "image",
        "prompt",
        "quality",
        "duration_seconds",
        "motion_mode",
        "seed"
      ],
      "optional": ["negative_prompt", "pixverse_template"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "PixverseImageToVideoNode",
    "display_name": "PixVerse Image to Video",
    "description": "Generates videos based on prompt and output_size.",
    "python_module": "comfy_api_nodes.nodes_pixverse",
    "category": "api node/video/PixVerse",
    "output_node": false,
    "api_node": true
  },
  "PixverseTransitionVideoNode": {
    "input": {
      "required": {
        "first_frame": ["IMAGE"],
        "last_frame": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Prompt for the video generation"
          }
        ],
        "quality": [
          ["360p", "540p", "720p", "1080p"],
          {
            "default": "540p"
          }
        ],
        "duration_seconds": [[5, 8]],
        "motion_mode": [["normal", "fast"]],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 2147483647,
            "control_after_generate": true,
            "tooltip": "Seed for video generation."
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "An optional text description of undesired elements on an image."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "first_frame",
        "last_frame",
        "prompt",
        "quality",
        "duration_seconds",
        "motion_mode",
        "seed"
      ],
      "optional": ["negative_prompt"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "PixverseTransitionVideoNode",
    "display_name": "PixVerse Transition Video",
    "description": "Generates videos based on prompt and output_size.",
    "python_module": "comfy_api_nodes.nodes_pixverse",
    "category": "api node/video/PixVerse",
    "output_node": false,
    "api_node": true
  },
  "PixverseTemplateNode": {
    "input": {
      "required": {
        "template": [
          [
            "Microwave",
            "Suit Swagger",
            "Anything, Robot",
            "Subject 3 Fever",
            "kiss kiss"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["template"]
    },
    "output": ["PIXVERSE_TEMPLATE"],
    "output_is_list": [false],
    "output_name": ["pixverse_template"],
    "name": "PixverseTemplateNode",
    "display_name": "PixVerse Template",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_pixverse",
    "category": "api node/video/PixVerse",
    "output_node": false
  },
  "StabilityStableImageUltraNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "What you wish to see in the output image. A strong, descriptive prompt that clearly definesWhat you wish to see in the output image. A strong, descriptive prompt that clearly defineselements, colors, and subjects will lead to better results. To control the weight of a given word use the format `(word:weight)`,where `word` is the word you'd like to control the weight of and `weight`is a value between 0 and 1. For example: `The sky was a crisp (blue:0.3) and (green:0.8)`would convey a sky that was blue and green, but more green than blue."
          }
        ],
        "aspect_ratio": [
          ["1:1", "16:9", "9:16", "3:2", "2:3", "5:4", "4:5", "21:9", "9:21"],
          {
            "default": "1:1",
            "tooltip": "Aspect ratio of generated image."
          }
        ],
        "style_preset": [
          [
            "None",
            "3d-model",
            "analog-film",
            "anime",
            "cinematic",
            "comic-book",
            "digital-art",
            "enhance",
            "fantasy-art",
            "isometric",
            "line-art",
            "low-poly",
            "modeling-compound",
            "neon-punk",
            "origami",
            "photographic",
            "pixel-art",
            "tile-texture"
          ],
          {
            "tooltip": "Optional desired style of generated image."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 4294967294,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {
        "image": ["IMAGE"],
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "A blurb of text describing what you do not wish to see in the output image. This is an advanced feature."
          }
        ],
        "image_denoise": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "tooltip": "Denoise of input image; 0.0 yields image identical to input, 1.0 is as if no image was provided at all."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["prompt", "aspect_ratio", "style_preset", "seed"],
      "optional": ["image", "negative_prompt", "image_denoise"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "StabilityStableImageUltraNode",
    "display_name": "Stability AI Stable Image Ultra",
    "description": "Generates images synchronously based on prompt and resolution.",
    "python_module": "comfy_api_nodes.nodes_stability",
    "category": "api node/image/Stability AI",
    "output_node": false,
    "api_node": true
  },
  "StabilityStableImageSD_3_5Node": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "What you wish to see in the output image. A strong, descriptive prompt that clearly defines elements, colors, and subjects will lead to better results."
          }
        ],
        "model": [["sd3.5-large", "sd3.5-medium"]],
        "aspect_ratio": [
          ["1:1", "16:9", "9:16", "3:2", "2:3", "5:4", "4:5", "21:9", "9:21"],
          {
            "default": "1:1",
            "tooltip": "Aspect ratio of generated image."
          }
        ],
        "style_preset": [
          [
            "None",
            "3d-model",
            "analog-film",
            "anime",
            "cinematic",
            "comic-book",
            "digital-art",
            "enhance",
            "fantasy-art",
            "isometric",
            "line-art",
            "low-poly",
            "modeling-compound",
            "neon-punk",
            "origami",
            "photographic",
            "pixel-art",
            "tile-texture"
          ],
          {
            "tooltip": "Optional desired style of generated image."
          }
        ],
        "cfg_scale": [
          "FLOAT",
          {
            "default": 4,
            "min": 1,
            "max": 10,
            "step": 0.1,
            "tooltip": "How strictly the diffusion process adheres to the prompt text (higher values keep your image closer to your prompt)"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 4294967294,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {
        "image": ["IMAGE"],
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "Keywords of what you do not wish to see in the output image. This is an advanced feature."
          }
        ],
        "image_denoise": [
          "FLOAT",
          {
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.01,
            "tooltip": "Denoise of input image; 0.0 yields image identical to input, 1.0 is as if no image was provided at all."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "model",
        "aspect_ratio",
        "style_preset",
        "cfg_scale",
        "seed"
      ],
      "optional": ["image", "negative_prompt", "image_denoise"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "StabilityStableImageSD_3_5Node",
    "display_name": "Stability AI Stable Diffusion 3.5 Image",
    "description": "Generates images synchronously based on prompt and resolution.",
    "python_module": "comfy_api_nodes.nodes_stability",
    "category": "api node/image/Stability AI",
    "output_node": false,
    "api_node": true
  },
  "StabilityUpscaleConservativeNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "What you wish to see in the output image. A strong, descriptive prompt that clearly defines elements, colors, and subjects will lead to better results."
          }
        ],
        "creativity": [
          "FLOAT",
          {
            "default": 0.35,
            "min": 0.2,
            "max": 0.5,
            "step": 0.01,
            "tooltip": "Controls the likelihood of creating additional details not heavily conditioned by the init image."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 4294967294,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "Keywords of what you do not wish to see in the output image. This is an advanced feature."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image", "prompt", "creativity", "seed"],
      "optional": ["negative_prompt"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "StabilityUpscaleConservativeNode",
    "display_name": "Stability AI Upscale Conservative",
    "description": "Upscale image with minimal alterations to 4K resolution.",
    "python_module": "comfy_api_nodes.nodes_stability",
    "category": "api node/image/Stability AI",
    "output_node": false,
    "api_node": true
  },
  "StabilityUpscaleCreativeNode": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "What you wish to see in the output image. A strong, descriptive prompt that clearly defines elements, colors, and subjects will lead to better results."
          }
        ],
        "creativity": [
          "FLOAT",
          {
            "default": 0.3,
            "min": 0.1,
            "max": 0.5,
            "step": 0.01,
            "tooltip": "Controls the likelihood of creating additional details not heavily conditioned by the init image."
          }
        ],
        "style_preset": [
          [
            "None",
            "3d-model",
            "analog-film",
            "anime",
            "cinematic",
            "comic-book",
            "digital-art",
            "enhance",
            "fantasy-art",
            "isometric",
            "line-art",
            "low-poly",
            "modeling-compound",
            "neon-punk",
            "origami",
            "photographic",
            "pixel-art",
            "tile-texture"
          ],
          {
            "tooltip": "Optional desired style of generated image."
          }
        ],
        "seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 4294967294,
            "control_after_generate": true,
            "tooltip": "The random seed used for creating the noise."
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "default": "",
            "forceInput": true,
            "tooltip": "Keywords of what you do not wish to see in the output image. This is an advanced feature."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image", "prompt", "creativity", "style_preset", "seed"],
      "optional": ["negative_prompt"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "StabilityUpscaleCreativeNode",
    "display_name": "Stability AI Upscale Creative",
    "description": "Upscale image with minimal alterations to 4K resolution.",
    "python_module": "comfy_api_nodes.nodes_stability",
    "category": "api node/image/Stability AI",
    "output_node": false,
    "api_node": true
  },
  "StabilityUpscaleFastNode": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "optional": {},
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["image"],
      "optional": [],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "StabilityUpscaleFastNode",
    "display_name": "Stability AI Upscale Fast",
    "description": "Quickly upscales an image via Stability API call to 4x its original size; intended for upscaling low-quality/compressed images.",
    "python_module": "comfy_api_nodes.nodes_stability",
    "category": "api node/image/Stability AI",
    "output_node": false,
    "api_node": true
  },
  "PikaImageToVideoNode2_2": {
    "input": {
      "required": {
        "image": [
          "IMAGE",
          {
            "tooltip": "The image to convert to video"
          }
        ],
        "prompt_text": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": null,
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": ["1080p", "720p"],
            "default": "1080p"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": [5, 10],
            "default": 5
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "image",
        "prompt_text",
        "negative_prompt",
        "seed",
        "resolution",
        "duration"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "PikaImageToVideoNode2_2",
    "display_name": "Pika Image to Video",
    "description": "Sends an image and prompt to the Pika API v2.2 to generate a video.",
    "python_module": "comfy_api_nodes.nodes_pika",
    "category": "api node/video/Pika",
    "output_node": false,
    "api_node": true
  },
  "PikaTextToVideoNode2_2": {
    "input": {
      "required": {
        "prompt_text": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": null,
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": ["1080p", "720p"],
            "default": "1080p"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": [5, 10],
            "default": 5
          }
        ],
        "aspect_ratio": [
          "FLOAT",
          {
            "default": 1.7777777777777777,
            "tooltip": "Aspect ratio (width / height)",
            "min": 0.4,
            "max": 2.5,
            "step": 0.001
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt_text",
        "negative_prompt",
        "seed",
        "resolution",
        "duration",
        "aspect_ratio"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "PikaTextToVideoNode2_2",
    "display_name": "Pika Text to Video",
    "description": "Sends a text prompt to the Pika API v2.2 to generate a video.",
    "python_module": "comfy_api_nodes.nodes_pika",
    "category": "api node/video/Pika",
    "output_node": false,
    "api_node": true
  },
  "PikaScenesV2_2": {
    "input": {
      "required": {
        "prompt_text": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": null,
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": ["1080p", "720p"],
            "default": "1080p"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": [5, 10],
            "default": 5
          }
        ],
        "ingredients_mode": [
          "COMBO",
          {
            "options": ["creative", "precise"],
            "default": "creative"
          }
        ],
        "aspect_ratio": [
          "FLOAT",
          {
            "default": 1.7777777777777777,
            "tooltip": "Aspect ratio (width / height)",
            "step": 0.001,
            "min": 0.4,
            "max": 2.5
          }
        ]
      },
      "optional": {
        "image_ingredient_1": [
          "IMAGE",
          {
            "tooltip": "Image that will be used as ingredient to create a video."
          }
        ],
        "image_ingredient_2": [
          "IMAGE",
          {
            "tooltip": "Image that will be used as ingredient to create a video."
          }
        ],
        "image_ingredient_3": [
          "IMAGE",
          {
            "tooltip": "Image that will be used as ingredient to create a video."
          }
        ],
        "image_ingredient_4": [
          "IMAGE",
          {
            "tooltip": "Image that will be used as ingredient to create a video."
          }
        ],
        "image_ingredient_5": [
          "IMAGE",
          {
            "tooltip": "Image that will be used as ingredient to create a video."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "prompt_text",
        "negative_prompt",
        "seed",
        "resolution",
        "duration",
        "ingredients_mode",
        "aspect_ratio"
      ],
      "optional": [
        "image_ingredient_1",
        "image_ingredient_2",
        "image_ingredient_3",
        "image_ingredient_4",
        "image_ingredient_5"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "PikaScenesV2_2",
    "display_name": "Pika Scenes (Video Image Composition)",
    "description": "Combine your images to create a video with the objects in them. Upload multiple images as ingredients and generate a high-quality video that incorporates all of them.",
    "python_module": "comfy_api_nodes.nodes_pika",
    "category": "api node/video/Pika",
    "output_node": false,
    "api_node": true
  },
  "Pikadditions": {
    "input": {
      "required": {
        "video": [
          "VIDEO",
          {
            "tooltip": "The video to add an image to."
          }
        ],
        "image": [
          "IMAGE",
          {
            "tooltip": "The image to add to the video."
          }
        ],
        "prompt_text": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": null,
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["video", "image", "prompt_text", "negative_prompt", "seed"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "Pikadditions",
    "display_name": "Pikadditions (Video Object Insertion)",
    "description": "Add any object or image into your video. Upload a video and specify what you'd like to add to create a seamlessly integrated result.",
    "python_module": "comfy_api_nodes.nodes_pika",
    "category": "api node/video/Pika",
    "output_node": false,
    "api_node": true
  },
  "Pikaswaps": {
    "input": {
      "required": {
        "video": [
          "VIDEO",
          {
            "tooltip": "The video to swap an object in."
          }
        ],
        "image": [
          "IMAGE",
          {
            "tooltip": "The image used to replace the masked object in the video."
          }
        ],
        "mask": [
          "MASK",
          {
            "tooltip": "Use the mask to define areas in the video to replace"
          }
        ],
        "prompt_text": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": null,
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "video",
        "image",
        "mask",
        "prompt_text",
        "negative_prompt",
        "seed"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "Pikaswaps",
    "display_name": "Pika Swaps (Video Object Replacement)",
    "description": "Swap out any object or region of your video with a new image or object. Define areas to replace either with a mask or coordinates.",
    "python_module": "comfy_api_nodes.nodes_pika",
    "category": "api node/video/Pika",
    "output_node": false,
    "api_node": true
  },
  "Pikaffects": {
    "input": {
      "required": {
        "image": [
          "IMAGE",
          {
            "tooltip": "The reference image to apply the Pikaffect to."
          }
        ],
        "pikaffect": [
          "COMBO",
          {
            "options": [
              "Cake-ify",
              "Crumble",
              "Crush",
              "Decapitate",
              "Deflate",
              "Dissolve",
              "Explode",
              "Eye-pop",
              "Inflate",
              "Levitate",
              "Melt",
              "Peel",
              "Poke",
              "Squish",
              "Ta-da",
              "Tear"
            ],
            "default": "Cake-ify"
          }
        ],
        "prompt_text": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": null,
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "image",
        "pikaffect",
        "prompt_text",
        "negative_prompt",
        "seed"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "Pikaffects",
    "display_name": "Pikaffects (Video Effects)",
    "description": "Generate a video with a specific Pikaffect. Supported Pikaffects: Cake-ify, Crumble, Crush, Decapitate, Deflate, Dissolve, Explode, Eye-pop, Inflate, Levitate, Melt, Peel, Poke, Squish, Ta-da, Tear",
    "python_module": "comfy_api_nodes.nodes_pika",
    "category": "api node/video/Pika",
    "output_node": false,
    "api_node": true
  },
  "PikaStartEndFrameNode2_2": {
    "input": {
      "required": {
        "image_start": [
          "IMAGE",
          {
            "tooltip": "The first image to combine."
          }
        ],
        "image_end": [
          "IMAGE",
          {
            "tooltip": "The last image to combine."
          }
        ],
        "prompt_text": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": null,
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": ["1080p", "720p"],
            "default": "1080p"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": [5, 10],
            "default": null
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": [
        "image_start",
        "image_end",
        "prompt_text",
        "negative_prompt",
        "seed",
        "resolution",
        "duration"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "PikaStartEndFrameNode2_2",
    "display_name": "Pika Start and End Frame to Video",
    "description": "Generate a video by combining your first and last frame. Upload two images to define the start and end points, and let the AI create a smooth transition between them.",
    "python_module": "comfy_api_nodes.nodes_pika",
    "category": "api node/video/Pika",
    "output_node": false,
    "api_node": true
  },
  "RunwayFirstLastFrameNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Text prompt for the generation",
            "multiline": true
          }
        ],
        "start_frame": [
          "IMAGE",
          {
            "tooltip": "Start frame to be used for the video"
          }
        ],
        "end_frame": [
          "IMAGE",
          {
            "tooltip": "End frame to be used for the video. Supported for gen3a_turbo only."
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": [5, 10]
          }
        ],
        "ratio": [
          "COMBO",
          {
            "options": ["768:1280", "1280:768"]
          }
        ],
        "seed": [
          "INT",
          {
            "tooltip": "Random seed for generation",
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "unique_id": "UNIQUE_ID",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "start_frame",
        "end_frame",
        "duration",
        "ratio",
        "seed"
      ],
      "hidden": ["auth_token", "unique_id", "comfy_api_key"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "RunwayFirstLastFrameNode",
    "display_name": "Runway First-Last-Frame to Video",
    "description": "Upload first and last keyframes, draft a prompt, and generate a video. More complex transitions, such as cases where the Last frame is completely different from the First frame, may benefit from the longer 10s duration. This would give the generation more time to smoothly transition between the two inputs. Before diving in, review these best practices to ensure that your input selections will set your generation up for success: https://help.runwayml.com/hc/en-us/articles/34170748696595-Creating-with-Keyframes-on-Gen-3.",
    "python_module": "comfy_api_nodes.nodes_runway",
    "category": "api node/video/Runway",
    "output_node": false,
    "api_node": true
  },
  "RunwayImageToVideoNodeGen3a": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Text prompt for the generation",
            "multiline": true
          }
        ],
        "start_frame": [
          "IMAGE",
          {
            "tooltip": "Start frame to be used for the video"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": [5, 10]
          }
        ],
        "ratio": [
          "COMBO",
          {
            "options": ["768:1280", "1280:768"]
          }
        ],
        "seed": [
          "INT",
          {
            "tooltip": "Random seed for generation",
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "start_frame", "duration", "ratio", "seed"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "RunwayImageToVideoNodeGen3a",
    "display_name": "Runway Image to Video (Gen3a Turbo)",
    "description": "Generate a video from a single starting frame using Gen3a Turbo model. Before diving in, review these best practices to ensure that your input selections will set your generation up for success: https://help.runwayml.com/hc/en-us/articles/33927968552339-Creating-with-Act-One-on-Gen-3-Alpha-and-Turbo.",
    "python_module": "comfy_api_nodes.nodes_runway",
    "category": "api node/video/Runway",
    "output_node": false,
    "api_node": true
  },
  "RunwayImageToVideoNodeGen4": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "tooltip": "Text prompt for the generation",
            "multiline": true
          }
        ],
        "start_frame": [
          "IMAGE",
          {
            "tooltip": "Start frame to be used for the video"
          }
        ],
        "duration": [
          "COMBO",
          {
            "options": [5, 10]
          }
        ],
        "ratio": [
          "COMBO",
          {
            "options": [
              "1280:720",
              "720:1280",
              "1104:832",
              "832:1104",
              "960:960",
              "1584:672"
            ]
          }
        ],
        "seed": [
          "INT",
          {
            "tooltip": "Random seed for generation",
            "min": 0,
            "max": 4294967295,
            "control_after_generate": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "start_frame", "duration", "ratio", "seed"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["VIDEO"],
    "name": "RunwayImageToVideoNodeGen4",
    "display_name": "Runway Image to Video (Gen4 Turbo)",
    "description": "Generate a video from a single starting frame using Gen4 Turbo model. Before diving in, review these best practices to ensure that your input selections will set your generation up for success: https://help.runwayml.com/hc/en-us/articles/37327109429011-Creating-with-Gen-4-Video.",
    "python_module": "comfy_api_nodes.nodes_runway",
    "category": "api node/video/Runway",
    "output_node": false,
    "api_node": true
  },
  "RunwayTextToImageNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "tooltip": "Text prompt for the image generation",
            "multiline": true
          }
        ],
        "ratio": [
          "COMBO",
          {
            "options": [
              "1920:1080",
              "1080:1920",
              "1024:1024",
              "1360:768",
              "1080:1080",
              "1168:880",
              "1440:1080",
              "1080:1440",
              "1808:768",
              "2112:912"
            ]
          }
        ]
      },
      "optional": {
        "reference_image": [
          "IMAGE",
          {
            "tooltip": "Optional reference image to guide the generation"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "ratio"],
      "optional": ["reference_image"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "RunwayTextToImageNode",
    "display_name": "Runway Text to Image",
    "description": "Generate an image from a text prompt using Runway's Gen 4 model. You can also include reference images to guide the generation.",
    "python_module": "comfy_api_nodes.nodes_runway",
    "category": "api node/image/Runway",
    "output_node": false,
    "api_node": true
  },
  "TripoTextToModelNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true
          }
        ]
      },
      "optional": {
        "negative_prompt": [
          "STRING",
          {
            "multiline": true
          }
        ],
        "model_version": [
          "COMBO",
          {
            "options": ["v2.5-20250123", "v2.0-20240919", "v1.4-20240625"],
            "default": "v2.5-20250123"
          }
        ],
        "style": [
          "COMBO",
          {
            "options": [
              "person:person2cartoon",
              "animal:venom",
              "object:clay",
              "object:steampunk",
              "object:christmas",
              "object:barbie",
              "gold",
              "ancient_bronze",
              "None"
            ],
            "default": "None"
          }
        ],
        "texture": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "pbr": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "image_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "model_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "texture_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "texture_quality": [
          ["standard", "detailed"],
          {
            "default": "standard"
          }
        ],
        "face_limit": [
          "INT",
          {
            "min": -1,
            "max": 500000,
            "default": -1
          }
        ],
        "quad": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt"],
      "optional": [
        "negative_prompt",
        "model_version",
        "style",
        "texture",
        "pbr",
        "image_seed",
        "model_seed",
        "texture_seed",
        "texture_quality",
        "face_limit",
        "quad"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING", "MODEL_TASK_ID"],
    "output_is_list": [false, false],
    "output_name": ["model_file", "model task_id"],
    "name": "TripoTextToModelNode",
    "display_name": "Tripo: Text to Model",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "TripoImageToModelNode": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "optional": {
        "model_version": [
          "COMBO",
          {
            "options": ["v2.5-20250123", "v2.0-20240919", "v1.4-20240625"],
            "default": null,
            "tooltip": "The model version to use for generation"
          }
        ],
        "style": [
          "COMBO",
          {
            "options": [
              "person:person2cartoon",
              "animal:venom",
              "object:clay",
              "object:steampunk",
              "object:christmas",
              "object:barbie",
              "gold",
              "ancient_bronze",
              "None"
            ],
            "default": "None"
          }
        ],
        "texture": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "pbr": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "model_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "orientation": [
          "COMBO",
          {
            "options": ["align_image", "default"],
            "default": "default"
          }
        ],
        "texture_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "texture_quality": [
          ["standard", "detailed"],
          {
            "default": "standard"
          }
        ],
        "texture_alignment": [
          ["original_image", "geometry"],
          {
            "default": "original_image"
          }
        ],
        "face_limit": [
          "INT",
          {
            "min": -1,
            "max": 500000,
            "default": -1
          }
        ],
        "quad": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["image"],
      "optional": [
        "model_version",
        "style",
        "texture",
        "pbr",
        "model_seed",
        "orientation",
        "texture_seed",
        "texture_quality",
        "texture_alignment",
        "face_limit",
        "quad"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING", "MODEL_TASK_ID"],
    "output_is_list": [false, false],
    "output_name": ["model_file", "model task_id"],
    "name": "TripoImageToModelNode",
    "display_name": "Tripo: Image to Model",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "TripoMultiviewToModelNode": {
    "input": {
      "required": {
        "image": ["IMAGE"]
      },
      "optional": {
        "image_left": ["IMAGE"],
        "image_back": ["IMAGE"],
        "image_right": ["IMAGE"],
        "model_version": [
          "COMBO",
          {
            "options": ["v2.5-20250123", "v2.0-20240919", "v1.4-20240625"],
            "default": null,
            "tooltip": "The model version to use for generation"
          }
        ],
        "orientation": [
          "COMBO",
          {
            "options": ["align_image", "default"],
            "default": "default"
          }
        ],
        "texture": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "pbr": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "model_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "texture_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "texture_quality": [
          ["standard", "detailed"],
          {
            "default": "standard"
          }
        ],
        "texture_alignment": [
          ["original_image", "geometry"],
          {
            "default": "original_image"
          }
        ],
        "face_limit": [
          "INT",
          {
            "min": -1,
            "max": 500000,
            "default": -1
          }
        ],
        "quad": [
          "BOOLEAN",
          {
            "default": false
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["image"],
      "optional": [
        "image_left",
        "image_back",
        "image_right",
        "model_version",
        "orientation",
        "texture",
        "pbr",
        "model_seed",
        "texture_seed",
        "texture_quality",
        "texture_alignment",
        "face_limit",
        "quad"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING", "MODEL_TASK_ID"],
    "output_is_list": [false, false],
    "output_name": ["model_file", "model task_id"],
    "name": "TripoMultiviewToModelNode",
    "display_name": "Tripo: Multiview to Model",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "TripoTextureNode": {
    "input": {
      "required": {
        "model_task_id": ["MODEL_TASK_ID"]
      },
      "optional": {
        "texture": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "pbr": [
          "BOOLEAN",
          {
            "default": true
          }
        ],
        "texture_seed": [
          "INT",
          {
            "default": 42
          }
        ],
        "texture_quality": [
          ["standard", "detailed"],
          {
            "default": "standard"
          }
        ],
        "texture_alignment": [
          ["original_image", "geometry"],
          {
            "default": "original_image"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["model_task_id"],
      "optional": [
        "texture",
        "pbr",
        "texture_seed",
        "texture_quality",
        "texture_alignment"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING", "MODEL_TASK_ID"],
    "output_is_list": [false, false],
    "output_name": ["model_file", "model task_id"],
    "name": "TripoTextureNode",
    "display_name": "Tripo: Texture model",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "TripoRefineNode": {
    "input": {
      "required": {
        "model_task_id": [
          "MODEL_TASK_ID",
          {
            "tooltip": "Must be a v1.4 Tripo model"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["model_task_id"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING", "MODEL_TASK_ID"],
    "output_is_list": [false, false],
    "output_name": ["model_file", "model task_id"],
    "name": "TripoRefineNode",
    "display_name": "Tripo: Refine Draft model",
    "description": "Refine a draft model created by v1.4 Tripo models only.",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "TripoRigNode": {
    "input": {
      "required": {
        "original_model_task_id": ["MODEL_TASK_ID"]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["original_model_task_id"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING", "RIG_TASK_ID"],
    "output_is_list": [false, false],
    "output_name": ["model_file", "rig task_id"],
    "name": "TripoRigNode",
    "display_name": "Tripo: Rig model",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "TripoRetargetNode": {
    "input": {
      "required": {
        "original_model_task_id": ["RIG_TASK_ID"],
        "animation": [
          [
            "preset:idle",
            "preset:walk",
            "preset:climb",
            "preset:jump",
            "preset:slash",
            "preset:shoot",
            "preset:hurt",
            "preset:fall",
            "preset:turn"
          ]
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["original_model_task_id", "animation"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING", "RETARGET_TASK_ID"],
    "output_is_list": [false, false],
    "output_name": ["model_file", "retarget task_id"],
    "name": "TripoRetargetNode",
    "display_name": "Tripo: Retarget rigged model",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "TripoConversionNode": {
    "input": {
      "required": {
        "original_model_task_id": [
          "MODEL_TASK_ID,RIG_TASK_ID,RETARGET_TASK_ID"
        ],
        "format": [["GLTF", "USDZ", "FBX", "OBJ", "STL", "3MF"]]
      },
      "optional": {
        "quad": [
          "BOOLEAN",
          {
            "default": false
          }
        ],
        "face_limit": [
          "INT",
          {
            "min": -1,
            "max": 500000,
            "default": -1
          }
        ],
        "texture_size": [
          "INT",
          {
            "min": 128,
            "max": 4096,
            "default": 4096
          }
        ],
        "texture_format": [
          [
            "BMP",
            "DPX",
            "HDR",
            "JPEG",
            "OPEN_EXR",
            "PNG",
            "TARGA",
            "TIFF",
            "WEBP"
          ],
          {
            "default": "JPEG"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["original_model_task_id", "format"],
      "optional": ["quad", "face_limit", "texture_size", "texture_format"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "TripoConversionNode",
    "display_name": "Tripo: Convert model",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_tripo",
    "category": "api node/3d/Tripo",
    "output_node": true,
    "api_node": true
  },
  "MoonvalleyImg2VideoNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": "low-poly, flat shader, bad rigging, stiff animation, uncanny eyes, low-quality textures, looping glitch, cheap effect, overbloom, bloom spam, default lighting, game asset, stiff face, ugly specular, AI artifacts",
            "tooltip": "Negative prompt text",
            "multiline": true
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": [
              "16:9 (1920 x 1080)",
              "9:16 (1080 x 1920)",
              "1:1 (1152 x 1152)",
              "4:3 (1440 x 1080)",
              "3:4 (1080 x 1440)",
              "21:9 (2560 x 1080)"
            ],
            "default": "16:9 (1920 x 1080)",
            "tooltip": "Resolution of the output video"
          }
        ],
        "prompt_adherence": [
          "FLOAT",
          {
            "default": 7,
            "tooltip": "Guidance scale for generation control",
            "step": 1,
            "min": 1,
            "max": 20
          }
        ],
        "seed": [
          "INT",
          {
            "default": 2992846741,
            "tooltip": "Random seed value",
            "min": 0,
            "max": 4294967295,
            "step": 1,
            "display": "number",
            "control_after_generate": true
          }
        ],
        "steps": [
          "INT",
          {
            "default": 100,
            "tooltip": "Number of denoising steps",
            "min": 1,
            "max": 100
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      },
      "optional": {
        "image": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "The reference image used to generate the video"
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "prompt",
        "negative_prompt",
        "resolution",
        "prompt_adherence",
        "seed",
        "steps"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"],
      "optional": ["image"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["video"],
    "name": "MoonvalleyImg2VideoNode",
    "display_name": "Moonvalley Marey Image to Video",
    "description": "Moonvalley Marey Image to Video Node",
    "python_module": "comfy_api_nodes.nodes_moonvalley",
    "category": "api node/video/Moonvalley Marey",
    "output_node": false,
    "api_node": true
  },
  "MoonvalleyTxt2VideoNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "default": null,
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": "low-poly, flat shader, bad rigging, stiff animation, uncanny eyes, low-quality textures, looping glitch, cheap effect, overbloom, bloom spam, default lighting, game asset, stiff face, ugly specular, AI artifacts",
            "tooltip": "Negative prompt text",
            "multiline": true
          }
        ],
        "resolution": [
          "COMBO",
          {
            "options": [
              "16:9 (1920 x 1080)",
              "9:16 (1080 x 1920)",
              "1:1 (1152 x 1152)",
              "4:3 (1440 x 1080)",
              "3:4 (1080 x 1440)",
              "21:9 (2560 x 1080)"
            ],
            "default": "16:9 (1920 x 1080)",
            "tooltip": "Resolution of the output video"
          }
        ],
        "prompt_adherence": [
          "FLOAT",
          {
            "default": 7,
            "tooltip": "Guidance scale for generation control",
            "step": 1,
            "min": 1,
            "max": 20
          }
        ],
        "seed": [
          "INT",
          {
            "default": 3752368933,
            "tooltip": "Random seed value",
            "min": 0,
            "max": 4294967295,
            "step": 1,
            "display": "number",
            "control_after_generate": true
          }
        ],
        "steps": [
          "INT",
          {
            "default": 100,
            "tooltip": "Number of denoising steps",
            "min": 1,
            "max": 100
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      },
      "optional": {}
    },
    "input_order": {
      "required": [
        "prompt",
        "negative_prompt",
        "resolution",
        "prompt_adherence",
        "seed",
        "steps"
      ],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"],
      "optional": []
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["video"],
    "name": "MoonvalleyTxt2VideoNode",
    "display_name": "Moonvalley Marey Text to Video",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_moonvalley",
    "category": "api node/video/Moonvalley Marey",
    "output_node": false,
    "api_node": true
  },
  "MoonvalleyVideo2VideoNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "tooltip": "Describes the video to generate",
            "multiline": true
          }
        ],
        "negative_prompt": [
          "STRING",
          {
            "default": "low-poly, flat shader, bad rigging, stiff animation, uncanny eyes, low-quality textures, looping glitch, cheap effect, overbloom, bloom spam, default lighting, game asset, stiff face, ugly specular, AI artifacts",
            "tooltip": "Negative prompt text",
            "multiline": true
          }
        ],
        "seed": [
          "INT",
          {
            "default": 441387494,
            "tooltip": "Random seed value",
            "min": 0,
            "max": 4294967295,
            "step": 1,
            "display": "number",
            "control_after_generate": true
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      },
      "optional": {
        "video": [
          "VIDEO",
          {
            "default": "",
            "multiline": false,
            "tooltip": "The reference video used to generate the output video. Must be at least 5 seconds long. Videos longer than 5s will be automatically trimmed. Only MP4 format supported."
          }
        ],
        "control_type": [
          ["Motion Transfer", "Pose Transfer"],
          {
            "default": "Motion Transfer"
          }
        ],
        "motion_intensity": [
          "INT",
          {
            "default": 100,
            "step": 1,
            "min": 0,
            "max": 100,
            "tooltip": "Only used if control_type is 'Motion Transfer'"
          }
        ]
      }
    },
    "input_order": {
      "required": ["prompt", "negative_prompt", "seed"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"],
      "optional": ["video", "control_type", "motion_intensity"]
    },
    "output": ["VIDEO"],
    "output_is_list": [false],
    "output_name": ["video"],
    "name": "MoonvalleyVideo2VideoNode",
    "display_name": "Moonvalley Marey Video to Video",
    "description": "",
    "python_module": "comfy_api_nodes.nodes_moonvalley",
    "category": "api node/video/Moonvalley Marey",
    "output_node": false,
    "api_node": true
  },
  "Rodin3D_Regular": {
    "input": {
      "required": {
        "Images": [
          "IMAGE",
          {
            "forceInput": true
          }
        ]
      },
      "optional": {
        "Seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 65535,
            "display": "number"
          }
        ],
        "Material_Type": [
          "COMBO",
          {
            "options": ["PBR", "Shaded"],
            "default": "PBR"
          }
        ],
        "Polygon_count": [
          "COMBO",
          {
            "options": [
              "4K-Quad",
              "8K-Quad",
              "18K-Quad",
              "50K-Quad",
              "200K-Triangle"
            ],
            "default": "18K-Quad"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["Images"],
      "optional": ["Seed", "Material_Type", "Polygon_count"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["3D Model Path"],
    "name": "Rodin3D_Regular",
    "display_name": "Rodin 3D Generate - Regular Generate",
    "description": "Generate 3D Assets using Rodin API",
    "python_module": "comfy_api_nodes.nodes_rodin",
    "category": "api node/3d/Rodin",
    "output_node": false,
    "api_node": true
  },
  "Rodin3D_Detail": {
    "input": {
      "required": {
        "Images": [
          "IMAGE",
          {
            "forceInput": true
          }
        ]
      },
      "optional": {
        "Seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 65535,
            "display": "number"
          }
        ],
        "Material_Type": [
          "COMBO",
          {
            "options": ["PBR", "Shaded"],
            "default": "PBR"
          }
        ],
        "Polygon_count": [
          "COMBO",
          {
            "options": [
              "4K-Quad",
              "8K-Quad",
              "18K-Quad",
              "50K-Quad",
              "200K-Triangle"
            ],
            "default": "18K-Quad"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["Images"],
      "optional": ["Seed", "Material_Type", "Polygon_count"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["3D Model Path"],
    "name": "Rodin3D_Detail",
    "display_name": "Rodin 3D Generate - Detail Generate",
    "description": "Generate 3D Assets using Rodin API",
    "python_module": "comfy_api_nodes.nodes_rodin",
    "category": "api node/3d/Rodin",
    "output_node": false,
    "api_node": true
  },
  "Rodin3D_Smooth": {
    "input": {
      "required": {
        "Images": [
          "IMAGE",
          {
            "forceInput": true
          }
        ]
      },
      "optional": {
        "Seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 65535,
            "display": "number"
          }
        ],
        "Material_Type": [
          "COMBO",
          {
            "options": ["PBR", "Shaded"],
            "default": "PBR"
          }
        ],
        "Polygon_count": [
          "COMBO",
          {
            "options": [
              "4K-Quad",
              "8K-Quad",
              "18K-Quad",
              "50K-Quad",
              "200K-Triangle"
            ],
            "default": "18K-Quad"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["Images"],
      "optional": ["Seed", "Material_Type", "Polygon_count"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["3D Model Path"],
    "name": "Rodin3D_Smooth",
    "display_name": "Rodin 3D Generate - Smooth Generate",
    "description": "Generate 3D Assets using Rodin API",
    "python_module": "comfy_api_nodes.nodes_rodin",
    "category": "api node/3d/Rodin",
    "output_node": false,
    "api_node": true
  },
  "Rodin3D_Sketch": {
    "input": {
      "required": {
        "Images": [
          "IMAGE",
          {
            "forceInput": true
          }
        ]
      },
      "optional": {
        "Seed": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 65535,
            "display": "number"
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG"
      }
    },
    "input_order": {
      "required": ["Images"],
      "optional": ["Seed"],
      "hidden": ["auth_token", "comfy_api_key"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["3D Model Path"],
    "name": "Rodin3D_Sketch",
    "display_name": "Rodin 3D Generate - Sketch Generate",
    "description": "Generate 3D Assets using Rodin API",
    "python_module": "comfy_api_nodes.nodes_rodin",
    "category": "api node/3d/Rodin",
    "output_node": false,
    "api_node": true
  },
  "GeminiNode": {
    "input": {
      "required": {
        "prompt": [
          "STRING",
          {
            "multiline": true,
            "default": "",
            "tooltip": "Text inputs to the model, used to generate a response. You can include detailed instructions, questions, or context for the model."
          }
        ],
        "model": [
          "COMBO",
          {
            "tooltip": "The Gemini model to use for generating responses.",
            "options": [
              "gemini-2.5-pro-preview-05-06",
              "gemini-2.5-flash-preview-04-17"
            ],
            "default": "gemini-2.5-pro-preview-05-06"
          }
        ],
        "seed": [
          "INT",
          {
            "default": 42,
            "min": 0,
            "max": 18446744073709552000,
            "control_after_generate": true,
            "tooltip": "When seed is fixed to a specific value, the model makes a best effort to provide the same response for repeated requests. Deterministic output isn't guaranteed. Also, changing the model or parameter settings, such as the temperature, can cause variations in the response even when you use the same seed value. By default, a random seed value is used."
          }
        ]
      },
      "optional": {
        "images": [
          "IMAGE",
          {
            "default": null,
            "tooltip": "Optional image(s) to use as context for the model. To include multiple images, you can use the Batch Images node."
          }
        ],
        "audio": [
          "AUDIO",
          {
            "tooltip": "Optional audio to use as context for the model.",
            "default": null
          }
        ],
        "video": [
          "VIDEO",
          {
            "tooltip": "Optional video to use as context for the model.",
            "default": null
          }
        ],
        "files": [
          "GEMINI_INPUT_FILES",
          {
            "default": null,
            "tooltip": "Optional file(s) to use as context for the model. Accepts inputs from the Gemini Generate Content Input Files node."
          }
        ]
      },
      "hidden": {
        "auth_token": "AUTH_TOKEN_COMFY_ORG",
        "comfy_api_key": "API_KEY_COMFY_ORG",
        "unique_id": "UNIQUE_ID"
      }
    },
    "input_order": {
      "required": ["prompt", "model", "seed"],
      "optional": ["images", "audio", "video", "files"],
      "hidden": ["auth_token", "comfy_api_key", "unique_id"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "GeminiNode",
    "display_name": "Google Gemini",
    "description": "Generate text responses with Google's Gemini AI model. You can provide multiple types of inputs (text, images, audio, video) as context for generating more relevant and meaningful responses.",
    "python_module": "comfy_api_nodes.nodes_gemini",
    "category": "api node/text/Gemini",
    "output_node": false,
    "api_node": true
  },
  "GeminiInputFiles": {
    "input": {
      "required": {
        "file": [
          "COMBO",
          {
            "tooltip": "Input files to include as context for the model. Only accepts text (.txt) and PDF (.pdf) files for now.",
            "options": [],
            "default": null
          }
        ]
      },
      "optional": {
        "GEMINI_INPUT_FILES": [
          "GEMINI_INPUT_FILES",
          {
            "tooltip": "An optional additional file(s) to batch together with the file loaded from this node. Allows chaining of input files so that a single message can include multiple input files.",
            "default": null
          }
        ]
      }
    },
    "input_order": {
      "required": ["file"],
      "optional": ["GEMINI_INPUT_FILES"]
    },
    "output": ["GEMINI_INPUT_FILES"],
    "output_is_list": [false],
    "output_name": ["GEMINI_INPUT_FILES"],
    "name": "GeminiInputFiles",
    "display_name": "Gemini Input Files",
    "description": "Loads and prepares input files to include as inputs for Gemini LLM nodes. The files will be read by the Gemini model when generating a response. The contents of the text file count toward the token limit. 🛈 TIP: Can be chained together with other Gemini Input File nodes.",
    "python_module": "comfy_api_nodes.nodes_gemini",
    "category": "api node/text/Gemini",
    "output_node": false
  },
  "DevToolsErrorRaiseNode": {
    "input": {
      "required": {}
    },
    "input_order": {
      "required": []
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "DevToolsErrorRaiseNode",
    "display_name": "Raise Error",
    "description": "Raise an error for development purposes",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsErrorRaiseNodeWithMessage": {
    "input": {
      "required": {
        "message": [
          "STRING",
          {
            "multiline": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["message"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsErrorRaiseNodeWithMessage",
    "display_name": "Raise Error with Message",
    "description": "Raise an error with message for development purposes",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true
  },
  "DevToolsExperimentalNode": {
    "input": {
      "required": {}
    },
    "input_order": {
      "required": []
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsExperimentalNode",
    "display_name": "Experimental Node",
    "description": "A experimental node",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true,
    "experimental": true
  },
  "DevToolsDeprecatedNode": {
    "input": {
      "required": {}
    },
    "input_order": {
      "required": []
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsDeprecatedNode",
    "display_name": "Deprecated Node",
    "description": "A deprecated node",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true,
    "deprecated": true
  },
  "DevToolsLongComboDropdown": {
    "input": {
      "required": {
        "option": [
          [
            "Option 0",
            "Option 1",
            "Option 2",
            "Option 3",
            "Option 4",
            "Option 5",
            "Option 6",
            "Option 7",
            "Option 8",
            "Option 9",
            "Option 10",
            "Option 11",
            "Option 12",
            "Option 13",
            "Option 14",
            "Option 15",
            "Option 16",
            "Option 17",
            "Option 18",
            "Option 19",
            "Option 20",
            "Option 21",
            "Option 22",
            "Option 23",
            "Option 24",
            "Option 25",
            "Option 26",
            "Option 27",
            "Option 28",
            "Option 29",
            "Option 30",
            "Option 31",
            "Option 32",
            "Option 33",
            "Option 34",
            "Option 35",
            "Option 36",
            "Option 37",
            "Option 38",
            "Option 39",
            "Option 40",
            "Option 41",
            "Option 42",
            "Option 43",
            "Option 44",
            "Option 45",
            "Option 46",
            "Option 47",
            "Option 48",
            "Option 49",
            "Option 50",
            "Option 51",
            "Option 52",
            "Option 53",
            "Option 54",
            "Option 55",
            "Option 56",
            "Option 57",
            "Option 58",
            "Option 59",
            "Option 60",
            "Option 61",
            "Option 62",
            "Option 63",
            "Option 64",
            "Option 65",
            "Option 66",
            "Option 67",
            "Option 68",
            "Option 69",
            "Option 70",
            "Option 71",
            "Option 72",
            "Option 73",
            "Option 74",
            "Option 75",
            "Option 76",
            "Option 77",
            "Option 78",
            "Option 79",
            "Option 80",
            "Option 81",
            "Option 82",
            "Option 83",
            "Option 84",
            "Option 85",
            "Option 86",
            "Option 87",
            "Option 88",
            "Option 89",
            "Option 90",
            "Option 91",
            "Option 92",
            "Option 93",
            "Option 94",
            "Option 95",
            "Option 96",
            "Option 97",
            "Option 98",
            "Option 99",
            "Option 100",
            "Option 101",
            "Option 102",
            "Option 103",
            "Option 104",
            "Option 105",
            "Option 106",
            "Option 107",
            "Option 108",
            "Option 109",
            "Option 110",
            "Option 111",
            "Option 112",
            "Option 113",
            "Option 114",
            "Option 115",
            "Option 116",
            "Option 117",
            "Option 118",
            "Option 119",
            "Option 120",
            "Option 121",
            "Option 122",
            "Option 123",
            "Option 124",
            "Option 125",
            "Option 126",
            "Option 127",
            "Option 128",
            "Option 129",
            "Option 130",
            "Option 131",
            "Option 132",
            "Option 133",
            "Option 134",
            "Option 135",
            "Option 136",
            "Option 137",
            "Option 138",
            "Option 139",
            "Option 140",
            "Option 141",
            "Option 142",
            "Option 143",
            "Option 144",
            "Option 145",
            "Option 146",
            "Option 147",
            "Option 148",
            "Option 149",
            "Option 150",
            "Option 151",
            "Option 152",
            "Option 153",
            "Option 154",
            "Option 155",
            "Option 156",
            "Option 157",
            "Option 158",
            "Option 159",
            "Option 160",
            "Option 161",
            "Option 162",
            "Option 163",
            "Option 164",
            "Option 165",
            "Option 166",
            "Option 167",
            "Option 168",
            "Option 169",
            "Option 170",
            "Option 171",
            "Option 172",
            "Option 173",
            "Option 174",
            "Option 175",
            "Option 176",
            "Option 177",
            "Option 178",
            "Option 179",
            "Option 180",
            "Option 181",
            "Option 182",
            "Option 183",
            "Option 184",
            "Option 185",
            "Option 186",
            "Option 187",
            "Option 188",
            "Option 189",
            "Option 190",
            "Option 191",
            "Option 192",
            "Option 193",
            "Option 194",
            "Option 195",
            "Option 196",
            "Option 197",
            "Option 198",
            "Option 199",
            "Option 200",
            "Option 201",
            "Option 202",
            "Option 203",
            "Option 204",
            "Option 205",
            "Option 206",
            "Option 207",
            "Option 208",
            "Option 209",
            "Option 210",
            "Option 211",
            "Option 212",
            "Option 213",
            "Option 214",
            "Option 215",
            "Option 216",
            "Option 217",
            "Option 218",
            "Option 219",
            "Option 220",
            "Option 221",
            "Option 222",
            "Option 223",
            "Option 224",
            "Option 225",
            "Option 226",
            "Option 227",
            "Option 228",
            "Option 229",
            "Option 230",
            "Option 231",
            "Option 232",
            "Option 233",
            "Option 234",
            "Option 235",
            "Option 236",
            "Option 237",
            "Option 238",
            "Option 239",
            "Option 240",
            "Option 241",
            "Option 242",
            "Option 243",
            "Option 244",
            "Option 245",
            "Option 246",
            "Option 247",
            "Option 248",
            "Option 249",
            "Option 250",
            "Option 251",
            "Option 252",
            "Option 253",
            "Option 254",
            "Option 255",
            "Option 256",
            "Option 257",
            "Option 258",
            "Option 259",
            "Option 260",
            "Option 261",
            "Option 262",
            "Option 263",
            "Option 264",
            "Option 265",
            "Option 266",
            "Option 267",
            "Option 268",
            "Option 269",
            "Option 270",
            "Option 271",
            "Option 272",
            "Option 273",
            "Option 274",
            "Option 275",
            "Option 276",
            "Option 277",
            "Option 278",
            "Option 279",
            "Option 280",
            "Option 281",
            "Option 282",
            "Option 283",
            "Option 284",
            "Option 285",
            "Option 286",
            "Option 287",
            "Option 288",
            "Option 289",
            "Option 290",
            "Option 291",
            "Option 292",
            "Option 293",
            "Option 294",
            "Option 295",
            "Option 296",
            "Option 297",
            "Option 298",
            "Option 299",
            "Option 300",
            "Option 301",
            "Option 302",
            "Option 303",
            "Option 304",
            "Option 305",
            "Option 306",
            "Option 307",
            "Option 308",
            "Option 309",
            "Option 310",
            "Option 311",
            "Option 312",
            "Option 313",
            "Option 314",
            "Option 315",
            "Option 316",
            "Option 317",
            "Option 318",
            "Option 319",
            "Option 320",
            "Option 321",
            "Option 322",
            "Option 323",
            "Option 324",
            "Option 325",
            "Option 326",
            "Option 327",
            "Option 328",
            "Option 329",
            "Option 330",
            "Option 331",
            "Option 332",
            "Option 333",
            "Option 334",
            "Option 335",
            "Option 336",
            "Option 337",
            "Option 338",
            "Option 339",
            "Option 340",
            "Option 341",
            "Option 342",
            "Option 343",
            "Option 344",
            "Option 345",
            "Option 346",
            "Option 347",
            "Option 348",
            "Option 349",
            "Option 350",
            "Option 351",
            "Option 352",
            "Option 353",
            "Option 354",
            "Option 355",
            "Option 356",
            "Option 357",
            "Option 358",
            "Option 359",
            "Option 360",
            "Option 361",
            "Option 362",
            "Option 363",
            "Option 364",
            "Option 365",
            "Option 366",
            "Option 367",
            "Option 368",
            "Option 369",
            "Option 370",
            "Option 371",
            "Option 372",
            "Option 373",
            "Option 374",
            "Option 375",
            "Option 376",
            "Option 377",
            "Option 378",
            "Option 379",
            "Option 380",
            "Option 381",
            "Option 382",
            "Option 383",
            "Option 384",
            "Option 385",
            "Option 386",
            "Option 387",
            "Option 388",
            "Option 389",
            "Option 390",
            "Option 391",
            "Option 392",
            "Option 393",
            "Option 394",
            "Option 395",
            "Option 396",
            "Option 397",
            "Option 398",
            "Option 399",
            "Option 400",
            "Option 401",
            "Option 402",
            "Option 403",
            "Option 404",
            "Option 405",
            "Option 406",
            "Option 407",
            "Option 408",
            "Option 409",
            "Option 410",
            "Option 411",
            "Option 412",
            "Option 413",
            "Option 414",
            "Option 415",
            "Option 416",
            "Option 417",
            "Option 418",
            "Option 419",
            "Option 420",
            "Option 421",
            "Option 422",
            "Option 423",
            "Option 424",
            "Option 425",
            "Option 426",
            "Option 427",
            "Option 428",
            "Option 429",
            "Option 430",
            "Option 431",
            "Option 432",
            "Option 433",
            "Option 434",
            "Option 435",
            "Option 436",
            "Option 437",
            "Option 438",
            "Option 439",
            "Option 440",
            "Option 441",
            "Option 442",
            "Option 443",
            "Option 444",
            "Option 445",
            "Option 446",
            "Option 447",
            "Option 448",
            "Option 449",
            "Option 450",
            "Option 451",
            "Option 452",
            "Option 453",
            "Option 454",
            "Option 455",
            "Option 456",
            "Option 457",
            "Option 458",
            "Option 459",
            "Option 460",
            "Option 461",
            "Option 462",
            "Option 463",
            "Option 464",
            "Option 465",
            "Option 466",
            "Option 467",
            "Option 468",
            "Option 469",
            "Option 470",
            "Option 471",
            "Option 472",
            "Option 473",
            "Option 474",
            "Option 475",
            "Option 476",
            "Option 477",
            "Option 478",
            "Option 479",
            "Option 480",
            "Option 481",
            "Option 482",
            "Option 483",
            "Option 484",
            "Option 485",
            "Option 486",
            "Option 487",
            "Option 488",
            "Option 489",
            "Option 490",
            "Option 491",
            "Option 492",
            "Option 493",
            "Option 494",
            "Option 495",
            "Option 496",
            "Option 497",
            "Option 498",
            "Option 499",
            "Option 500",
            "Option 501",
            "Option 502",
            "Option 503",
            "Option 504",
            "Option 505",
            "Option 506",
            "Option 507",
            "Option 508",
            "Option 509",
            "Option 510",
            "Option 511",
            "Option 512",
            "Option 513",
            "Option 514",
            "Option 515",
            "Option 516",
            "Option 517",
            "Option 518",
            "Option 519",
            "Option 520",
            "Option 521",
            "Option 522",
            "Option 523",
            "Option 524",
            "Option 525",
            "Option 526",
            "Option 527",
            "Option 528",
            "Option 529",
            "Option 530",
            "Option 531",
            "Option 532",
            "Option 533",
            "Option 534",
            "Option 535",
            "Option 536",
            "Option 537",
            "Option 538",
            "Option 539",
            "Option 540",
            "Option 541",
            "Option 542",
            "Option 543",
            "Option 544",
            "Option 545",
            "Option 546",
            "Option 547",
            "Option 548",
            "Option 549",
            "Option 550",
            "Option 551",
            "Option 552",
            "Option 553",
            "Option 554",
            "Option 555",
            "Option 556",
            "Option 557",
            "Option 558",
            "Option 559",
            "Option 560",
            "Option 561",
            "Option 562",
            "Option 563",
            "Option 564",
            "Option 565",
            "Option 566",
            "Option 567",
            "Option 568",
            "Option 569",
            "Option 570",
            "Option 571",
            "Option 572",
            "Option 573",
            "Option 574",
            "Option 575",
            "Option 576",
            "Option 577",
            "Option 578",
            "Option 579",
            "Option 580",
            "Option 581",
            "Option 582",
            "Option 583",
            "Option 584",
            "Option 585",
            "Option 586",
            "Option 587",
            "Option 588",
            "Option 589",
            "Option 590",
            "Option 591",
            "Option 592",
            "Option 593",
            "Option 594",
            "Option 595",
            "Option 596",
            "Option 597",
            "Option 598",
            "Option 599",
            "Option 600",
            "Option 601",
            "Option 602",
            "Option 603",
            "Option 604",
            "Option 605",
            "Option 606",
            "Option 607",
            "Option 608",
            "Option 609",
            "Option 610",
            "Option 611",
            "Option 612",
            "Option 613",
            "Option 614",
            "Option 615",
            "Option 616",
            "Option 617",
            "Option 618",
            "Option 619",
            "Option 620",
            "Option 621",
            "Option 622",
            "Option 623",
            "Option 624",
            "Option 625",
            "Option 626",
            "Option 627",
            "Option 628",
            "Option 629",
            "Option 630",
            "Option 631",
            "Option 632",
            "Option 633",
            "Option 634",
            "Option 635",
            "Option 636",
            "Option 637",
            "Option 638",
            "Option 639",
            "Option 640",
            "Option 641",
            "Option 642",
            "Option 643",
            "Option 644",
            "Option 645",
            "Option 646",
            "Option 647",
            "Option 648",
            "Option 649",
            "Option 650",
            "Option 651",
            "Option 652",
            "Option 653",
            "Option 654",
            "Option 655",
            "Option 656",
            "Option 657",
            "Option 658",
            "Option 659",
            "Option 660",
            "Option 661",
            "Option 662",
            "Option 663",
            "Option 664",
            "Option 665",
            "Option 666",
            "Option 667",
            "Option 668",
            "Option 669",
            "Option 670",
            "Option 671",
            "Option 672",
            "Option 673",
            "Option 674",
            "Option 675",
            "Option 676",
            "Option 677",
            "Option 678",
            "Option 679",
            "Option 680",
            "Option 681",
            "Option 682",
            "Option 683",
            "Option 684",
            "Option 685",
            "Option 686",
            "Option 687",
            "Option 688",
            "Option 689",
            "Option 690",
            "Option 691",
            "Option 692",
            "Option 693",
            "Option 694",
            "Option 695",
            "Option 696",
            "Option 697",
            "Option 698",
            "Option 699",
            "Option 700",
            "Option 701",
            "Option 702",
            "Option 703",
            "Option 704",
            "Option 705",
            "Option 706",
            "Option 707",
            "Option 708",
            "Option 709",
            "Option 710",
            "Option 711",
            "Option 712",
            "Option 713",
            "Option 714",
            "Option 715",
            "Option 716",
            "Option 717",
            "Option 718",
            "Option 719",
            "Option 720",
            "Option 721",
            "Option 722",
            "Option 723",
            "Option 724",
            "Option 725",
            "Option 726",
            "Option 727",
            "Option 728",
            "Option 729",
            "Option 730",
            "Option 731",
            "Option 732",
            "Option 733",
            "Option 734",
            "Option 735",
            "Option 736",
            "Option 737",
            "Option 738",
            "Option 739",
            "Option 740",
            "Option 741",
            "Option 742",
            "Option 743",
            "Option 744",
            "Option 745",
            "Option 746",
            "Option 747",
            "Option 748",
            "Option 749",
            "Option 750",
            "Option 751",
            "Option 752",
            "Option 753",
            "Option 754",
            "Option 755",
            "Option 756",
            "Option 757",
            "Option 758",
            "Option 759",
            "Option 760",
            "Option 761",
            "Option 762",
            "Option 763",
            "Option 764",
            "Option 765",
            "Option 766",
            "Option 767",
            "Option 768",
            "Option 769",
            "Option 770",
            "Option 771",
            "Option 772",
            "Option 773",
            "Option 774",
            "Option 775",
            "Option 776",
            "Option 777",
            "Option 778",
            "Option 779",
            "Option 780",
            "Option 781",
            "Option 782",
            "Option 783",
            "Option 784",
            "Option 785",
            "Option 786",
            "Option 787",
            "Option 788",
            "Option 789",
            "Option 790",
            "Option 791",
            "Option 792",
            "Option 793",
            "Option 794",
            "Option 795",
            "Option 796",
            "Option 797",
            "Option 798",
            "Option 799",
            "Option 800",
            "Option 801",
            "Option 802",
            "Option 803",
            "Option 804",
            "Option 805",
            "Option 806",
            "Option 807",
            "Option 808",
            "Option 809",
            "Option 810",
            "Option 811",
            "Option 812",
            "Option 813",
            "Option 814",
            "Option 815",
            "Option 816",
            "Option 817",
            "Option 818",
            "Option 819",
            "Option 820",
            "Option 821",
            "Option 822",
            "Option 823",
            "Option 824",
            "Option 825",
            "Option 826",
            "Option 827",
            "Option 828",
            "Option 829",
            "Option 830",
            "Option 831",
            "Option 832",
            "Option 833",
            "Option 834",
            "Option 835",
            "Option 836",
            "Option 837",
            "Option 838",
            "Option 839",
            "Option 840",
            "Option 841",
            "Option 842",
            "Option 843",
            "Option 844",
            "Option 845",
            "Option 846",
            "Option 847",
            "Option 848",
            "Option 849",
            "Option 850",
            "Option 851",
            "Option 852",
            "Option 853",
            "Option 854",
            "Option 855",
            "Option 856",
            "Option 857",
            "Option 858",
            "Option 859",
            "Option 860",
            "Option 861",
            "Option 862",
            "Option 863",
            "Option 864",
            "Option 865",
            "Option 866",
            "Option 867",
            "Option 868",
            "Option 869",
            "Option 870",
            "Option 871",
            "Option 872",
            "Option 873",
            "Option 874",
            "Option 875",
            "Option 876",
            "Option 877",
            "Option 878",
            "Option 879",
            "Option 880",
            "Option 881",
            "Option 882",
            "Option 883",
            "Option 884",
            "Option 885",
            "Option 886",
            "Option 887",
            "Option 888",
            "Option 889",
            "Option 890",
            "Option 891",
            "Option 892",
            "Option 893",
            "Option 894",
            "Option 895",
            "Option 896",
            "Option 897",
            "Option 898",
            "Option 899",
            "Option 900",
            "Option 901",
            "Option 902",
            "Option 903",
            "Option 904",
            "Option 905",
            "Option 906",
            "Option 907",
            "Option 908",
            "Option 909",
            "Option 910",
            "Option 911",
            "Option 912",
            "Option 913",
            "Option 914",
            "Option 915",
            "Option 916",
            "Option 917",
            "Option 918",
            "Option 919",
            "Option 920",
            "Option 921",
            "Option 922",
            "Option 923",
            "Option 924",
            "Option 925",
            "Option 926",
            "Option 927",
            "Option 928",
            "Option 929",
            "Option 930",
            "Option 931",
            "Option 932",
            "Option 933",
            "Option 934",
            "Option 935",
            "Option 936",
            "Option 937",
            "Option 938",
            "Option 939",
            "Option 940",
            "Option 941",
            "Option 942",
            "Option 943",
            "Option 944",
            "Option 945",
            "Option 946",
            "Option 947",
            "Option 948",
            "Option 949",
            "Option 950",
            "Option 951",
            "Option 952",
            "Option 953",
            "Option 954",
            "Option 955",
            "Option 956",
            "Option 957",
            "Option 958",
            "Option 959",
            "Option 960",
            "Option 961",
            "Option 962",
            "Option 963",
            "Option 964",
            "Option 965",
            "Option 966",
            "Option 967",
            "Option 968",
            "Option 969",
            "Option 970",
            "Option 971",
            "Option 972",
            "Option 973",
            "Option 974",
            "Option 975",
            "Option 976",
            "Option 977",
            "Option 978",
            "Option 979",
            "Option 980",
            "Option 981",
            "Option 982",
            "Option 983",
            "Option 984",
            "Option 985",
            "Option 986",
            "Option 987",
            "Option 988",
            "Option 989",
            "Option 990",
            "Option 991",
            "Option 992",
            "Option 993",
            "Option 994",
            "Option 995",
            "Option 996",
            "Option 997",
            "Option 998",
            "Option 999"
          ]
        ]
      }
    },
    "input_order": {
      "required": ["option"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsLongComboDropdown",
    "display_name": "Long Combo Dropdown",
    "description": "A long combo dropdown",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true
  },
  "DevToolsNodeWithOptionalInput": {
    "input": {
      "required": {
        "required_input": ["IMAGE"]
      },
      "optional": {
        "optional_input": [
          "IMAGE",
          {
            "default": null
          }
        ]
      }
    },
    "input_order": {
      "required": ["required_input"],
      "optional": ["optional_input"]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "DevToolsNodeWithOptionalInput",
    "display_name": "Node With Optional Input",
    "description": "A node with an optional input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithOptionalComboInput": {
    "input": {
      "optional": {
        "optional_combo_input": [
          [
            "Random Unique Option 1754025221.090168",
            "Random Unique Option 1754025221.090173",
            "Random Unique Option 1754025221.0901737",
            "Random Unique Option 1754025221.0901742",
            "Random Unique Option 1754025221.0901747",
            "Random Unique Option 1754025221.0901752",
            "Random Unique Option 1754025221.0901756",
            "Random Unique Option 1754025221.0901763"
          ],
          {
            "default": null
          }
        ]
      }
    },
    "input_order": {
      "optional": ["optional_combo_input"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "DevToolsNodeWithOptionalComboInput",
    "display_name": "Node With Optional Combo Input",
    "description": "A node with an optional combo input that returns unique values every time INPUT_TYPES is called",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithOnlyOptionalInput": {
    "input": {
      "optional": {
        "text": [
          "STRING",
          {
            "multiline": true,
            "dynamicPrompts": true
          }
        ],
        "clip": ["CLIP", {}]
      }
    },
    "input_order": {
      "optional": ["text", "clip"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithOnlyOptionalInput",
    "display_name": "Node With Only Optional Input",
    "description": "A node with only optional input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithOutputList": {
    "input": {
      "required": {}
    },
    "input_order": {
      "required": []
    },
    "output": ["INT", "INT"],
    "output_is_list": [false, true],
    "output_name": ["INTEGER OUTPUT", "INTEGER LIST OUTPUT"],
    "name": "DevToolsNodeWithOutputList",
    "display_name": "Node With Output List",
    "description": "A node with an output list",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithForceInput": {
    "input": {
      "required": {
        "int_input": [
          "INT",
          {
            "forceInput": true
          }
        ],
        "int_input_widget": [
          "INT",
          {
            "default": 1
          }
        ]
      },
      "optional": {
        "float_input": [
          "FLOAT",
          {
            "forceInput": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["int_input", "int_input_widget"],
      "optional": ["float_input"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithForceInput",
    "display_name": "Node With Force Input",
    "description": "A node with a forced input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true
  },
  "DevToolsNodeWithDefaultInput": {
    "input": {
      "required": {
        "int_input": [
          "INT",
          {
            "defaultInput": true
          }
        ],
        "int_input_widget": [
          "INT",
          {
            "default": 1
          }
        ]
      },
      "optional": {
        "float_input": [
          "FLOAT",
          {
            "defaultInput": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["int_input", "int_input_widget"],
      "optional": ["float_input"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithDefaultInput",
    "display_name": "Node With Default Input",
    "description": "A node with a default input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true
  },
  "DevToolsNodeWithStringInput": {
    "input": {
      "required": {
        "string_input": ["STRING"]
      }
    },
    "input_order": {
      "required": ["string_input"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithStringInput",
    "display_name": "Node With String Input",
    "description": "A node with a string input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithUnionInput": {
    "input": {
      "optional": {
        "string_or_int_input": ["STRING,INT"],
        "string_input": [
          "STRING",
          {
            "forceInput": true
          }
        ],
        "int_input": [
          "INT",
          {
            "forceInput": true
          }
        ]
      }
    },
    "input_order": {
      "optional": ["string_or_int_input", "string_input", "int_input"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithUnionInput",
    "display_name": "Node With Union Input",
    "description": "A node with a union input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true
  },
  "DevToolsSimpleSlider": {
    "input": {
      "required": {
        "value": [
          "FLOAT",
          {
            "display": "slider",
            "default": 0.5,
            "min": 0,
            "max": 1,
            "step": 0.001
          }
        ]
      }
    },
    "input_order": {
      "required": ["value"]
    },
    "output": ["FLOAT"],
    "output_is_list": [false],
    "output_name": ["FLOAT"],
    "name": "DevToolsSimpleSlider",
    "display_name": "Simple Slider",
    "description": "",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithSeedInput": {
    "input": {
      "required": {
        "seed": [
          "INT",
          {
            "default": 0
          }
        ]
      }
    },
    "input_order": {
      "required": ["seed"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithSeedInput",
    "display_name": "Node With Seed Input",
    "description": "A node with a seed input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true
  },
  "DevToolsObjectPatchNode": {
    "input": {
      "required": {
        "model": ["MODEL"],
        "target_module": [
          "STRING",
          {
            "multiline": true
          }
        ]
      },
      "optional": {
        "dummy_float": [
          "FLOAT",
          {
            "default": 0
          }
        ]
      }
    },
    "input_order": {
      "required": ["model", "target_module"],
      "optional": ["dummy_float"]
    },
    "output": ["MODEL"],
    "output_is_list": [false],
    "output_name": ["MODEL"],
    "name": "DevToolsObjectPatchNode",
    "display_name": "Object Patch Node",
    "description": "A node that applies an object patch",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithBooleanInput": {
    "input": {
      "required": {
        "boolean_input": ["BOOLEAN"]
      }
    },
    "input_order": {
      "required": ["boolean_input"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithBooleanInput",
    "display_name": "Node With Boolean Input",
    "description": "A node with a boolean input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsRemoteWidgetNode": {
    "input": {
      "required": {
        "remote_widget_value": [
          "COMBO",
          {
            "remote": {
              "route": "/api/models/checkpoints"
            }
          }
        ]
      }
    },
    "input_order": {
      "required": ["remote_widget_value"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "DevToolsRemoteWidgetNode",
    "display_name": "Remote Widget Node",
    "description": "A node that lazily fetches options from a remote endpoint",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsRemoteWidgetNodeWithParams": {
    "input": {
      "required": {
        "remote_widget_value": [
          "COMBO",
          {
            "remote": {
              "route": "/api/models/checkpoints",
              "query_params": {
                "sort": "true"
              }
            }
          }
        ]
      }
    },
    "input_order": {
      "required": ["remote_widget_value"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "DevToolsRemoteWidgetNodeWithParams",
    "display_name": "Remote Widget Node With Sort Query Param",
    "description": "A node that lazily fetches options from a remote endpoint with query params",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsRemoteWidgetNodeWithRefresh": {
    "input": {
      "required": {
        "remote_widget_value": [
          "COMBO",
          {
            "remote": {
              "route": "/api/models/checkpoints",
              "refresh": 300,
              "max_retries": 10,
              "timeout": 256
            }
          }
        ]
      }
    },
    "input_order": {
      "required": ["remote_widget_value"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "DevToolsRemoteWidgetNodeWithRefresh",
    "display_name": "Remote Widget Node With 300ms Refresh",
    "description": "A node that lazily fetches options from a remote endpoint and refresh the options every 300 ms",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsRemoteWidgetNodeWithRefreshButton": {
    "input": {
      "required": {
        "remote_widget_value": [
          "COMBO",
          {
            "remote": {
              "route": "/api/models/checkpoints",
              "refresh_button": true
            }
          }
        ]
      }
    },
    "input_order": {
      "required": ["remote_widget_value"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "DevToolsRemoteWidgetNodeWithRefreshButton",
    "display_name": "Remote Widget Node With Refresh Button",
    "description": "A node that lazily fetches options from a remote endpoint and has a refresh button to manually reload options",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsRemoteWidgetNodeWithControlAfterRefresh": {
    "input": {
      "required": {
        "remote_widget_value": [
          "COMBO",
          {
            "remote": {
              "route": "/api/models/checkpoints",
              "refresh_button": true,
              "control_after_refresh": "first"
            }
          }
        ]
      }
    },
    "input_order": {
      "required": ["remote_widget_value"]
    },
    "output": ["STRING"],
    "output_is_list": [false],
    "output_name": ["STRING"],
    "name": "DevToolsRemoteWidgetNodeWithControlAfterRefresh",
    "display_name": "Remote Widget Node With Refresh Button and Control After Refresh",
    "description": "A node that lazily fetches options from a remote endpoint and has a refresh button to manually reload options and select the first option on refresh",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsNodeWithOutputCombo": {
    "input": {
      "required": {
        "subset_options": [
          ["A", "B"],
          {
            "forceInput": true
          }
        ],
        "subset_options_v2": [
          "COMBO",
          {
            "options": ["A", "B"],
            "forceInput": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["subset_options", "subset_options_v2"]
    },
    "output": [["A", "B", "C"]],
    "output_is_list": [false],
    "output_name": [["A", "B", "C"]],
    "name": "DevToolsNodeWithOutputCombo",
    "display_name": "Node With Output Combo",
    "description": "A node that outputs a combo type",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsMultiSelectNode": {
    "input": {
      "required": {
        "foo": [
          "COMBO",
          {
            "options": ["A", "B", "C"],
            "multi_select": {
              "placeholder": "Choose foos",
              "chip": true
            }
          }
        ]
      }
    },
    "input_order": {
      "required": ["foo"]
    },
    "output": ["STRING"],
    "output_is_list": [true],
    "output_name": ["STRING"],
    "name": "DevToolsMultiSelectNode",
    "display_name": "Multi Select Node",
    "description": "A node that outputs a multi select type",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "DevToolsLoadAnimatedImageTest": {
    "input": {
      "required": {
        "image": [
          [],
          {
            "animated_image_upload": true
          }
        ]
      }
    },
    "input_order": {
      "required": ["image"]
    },
    "output": ["IMAGE", "MASK"],
    "output_is_list": [false, false],
    "output_name": ["IMAGE", "MASK"],
    "name": "DevToolsLoadAnimatedImageTest",
    "display_name": "Load Animated Image",
    "description": "",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "image",
    "output_node": false
  },
  "DevToolsNodeWithValidation": {
    "input": {
      "required": {
        "int_input": ["INT"]
      }
    },
    "input_order": {
      "required": ["int_input"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "DevToolsNodeWithValidation",
    "display_name": "Node With Validation",
    "description": "A node that validates an input",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": true
  },
  "DevToolsNodeWithV2ComboInput": {
    "input": {
      "required": {
        "combo_input": [
          "COMBO",
          {
            "options": ["A", "B"]
          }
        ]
      }
    },
    "input_order": {
      "required": ["combo_input"]
    },
    "output": ["COMBO"],
    "output_is_list": [false],
    "output_name": ["COMBO"],
    "name": "DevToolsNodeWithV2ComboInput",
    "display_name": "Node With V2 Combo Input",
    "description": "A node that outputs a combo type that adheres to the v2 combo input spec",
    "python_module": "custom_nodes.ComfyUI_devtools",
    "category": "DevTools",
    "output_node": false
  },
  "Example": {
    "input": {
      "required": {
        "image": ["IMAGE"],
        "int_field": [
          "INT",
          {
            "default": 0,
            "min": 0,
            "max": 4096,
            "step": 64,
            "display": "number",
            "lazy": true
          }
        ],
        "float_field": [
          "FLOAT",
          {
            "default": 1,
            "min": 0,
            "max": 10,
            "step": 0.01,
            "round": 0.001,
            "display": "number",
            "lazy": true
          }
        ],
        "print_to_screen": [["enable", "disable"]],
        "string_field": [
          "STRING",
          {
            "multiline": false,
            "default": "Hello World!",
            "lazy": true
          }
        ]
      }
    },
    "input_order": {
      "required": [
        "image",
        "int_field",
        "float_field",
        "print_to_screen",
        "string_field"
      ]
    },
    "output": ["IMAGE"],
    "output_is_list": [false],
    "output_name": ["IMAGE"],
    "name": "Example",
    "display_name": "Example Node",
    "description": "",
    "python_module": "custom_nodes.example_node",
    "category": "Example",
    "output_node": false
  },
  "SaveImageWebsocket": {
    "input": {
      "required": {
        "images": ["IMAGE"]
      }
    },
    "input_order": {
      "required": ["images"]
    },
    "output": [],
    "output_is_list": [],
    "output_name": [],
    "name": "SaveImageWebsocket",
    "display_name": "SaveImageWebsocket",
    "description": "",
    "python_module": "custom_nodes.websocket_image_save",
    "category": "api/image",
    "output_node": true
  }
}
```

### 9. Request URL: /api/userdata/comfy.templates.json. Status: 404 Not Found

### 10. Request URL: /api/experiment/models. Status: 200 OK

```json
[
  {
    "name": "checkpoints",
    "folders": [
      "/home/jang/ComfyUI/models/checkpoints",
      "/home/jang/ComfyUI/output/checkpoints"
    ]
  },
  {
    "name": "loras",
    "folders": [
      "/home/jang/ComfyUI/models/loras",
      "/home/jang/ComfyUI/output/loras"
    ]
  },
  {
    "name": "vae",
    "folders": [
      "/home/jang/ComfyUI/models/vae",
      "/home/jang/ComfyUI/output/vae"
    ]
  },
  {
    "name": "text_encoders",
    "folders": [
      "/home/jang/ComfyUI/models/text_encoders",
      "/home/jang/ComfyUI/models/clip",
      "/home/jang/ComfyUI/output/clip"
    ]
  },
  {
    "name": "diffusion_models",
    "folders": [
      "/home/jang/ComfyUI/models/unet",
      "/home/jang/ComfyUI/models/diffusion_models",
      "/home/jang/ComfyUI/output/diffusion_models"
    ]
  },
  {
    "name": "clip_vision",
    "folders": ["/home/jang/ComfyUI/models/clip_vision"]
  },
  {
    "name": "style_models",
    "folders": ["/home/jang/ComfyUI/models/style_models"]
  },
  {
    "name": "embeddings",
    "folders": ["/home/jang/ComfyUI/models/embeddings"]
  },
  {
    "name": "diffusers",
    "folders": ["/home/jang/ComfyUI/models/diffusers"]
  },
  {
    "name": "vae_approx",
    "folders": ["/home/jang/ComfyUI/models/vae_approx"]
  },
  {
    "name": "controlnet",
    "folders": [
      "/home/jang/ComfyUI/models/controlnet",
      "/home/jang/ComfyUI/models/t2i_adapter"
    ]
  },
  {
    "name": "gligen",
    "folders": ["/home/jang/ComfyUI/models/gligen"]
  },
  {
    "name": "upscale_models",
    "folders": ["/home/jang/ComfyUI/models/upscale_models"]
  },
  {
    "name": "hypernetworks",
    "folders": ["/home/jang/ComfyUI/models/hypernetworks"]
  },
  {
    "name": "photomaker",
    "folders": ["/home/jang/ComfyUI/models/photomaker"]
  },
  {
    "name": "classifiers",
    "folders": ["/home/jang/ComfyUI/models/classifiers"]
  }
]
```

## Thao tác
### Phần lặp lại promt, queue, history.

Được xác định khi kết nối với server k tốt.

#### Request URL: /api/prompt. Status: 200 OK

```json
{
  "exec_info": {
    "queue_remaining": 0
  }
}
```

#### Request URL: /api/queue. Status: 200 OK

```json
{
  "queue_running": [],
  "queue_pending": []
}
```

#### Request URL: /api/history.

```
Raw query string: max_items=64
max_items: 64
```

Status: 200 OK: {}

### Khi nhận RUN mà không có Checkpoint:

Log tại CLI Core:

```bash
got prompt
Failed to validate prompt for output 9:
* CheckpointLoaderSimple 4:
  - Value not in list: ckpt_name: 'v1-5-pruned-emaonly-fp16.safetensors' not in []
Output will be ignored
invalid prompt: {'type': 'prompt_outputs_failed_validation', 'message': 'Prompt outputs failed validation', 'details': '', 'extra_info': {}}
```

#### Lệnh **POST** được gửi đi: Request URL: `/api/prompt`

header 19 mục(nhiều nhất trong cách lệnh gửi):

```
Accept: */*
Accept-encoding: gzip, deflate, br, zstd
Accept-language: vi,en-US;q=0.9,en;q=0.8,fr-FR;q=0.7,fr;q=0.6
Cache-control: max-age=0
Comfy-user:
Connection: close
Content-length: 3992
Content-type: application/json
Cookie: i18n_redirected=en
Host: localhost:5173
Origin: http://localhost:5173
Referer: http://localhost:5173/
Sec-ch-ua: "Google Chrome";v="137", "Chromium";v="137", "Not/A)Brand";v="24"
Sec-ch-ua-mobile: ?0
Sec-ch-ua-platform: "Windows"
Sec-fetch-dest: empty
Sec-fetch-mode: cors
Sec-fetch-site: same-origin
User-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
```

```json
{
  "client_id": "",
  "prompt": {
    "3": {
      "inputs": {
        "seed": 156680208700286,
        "steps": 20,
        "cfg": 8,
        "sampler_name": "euler",
        "scheduler": "normal",
        "denoise": 1,
        "model": ["4", 0],
        "positive": ["6", 0],
        "negative": ["7", 0],
        "latent_image": ["5", 0]
      },
      "class_type": "KSampler",
      "_meta": {
        "title": "KSampler"
      }
    },
    "4": {
      "inputs": {
        "ckpt_name": "v1-5-pruned-emaonly-fp16.safetensors"
      },
      "class_type": "CheckpointLoaderSimple",
      "_meta": {
        "title": "Load Checkpoint"
      }
    },
    "5": {
      "inputs": {
        "width": 512,
        "height": 512,
        "batch_size": 1
      },
      "class_type": "EmptyLatentImage",
      "_meta": {
        "title": "Empty Latent Image"
      }
    },
    "6": {
      "inputs": {
        "text": "beautiful scenery nature glass bottle landscape, , purple galaxy bottle,",
        "clip": ["4", 1]
      },
      "class_type": "CLIPTextEncode",
      "_meta": {
        "title": "CLIP Text Encode (Prompt)"
      }
    },
    "7": {
      "inputs": {
        "text": "text, watermark",
        "clip": ["4", 1]
      },
      "class_type": "CLIPTextEncode",
      "_meta": {
        "title": "CLIP Text Encode (Prompt)"
      }
    },
    "8": {
      "inputs": {
        "samples": ["3", 0],
        "vae": ["4", 2]
      },
      "class_type": "VAEDecode",
      "_meta": {
        "title": "VAE Decode"
      }
    },
    "9": {
      "inputs": {
        "filename_prefix": "ComfyUI",
        "images": ["8", 0]
      },
      "class_type": "SaveImage",
      "_meta": {
        "title": "Save Image"
      }
    }
  },
  "extra_data": {
    "extra_pnginfo": {
      "workflow": {
        "id": "03935b99-db1e-47e0-8eff-e48cbde9b46b",
        "revision": 0,
        "last_node_id": 9,
        "last_link_id": 9,
        "nodes": [
          {
            "id": 7,
            "type": "CLIPTextEncode",
            "pos": [413, 389],
            "size": [425.27801513671875, 180.6060791015625],
            "flags": {},
            "order": 3,
            "mode": 0,
            "inputs": [
              {
                "name": "clip",
                "type": "CLIP",
                "link": 5
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
              "Node name for S&R": "CLIPTextEncode"
            },
            "widgets_values": ["text, watermark"]
          },
          {
            "id": 6,
            "type": "CLIPTextEncode",
            "pos": [415, 186],
            "size": [422.84503173828125, 164.31304931640625],
            "flags": {},
            "order": 2,
            "mode": 0,
            "inputs": [
              {
                "name": "clip",
                "type": "CLIP",
                "link": 3
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
              "Node name for S&R": "CLIPTextEncode"
            },
            "widgets_values": [
              "beautiful scenery nature glass bottle landscape, , purple galaxy bottle,"
            ]
          },
          {
            "id": 5,
            "type": "EmptyLatentImage",
            "pos": [473, 609],
            "size": [315, 106],
            "flags": {},
            "order": 0,
            "mode": 0,
            "inputs": [],
            "outputs": [
              {
                "name": "LATENT",
                "type": "LATENT",
                "slot_index": 0,
                "links": [2]
              }
            ],
            "properties": {
              "Node name for S&R": "EmptyLatentImage"
            },
            "widgets_values": [512, 512, 1]
          },
          {
            "id": 3,
            "type": "KSampler",
            "pos": [863, 186],
            "size": [315, 262],
            "flags": {},
            "order": 4,
            "mode": 0,
            "inputs": [
              {
                "name": "model",
                "type": "MODEL",
                "link": 1
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
                "link": 2
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
              "Node name for S&R": "KSampler"
            },
            "widgets_values": [
              156680208700286,
              "randomize",
              20,
              8,
              "euler",
              "normal",
              1
            ]
          },
          {
            "id": 8,
            "type": "VAEDecode",
            "pos": [1209, 188],
            "size": [210, 46],
            "flags": {},
            "order": 5,
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
                "link": 8
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
              "Node name for S&R": "VAEDecode"
            },
            "widgets_values": []
          },
          {
            "id": 9,
            "type": "SaveImage",
            "pos": [1451, 189],
            "size": [210, 58],
            "flags": {},
            "order": 6,
            "mode": 0,
            "inputs": [
              {
                "name": "images",
                "type": "IMAGE",
                "link": 9
              }
            ],
            "outputs": [],
            "properties": {},
            "widgets_values": ["ComfyUI"]
          },
          {
            "id": 4,
            "type": "CheckpointLoaderSimple",
            "pos": [42, 398],
            "size": [315, 98],
            "flags": {},
            "order": 1,
            "mode": 0,
            "inputs": [],
            "outputs": [
              {
                "name": "MODEL",
                "type": "MODEL",
                "slot_index": 0,
                "links": [1]
              },
              {
                "name": "CLIP",
                "type": "CLIP",
                "slot_index": 1,
                "links": [3, 5]
              },
              {
                "name": "VAE",
                "type": "VAE",
                "slot_index": 2,
                "links": [8]
              }
            ],
            "properties": {
              "Node name for S&R": "CheckpointLoaderSimple"
            },
            "widgets_values": ["v1-5-pruned-emaonly-fp16.safetensors"]
          }
        ],
        "links": [
          [1, 4, 0, 3, 0, "MODEL"],
          [2, 5, 0, 3, 3, "LATENT"],
          [3, 4, 1, 6, 0, "CLIP"],
          [4, 6, 0, 3, 1, "CONDITIONING"],
          [5, 4, 1, 7, 0, "CLIP"],
          [6, 7, 0, 3, 2, "CONDITIONING"],
          [7, 3, 0, 8, 0, "LATENT"],
          [8, 4, 2, 8, 1, "VAE"],
          [9, 8, 0, 9, 0, "IMAGE"]
        ],
        "groups": [],
        "config": {},
        "extra": {
          "ds": {
            "scale": 1,
            "offset": [0, -236]
          },
          "frontendVersion": "1.25.3"
        },
        "version": 0.4
      }
    }
  }
}
```

#### Status: 400 Bad Request:

header:

```
Access-control-allow-headers: Content-Type, Origin, Accept, Authorization, Content-Length, X-Requested-With
Access-control-allow-methods: GET,POST,PUT,PATCH,DELETE,HEAD,OPTIONS
Connection: close
Content-length: 821
Content-type: application/json; charset=utf-8
Date: Fri, 01 Aug 2025 05:49:36 GMT
Origin: http:\\localhost:9999
Server: Python/3.12 aiohttp/3.12.15
```

```json
{
  "error": {
    "type": "prompt_outputs_failed_validation",
    "message": "Prompt outputs failed validation",
    "details": "",
    "extra_info": {}
  },
  "node_errors": {
    "4": {
      "errors": [
        {
          "type": "value_not_in_list",
          "message": "Value not in list",
          "details": "ckpt_name: 'v1-5-pruned-emaonly-fp16.safetensors' not in []",
          "extra_info": {
            "input_name": "ckpt_name",
            "input_config": [
              [],
              {
                "tooltip": "The name of the checkpoint (model) to load."
              }
            ],
            "received_value": "v1-5-pruned-emaonly-fp16.safetensors"
          }
        }
      ],
      "dependent_outputs": ["9"],
      "class_type": "CheckpointLoaderSimple"
    }
  }
}
```

### Request URL: /internal/log. Status: 200 OK. "TEXT LOG QUÁ TRÌNH TRẢ LỜI CỦA SERVER"

### Sau khi kết với server bằng HTTP API:

liên tục gửi `/ws`, nếu không nhận được sẽ bị lỗi:

```bash
 ERROR  8:04:44 PM [vite] ws proxy socket error:
Error: read ECONNRESET
    at TCP.onStreamRead (node:internal/stream_base_commons:216:20) (x9)
```

### Server Disconect Error:

```bash
 ERROR  7:54:59 PM [vite] http proxy error: /api/prompt
AggregateError [ECONNREFUSED]:
    at internalConnectMultiple (node:net:1134:18)
    at afterConnectMultiple (node:net:1715:7)

```

```bash
 ERROR  7:54:59 PM [vite] http proxy error: /api/queue
AggregateError [ECONNREFUSED]:
    at internalConnectMultiple (node:net:1134:18)
    at afterConnectMultiple (node:net:1715:7)
```

```bash
ERROR  7:54:59 PM [vite] http proxy error: /api/history?max_items=64
AggregateError [ECONNREFUSED]:
    at internalConnectMultiple (node:net:1134:18)
    at afterConnectMultiple (node:net:1715:7)
```

```bash
ERROR  7:55:02 PM [vite] ws proxy error:
AggregateError [ECONNREFUSED]:
    at internalConnectMultiple (node:net:1134:18)
    at afterConnectMultiple (node:net:1715:7)
```
