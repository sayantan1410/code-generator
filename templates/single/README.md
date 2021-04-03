[![Code-Generator](https://badgen.net/badge/Template%20by/Code-Generator/ee4c2c?labelColor=eaa700)](https://github.com/pytorch-ignite/code-generator)

# Single Model, Single Optimizer Template

This is a template generated for single model, single optimizer based training.
This template has structured like a python package to imports modules easily.

<details>
<summary>
Table of Contents
</summary>

- [Getting Started](#getting-started)
- [Training](#training)
- [PyTorch Hub](#pytorch-hub)

</details>

## Getting Started

<details>
<summary>
Detailed Directory List
</summary>

```sh
single
├── README.md
├── find_and_replace.sh
├── hubconf.py
├── requirements.txt
├── setup.py
├── single_cg
│   ├── __init__.py
│   ├── datasets.py
│   ├── engines.py
│   ├── events.py
│   ├── handlers.py
│   ├── main.py
│   ├── models.py
│   └── utils.py
└── tests
    ├── test_engines.py
    ├── test_handlers.py
    └── test_utils.py

2 directories, 16 files
```

</details>

- Install the dependencies with `pip` and install the project in `editable` mode:

  ```sh
  pip install -r requirements.txt --progress-bar off -U
  pip install -e .
  ```

> **💡 TIP**
>
> To quickly adapt to the generated code structure, there are TODOs in the files that are needed to be edited.
> [PyCharm TODO comments](https://www.jetbrains.com/help/pycharm/using-todo.html) or
> [VSCode Todo Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)
> can help you find them easily.

- Edit `datasets.py` for your custom datasets and dataloaders.
- Edit `models.py` for your custom models.
- Extend `config.py` for additional command line arguments.
- Extend `trainers.py` for your custom models' forward pass, backward pass, and evaluation.
- Extend `handlers.py` for your custom handlers. _(**OPTIONAL**)_

## Training

### Single Node, Single GPU

```sh
python main.py --verbose
```

### Single Node, Multiple GPUs

- Using `torch.distributed.launch` (preferred)

  ```sh
  python -m torch.distributed.launch \
    --nproc_per_node=2 \
    --use_env main.py \
    --backend="nccl" \
    --verbose \
  ```

- Using function spawn inside the code

  ```sh
  python main.py \
    --backend="nccl" \
    --nproc_per_node=2 \
    --verbose \
  ```

### Multiple Nodes, Multiple GPUs

Let's start training on two nodes with 2 gpus each. We assuming that master node can be connected as master, e.g. ping master.

- Execute on master node

  ```sh
  python -m torch.distributed.launch \
    --nnodes=2 \
    --nproc_per_node=2 \
    --node_rank=0 \
    --master_addr=master \
    --master_port=2222 \
    --use_env main.py \
    --backend="nccl" \
    --verbose \
  ```

- Execute on worker node

  ```sh
  python -m torch.distributed.launch \
    --nnodes=2 \
    --nproc_per_node=2 \
    --node_rank=1 \
    --master_addr=master \
    --master_port=2222 \
    --use_env main.py \
    --backend="nccl" \
    --verbose \
  ```

### Colab 8 TPUs

```sh
python main.py --verbose --backend='xla-tpu' --nproc_per_node=8
```

## PyTorch Hub

- Edit `hubconf.py` to use the custom model easily via `torch.hub.load()`.
- Add additional requirements inside `dependencies` list in `hubconf.py`.