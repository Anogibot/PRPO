# Training Guide

## Overview

DX-LLaVA training involves fine-tuning the LLaVA model with ConvNeXt vision encoder for deepfake detection tasks.

## Training Configuration

### Basic Training Command

```bash
./train_dx_llava.sh
```

### Configuration Parameters

Edit `train_dx_llava.sh` to customize:

```bash
# Model Configuration
MODEL_PATH="liuhaotian/llava-v1.5-7b"  # Base model
VISION_TOWER="hf-hub:laion/CLIP-convnext_large_d_320.laion2B-s29B-b131K-ft-soup"

# Data Configuration
TRAIN_DATA_PATH="/path/to/train_data.json"
EVAL_DATA_PATH="/path/to/eval_data.json"
OUTPUT_DIR="/path/to/output"

# Training Hyperparameters
--learning_rate 2e-5
--per_device_train_batch_size 8
--num_train_epochs 1
--binary_loss_weight 10.0
```

## Data Preparation

### Dataset Format

Your training data should be in JSON format:

```json
[
  {
    "id": "sample_001",
    "image": "images/real_face.jpg",
    "conversations": [
      {
        "from": "human",
        "value": "<image>\nAnalyze this image for signs of manipulation."
      },
      {
        "from": "gpt",
        "value": "This appears to be a genuine, unmanipulated image."
      }
    ]
  }
]
```

### Binary Classification Labels

For deepfake detection, use binary responses:
- `"real"` or `"genuine"` for authentic images
- `"fake"` or `"manipulated"` for deepfakes

## Training Process

### Stage 1: Feature Alignment (Optional)

If starting from scratch, you may want to pretrain the multimodal projector:

```bash
deepspeed --include localhost:0,1,2 llava/train/train_mem.py \
    --deepspeed ./scripts/zero3.json \
    --model_name_or_path liuhaotian/llava-v1.5-7b \
    --data_path pretrain_data.json \
    --tune_mm_mlp_adapter True \
    --output_dir ./checkpoints/dx-llava-pretrain
```

### Stage 2: Fine-tuning

Main training stage using the provided script:

```bash
./train_dx_llava.sh
```

## Monitoring Training

### WandB Integration

The training automatically logs to Weights & Biases. Key metrics to monitor:

- **Training Loss**: Overall model loss
- **Binary Loss**: Classification-specific loss
- **LM Loss**: Language modeling loss
- **Learning Rate**: Cosine schedule progression

### Evaluation During Training

The model evaluates every 200 steps on the validation set. Check:

- **Eval Loss**: Validation performance
- **Binary Accuracy**: Classification accuracy
- **Perplexity**: Language generation quality

## Advanced Configuration

### Multi-GPU Training

Adjust GPU configuration:
```bash
# For 4 GPUs
--include localhost:0,1,2,3

# For 8 GPUs
--include localhost:0,1,2,3,4,5,6,7
```

### Memory Optimization

For limited VRAM:
```bash
--per_device_train_batch_size 4    # Reduce batch size
--gradient_accumulation_steps 2    # Maintain effective batch size
--gradient_checkpointing True       # Enable checkpointing
```

### Loss Weight Tuning

Balance language modeling and classification:
```bash
--lm_loss_weight 1.0        # Language modeling weight
--binary_loss_weight 5.0    # Binary classification weight
```

## Best Practices

1. **Data Quality**: Ensure balanced dataset with equal real/fake samples
2. **Evaluation**: Use separate validation set from different domains
3. **Checkpointing**: Save model frequently with `--save_steps 1000`
4. **Monitoring**: Watch for overfitting on binary classification
5. **Convergence**: Typically converges within 1-2 epochs for fine-tuning