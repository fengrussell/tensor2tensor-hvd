## Tensor2Tensor supported Horovod

### Version

* [Tensor2Tensor](https://github.com/tensorflow/tensor2tensor) v1.4.4

### Usage
##### 1. To run on a machine with 4 GPUs:

```
mpirun -np 4 \
    -H localhost:4 \
    -bind-to none -map-by slot \
    -x NCCL_DEBUG=INFO -x LD_LIBRARY_PATH -x PATH \
    -mca pml ob1 -mca btl ^openib \
    -mca btl_tcp_if_include eth0 \
    hvd/t2t-hvd-trainer \
      --data_dir=~/t2t_data \
      --output_dir=~/t2t_train/mnist \
      --problems=image_mnist \
      --model=shake_shake  \
      --hparams_set=shakeshake_small \
      --train_steps=10 \
      --eval_steps=10
```
*Note: mpirun must spec the params ```-mca btl_tcp_if_include``` explicitly*


##### 2. To run on 4 machines with 4 GPUs each:


```
mpirun -np 16 \
    -H server1:4,server2:4,server3:4,server4:4 \
    -bind-to none -map-by slot \
    -x NCCL_DEBUG=INFO -x LD_LIBRARY_PATH -x PATH \
    -mca pml ob1 -mca btl ^openib \
    -mca btl_tcp_if_include eth0 \
    hvd/t2t-hvd-trainer \
      --data_dir=~/t2t_data \
      --output_dir=~/t2t_train/mnist \
      --problems=image_mnist \
      --model=shake_shake  \
      --hparams_set=shakeshake_small \
      --train_steps=10 \
      --eval_steps=10
```
